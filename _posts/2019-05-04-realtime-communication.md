---
layout: post
title: "요청하지도 않았는데 메시지가 온다고? - 실시간 통신"
comments: true
author: feynubrick
date: 2019-05-04
tags: [Study, CodeStates]
---

난 스마트폰을 무음으로 해놓고 산다.
내 주변 사람들은 불편해하는 것 같지만(다들 적응한것 같기도 하다), 어쨌든 나는 좋다.
무음으로 해놓는 이유는 매우 간단하다.
방해받고 싶지 않아서다.
더 장황하게는 내가 원하지도 않았는데 누군가가 보낸 메시지에 내가 하고 있던 일을 방해받고, 내 주의력이 분산되는 것이 싫기 때문이다.

나에게 메시지를 보낸 사람이 나를 이기적으로 생각할지 모르지만, 상대도 또한 메시지를 보내 나의 시간을 잠시 자신을 위한 일로 사용하기를 바란 것이라고 생각할 수 있다.
따라서 메시지를 무음으로 놓고 내가 원하는 시간에 그 메시지를 확인하는 것은 예의에 어긋나는 일이 아니다.
상대를 위해 내가 24시간 대기조가 되어야할 필요는 없지 않은가?

하지만 실시간으로 확인하지는 않더라도, 상대가 원치않는 시점에(요청이 없는데도) 메시지를 보내는 일은 필요하다.
상대의 "시간주권"을 존중하기 위해서 내가 하고 싶은 일을 못하는 것도 말이 안되기 때문이다.
어쨌든 우리가 살아가려면 얼마간의 폐를 서로에게 끼칠 수 밖에 없다!

웹 프로그래밍을 할 때도 요청하지 않은 상대에게 메시지를 전달해야할 상황이 왕왕 발생한다.
부트캠프인 코드 스테이츠의 마지막 팀 프로젝트인 "4주 프로젝트"로 "꾹꾹이"라는 이름을 가진 카페 쿠폰 적립 서비스를 만드는 것에 참여했다.
이 프로젝트에서 나는 백엔드를 담당했었다.
이 서비스의 가장 핵심적인 기능은 쿠폰을 적립하는 기능이었다.
이 프로젝트에서 쿠폰 적립은 다음 그림과 같이 이루어진다.

<figure>
  <img src="/assets/figures/ggugx2_coupon.png" alt="server-client"/>
</figure>

이 과정은 카페 고객이 사용하는 클라이언트 앱(이하 앱), 서버, 사장님이 사용하는 관리자 웹페이지(이하 웹) 사이에 다음과 같이 일어난다. 

- 앱에서 고객이 적립 요청
- 서버가 가게에 적립 요청 전달
- 웹에서 사장님이 승인 메시지 서버에 전달
- 서버는 데이터베이스를 적절히 업데이트하고, 성공 메시지를 고객과 사장님 모두에게 전달

백엔드 담당한 개발자로서 내가 고민하고 해결해야했던 기술적인 문제는 서버에서 클라이언트로 먼저 메시지를 보내는 일이었다.
얼핏 생각하면 별 문제가 아니라고 생각할 수 있지만,
사실 이 문제를 해결하는 것은 초심자인 나에게는 그리 간단치 않은 문제였다.
왜냐하면 내가 알고 있던 통신 방식인 HTTP는 서버-클라이언트 모델로 되어있기 때문이다.
대체 HTTP가 어떻길래 이게 어렵다고 생각한 것인지 HTTP를 잠시 알아보자.

## HTTP

<figure>
  <img src="https://developers.google.com/web/fundamentals/performance/http2/images/multiplexing01.svg" alt="server-client"/>
  <figcaption>HTTP 2.0 연결에서 스트림. 스트림을 이해할 필요는 없지만 HTTP에서 데이터 흐름이 클라이언트에서 서버 방향으로만 이루어진다는 것을 보여준다. 출처: <a href="https://developers.google.com/web/fundamentals/performance/http2/">Google Developers의 웹 기초</a> </figcaption>
</figure>

HTTP 프로토콜에서는 클라이언트가 서버에 '요청(request)'를 하면, 서버가 '응답(response)'를 하는 방식으로 통신이 이루어진다.
때문에 내가 원했던 기능인 서버가 먼저 클라이언트에 통신을 보내는 것은 불가능하다.

방법을 찾아보니 이런 문제를 '실시간 통신'이라고 부르고 있었다.
바닥부터 모든 부분을 완벽하게 이해하고 싶지만, 그럴 수도 없고 시간도 많지 않으니,
딱 궁금증을 해결할 정도로만 이해하고 문제 해결에 집중하도록 하자.
일단 '실시간 통신'이 무엇인지, 어떤 방식으로 구현할 수 있는지 간략하게 알아보도록 하자.

# 실시간 통신

