---
layout: post
title: "자바스크립트 공부 // Promise"
comments: true
author: feynubrick
date: 2019-03-10
tags: [JavaScript, Study, Database]
---

# Prmoise?

자바스크립트에서 서버에 요청을 보내거나 파일을 읽어오는 등의 작업은 "비동기"로,
그러니까 동기가 아닌 방식으로 처리하는 경우가 많다.
동기는 여러 작업이 있을 때, 한 작업씩 차례차례 처리하는 방식을 말한다.
자바스크립트는 싱글 스레드(single thread) 언어라 자바스크립트 엔진은 하나의 콜스택만 갖고 있어,
웹브라우저나 Node 같은 외부의 api를 사용해 비동기를 구현한다.

비동기로 처리되는 작업은 작업의 종료 시기가 작업마다 다르기 때문에 주의해야한다.
비동기함수의 결과를 이용해 무엇인가를 처리해야 할 경우에 비동기 함수의 결과를 기다리지 않는다면
원하는 결과를 얻을 수 없을 것이다.

이런 문제는 callback 함수를 사용해 해결이 가능하다.
예를 들어, A > B > C > D 의 순서로 실행해야하는 비동기 함수들을 원하는대로 작동시키려면 다음과 같은 방법을 사용하면 된다.

```javascript
function asyncFunctions() {
  return funcD(
    funcC(
      funcB(
        funcA()
        )
      )
    );
}
```

문제는 해결할 수 있지만, 아주 보기 흉측스럽다.
이런 끔찍한 모양을 가리켜 callback hell(콜백 지옥)이라고 부르는데,
보기에 끔찍할 뿐아니라 엄청난 고통을 준다는 것을 함축적으로 보여주는 아주 적절한 말인 것 같다.

이런 문제를 해결하는 방법이 바로 Promise다.
Promise를 사용하면 저런 콜백 지옥은 보지 않아도 된다.
다음을 보자.

```javascript
function asyncFunctions(){
  return funcA()
    .then(() => {
      return funcB();
    })
    .then(() => {
      return funcC();
    })
    .then(() => {
      return funcD();
    });
}
```

훨씬 보기 좋고 직관적이다.
그럼 이 Promise는 무엇이고, 어떻게 사용하는 것인지 알아보자.

# Promise 객체

Promise는 객체다.
그래서 다음의 방식으로 만들어낸다.

```javascript
new Promise((resolve, reject) => { ... });
```

이 모습을 보면 Promise 라는 함수가 생성자라는 사실을 알 수 있다.

## Promise 생성자

### resolve와 reject

### promise status와 promise value

- pending
- resolve
- reject

## Prmoise 메서드

### then

## Promise 스태틱 메서드

# 참고

- [Understanding Promises in Javascript](https://hackernoon.com/understanding-promises-in-javascript-13d99df067c1)
