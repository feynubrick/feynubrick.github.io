---
layout: post
title: "자바스크립트 공부 // 7. 비동기 호출(Asynchronous Call)"
comments: true
author: feynubrick
date: 2019-01-16
tags: [JavaScript, Study]
---

# 콜백(Callback)과 콜백 함수(Callback function)

콜백(Callback)은 다른 코드의 인수로서 넘겨주는 실행가능한 코드를 말한다.
콜백을 넘겨받는 코드는 이 콜백을 필요에따라 즉시 실행할 수도 있고, 아니면 나중에 실행할 수도 있다.

콜백 함수(callback function)는 그런 함수를 말한다.
그러니까 다른 코드의 인자(argument)로서 넘겨주는 함수를 말한다.

콜백을 매개변수(parameter)로 넘겨받는 코드는 이 콜백을 필요에 따라 즉시 실행(동기, synchrously)하거나,
나중에 실행(asynchronously)할 수 있다.

```javascript
function executableCode() {
    // codes
}

function otherCode(callback) {
    callback(); // it can be executed
}

otherCode(executableCode());
```

## 콜백의 실제 사용예

### iterator

배열의 메서드를 공부할 때 `map`, `reduce`, `forEach` 같은 iterator를 배웠다.
이 메서드의 첫번째 인자는 함수였는데, 이 함수들도 콜백 함수이다.
그러니까 for loop을 돌면서 매개변수(parameter)로 전달된 콜백함수를 배열의 요소 각각에 대해 실행한다.

다음의 예는 `map`을 사용해 배열의 각 요소를 제곱하는 것을 보여준다.

```javascript
[1, 2, 3].map(function(val) {
    return val * val;
});
```

### 이벤트 핸들러(Event handler)

다음 예는 DOM(Document Object Model)라는 Web API(Application Programming Language)를 사용해,
HTML로 만들어진 웹페이지에서 어떤 버튼을 눌렀을 때 콘솔에 "button clicked!"라는 메시지가 뜨도록 만든 것이다.

```javascript
document.querySelector('#btn').addEventListener('click', function(e) {
    console.log('button clicked!');
});
```

`document` 객체의 `querySelector` 메서드를 사용해 HTML 속성 id(`#`)로 `btn`을 갖는 요소를 찾는다.
그 뒤, `addEventListener`를 사용해 `'click'` 이벤트가 발생하면, 콜백함수를 실행하도록 등록한다.

그러니까 이 함수가 호출되고 실행돼도 콘솔에 메시지가 찍히진 않는다.
나중에 이 자바스크립트가 실행된 HTML 문서에서 id `btn`을 갖는 버튼이 눌려야(click) 비로소 이 콜백 함수가 실행 되고,
콘솔에 메시지가 나오는 것이다.

논리적으로 생각해봤을 때, 이 과정에서 당연히 event가 발생하는지 지속적으로 확인하는 작업이 있어야한다.
그러니까 누군가는 이벤트가 생기는지 아닌지 감시하고, 이벤트가 발생하면 그 이벤트와 연결된 콜백함수가 있는지 확인한 다음 실행시키는 일을 해야하는 것이다.
그런데 자바스크립트는 싱글 스레드(single thread) 언어로 한번에 하나씩 함수호출을 처리하는데, 어떻게 동시에 두가지 일을 할 수 있는가?
더군다나 이 콜백함수가 많은 시간을 사용하는 함수라고 하면, 이 시간에 생긴 이벤트들은 버려지는 것인가?

# 비동기 호출(Asynchronous Call)

자바스크립트는 함수를 하나씩 차례차례 실행하는데, 이를 동기적(synchronous)이라고 한다.
비동기(asynchronous)란 하나씩 차례차례 실행하는 것이 아니라, 실시간으로 발생하는 요청을 처리하는 것을 의미한다.
앞에서 이야기했던 이벤트 핸들러는, 실제 사용예라는 제목에서 보듯이 이렇게 비동기적으로 함수를 처리해서 자바스크립트에서 잘 작동하고 있다.

동기적인 언어인 자바스크립트에서 어떻게 이것이 가능한 걸까?

## 자바스크립트 런타임 환경(JS Runtime Environment)

자바스크립트가 실행되는 크롬(Chrome), 파이어폭스(Firefox), 엣지(Edge), 사파리(Safari) 같은 웹브라우저는 각각의 런타임 환경을 갖고 있다.
이 환경에는 개발자가 사용할 수 있는 Web API와 함께 자바스크립트 엔진도 같이 들어있다.
브라우저마다 쓰는 자바스크립트 엔진이 다른데, 크롬에서는 V8엔진을 사용한다.

<figure>
  <img src="https://cdn-images-1.medium.com/max/800/1*zeKjWCjyAGZ9JN4fvnWsiA.png" alt="runtime-concept"/>
  <figcaption>자바스크립트 런타임 환경</figcaption>
</figure>

위의 그림을 살펴보자.
이 그림에서 보듯이 자바스크립트 런타임 환경은 다음의 것들을 포함한다.

- 자바스크립트 엔진
  - 힙(Heap)
  - 호출 스택(Call Stack)
- Web API container
- 콜백 대기열(Callback queue)
- 이벤트 루프(Event loop)

각각의 것이 무엇을 말하는지 살펴보자.