실시간 통신은 서버/클라이언트가 필요한 시점에 바로 통신할 수 있는 것을 말한다.
여기서부터는 어떤 방법으로 실시간 통신을 구현할 수 있는지 살짝 살펴보도록하겠다.
더 다양한 방법이 있을 수 있겠지만, 일단은 세가지 방법만 소개하도록 한다.

## Polling

<figure>
  <img src="https://hpbn.co/assets/diagrams/4ee6145071a2992920f9681c069824a4.svg" alt="bootcamp_marine"/>
  <figcaption>Polling과 Long-polling이 어떻게 동작하는지 보여주는 도식. 출처: <a href="https://hpbn.co/xmlhttprequest/">High Performance Browser Networking | O'Reilly</a> </figcaption>
</figure>

Polling은 아주 단순한 방식으로 실시간 통신을 구현한다. 
바로 클라이언트가 계속 서버에 요청응답을 정해진 시간(polling interval)마다 반복하는 것이다.
이렇게 하면, 서버는 업데이트해서 보내줄 내용이 있다면, 다음에 오는 요청에대한 응답으로 내용을 보내주면 된다.
물론, 줄 정보가 없다면, 빈 응답을 내려준다.

이런 방식의 단점은 매우 명확하다.
낭비되는 요청-응답이 매우 많기 때문에 서버에 traffic을 많이 발생시키게 된다.
그렇다고 traffic을 줄이기 위해서 polling interval을 늘리면, 점점 실시간에서 멀어지게 된다. 이 polling interval을 정하는 것에는 정답은 없는 것 같다. 필요에 맞게 적절한 값을 찾아야할 것이다.

### 요약

- 요청-응답을 정해진 시간마다 반복
- 단점
  - 낭비되는 요청-응답 많음
  - 서버에 필요없는 traffic 많이 발생
  - HTTP 사용으로 인한 overhead 많음
  - 전송지연 일어남

## Long-polling

Long-polling은 polling과 동일한 방식을 사용한다.
단, 응답을 바로 하는 것이 아니라 오랜시간 기다린 후에 응답을 보낸다.
이렇게 되면 기다리는 중간에 업데이트가 생길 때 정말 실시간으로 통신을 할 수 있게 된다.
이런 점 때문에 polling 방식에서 낭비되는 요청-응답을 줄일 수 있고, 서버에 traffic을 덜 발생시킬 수 있다. 

Long-polling도 단점을 갖고 있는데, 가장 큰 단점은 바로 HTTP 자체에 있다.
데이터를 여러번 주고 받아야 된다고 생각해보자.
HTTP라는 약속을 지키기 위해 필수로 넣어야하는, 그러나 데이터와는 관련이 없는 동일한 정보(overhead)를 쓸 데 없이 많이 주고받아야한다.
이것이 전송 지연을 일으킨다.

### 요약

- Polling + 응답 지연
- Polling보다 서버 traffic 발생 줄여줌
- 단점
  - HTTP 사용으로 인한 overhead 많음
  - 전송 지연 일어남

## WebSocket

<figure>
  <img src="https://hpbn.co/assets/diagrams/1a8db2948eb2aad0dd47470c6c011a42.svg" alt="bootcamp_marine"/>
  <figcaption>가장 왼쪽은 polling, 가장 오른쪽은 WebSocket의 동작 모습이다. 출처: <a href="https://hpbn.co/websocket/">High Performance Browser Networking | O'Reilly</a> </figcaption>
</figure>

WebSocket은 HTTP의 근간을 이루는 TCP/IP 프로토콜을 사용해 동작한다.
TCP/IP는 point-to-point 통신으로 전화 같은 것이라고 생각하면 된다.
번호만 알면, 원하는 사람에게 전화를 걸 수 있고 양방향으로 모두 소통이 가능하다.
TCP/IP가 HTTP 보다 더 low level의 약속이기 때문에 overhead가 적다.
또, 직접 양방향으로 직접 소통할 수 있기 때문에 실시간 통신이 제대로 구현된다.

WebSocket을 연결하기 위해서는 먼저 HTTP(요청-응답)로 'handshake'를 한다.
이 과정이 끝나면 서버와 클라이언트 모두 서로 직접 TCP/IP로 통신할 수 있는 정보를 갖게된다.
이후의 통신은 앞서 두가지 방식의 소통보다 overhead는 적게, '실시간'에 가깝게 이루어진다.

# 다음 포스트에서는... (socket.io)

여기까지 실시간 통신이 어떤 방식으로 구현될 수 있는지 살펴봤다.
다음 포스트에서는 실시간 통신을 socket.io를 이용해 Javascript에서 어떻게 구현해 쿠폰 적립 프로세스를 완료했는지 설명할 예정이다.

# 참고

- https://hpbn.co/xmlhttprequest/
- https://hpbn.co/websocket/