힙(heap)은 자바스크립트 엔진(여기서는 크롬의 V8 엔진)의 한 부분인데,
변수와 함수 같은 것들이 저장되는 크고, 대부분 구조가 갖춰지지 않은 메모리 공간을 말한다.

호출 스택(call stack)은 역시 자바스크립트 엔진의 한 부분으로 실행컨텍스트가 쌓이는 곳을 말한다.
여기에 올라간 실행컨텍스트는 LIFO(Last In First Out) 방식으로, 그러니까 나중에 들어간 것이 먼저 처리된다.
자바스크립트가 싱글 스레드(single thread) 언어라는 것은 자바스크립트 엔진에 호출 스택은 하나밖에 없다는 말이다.
그러니까 하나씩 차례차례 처리된다는 것이다.

Web API 컨테이너는 호출 스택에서 보내온 이벤트 핸들러, HTTP/AJAX 요청, 타이밍 함수(setInterval, setTimeout) 같은 Web API 호출을 가지고 있다가 각각의 Web API에 대한 호출 조건이 만족되면 이를 콜백 대기열로 보낸다.

콜백 대기열(callback queue)은 쌓여있는 콜백들을 호출 스택이 완전히 비워질 때까지 기다리다가 FIFO(First In First Out) 방식으로,
그러니까 처음 들어온 것을 먼저 콜스택으로 옮긴다.
남은 콜백들은 방금 보낸 콜백이 호출 스택에서 완전히 처리되기 전까지 기다리다가,
호출 스택이 완전히 비워지면 역시 가장 먼저 들어온 콜백을 콜스택으로 옮기고 같은 작업을 반복한다.

이벤트 루프(event loop)는 호출 스택과 콜백 대기열을 계속 감시한다.
호출 스택이 비었을 때 콜백 대기열에게 콜백을 넘기라고 알려주는 일 역시 이 이벤트 루프가 한다.
호출 스택과 콜백 대기열이 모두 비었을 경우에도 이벤트 루프는 감시를 게을리하지 않는데,
Web API 컨테이너에서 Web API 호출이 발생하면 언제라도 이 호출과 연결된 콜백 함수가 콜백 대기열로 옮겨지기 때문이다.

엄밀하게 말하면, 이렇게 처리되는 방식은 비동기적인 것이 아니다.
실제 동작하는 결과를 보면 비동기적으로 작동하는 것으로 보일 뿐이다.
하지만 그게 무슨 상관인가?
원하는 대로 비동기"적"으로 작동만 잘 되면 되는 것 아닌가.

## 자바스크립트는 non-blocking 언어

하지만 여전히 의문은 남는다.
만약 호출 스택이 안비워진다면?
그러니까 호출 스택에 올라간 함수가 워낙 복잡한 작업을 하는 것이어서 시간이 엄청나게 오래걸린다면,
전체가 처리되지 않고 계속 대기하게 되지 않는가?
이런 경우 확실히 이 특정 호출이 다른 호출의 처리를 가로막게(block)된다.

하지만 이런 작업을 Web API에게 넘기고, 처리가 완료되면 어떤 일을 하라는 콜백을 달아준다면,
이런 문제가 있다 하더라도 문제가 있는 호출을 제외한 나머지는 정상적으로 처리될 것이다.
자바스크립트 런타임 환경은 이를 가능하게 만든다.

"자바스크립트는 non-blocking 언어"라고 말하는 것은 바로 이런 방식을 사용하기 때문이다.

# Web API: 타이밍 함수(timing functions)

여기서는 Web API 중에서도 시간과 관련있는 비동기 함수인 타이밍 함수(timing functions)를 살펴보도록 한다.

## setTimeout

```javascript
setTimeout(callback, milliseconds)
```

이 함수는 일정 시간(단위: 밀리 초) 이후에 콜백함수를 호출하는 함수로 다음과 같이 사용한다.

```javascript
setTimeout(function() {
    console.log('1초 후 실행');
}, 1000);
```

이 함수의 반환 값은 이 함수가 사용하는 타이머의 ID다.

## setInterval

```javascript
setInterval(callback, milliseconds)
```

이 함수는 일정 시간(단위: 밀리 초) 마다 콜백함수를 반복해서 호출하는 함수로 다음과 같이 사용한다.

```javascript
setInterval(function() {
    console.log('1초 마다 실행');
}, 1000);
```

이 함수의 반환 값은 이 함수가 사용하는 타이머의 ID다.

이 ID를 이용해 반복적인 실행을 중단할 수 있는데, 다음과 같이 `clearInterval`을 사용하는 것이다.

```javascript
var timer = setInterval(function() {
    console.log('1초 마다 실행');
}, 1000);

clearInterval(timer);
```

이러면 반복적으로 실행되지 않는다.

# 어떨 때 비동기를 사용하는가?

- 이벤트 핸들러
- 타이머에서의 콜백: animation
- 서버에 자원 요청

# 참고

- 코드스테이츠 프리코스 강의
- [동시성 모델과 이벤트 루프](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)
- [비동기와 이벤트 루프(비디오)](https://vimeo.com/96425312)
- [자바스크립트 런타임 환경](https://medium.com/@olinations/the-javascript-runtime-environment-d58fa2e60dd0)
