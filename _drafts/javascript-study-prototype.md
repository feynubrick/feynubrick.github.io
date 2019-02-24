---
layout: post
title: "자바스크립트 공부 // 프로토타입(Prototype)"
comments: true
author: feynubrick
date: 2019-02-24
tags: [JavaScript, Study, CodeStates]
---

# 프로토타입(Prototype)?

객체지향 언어에서 사용하는 클래스를 자바스크립트에서는 프로토타입으로 구현할 수 있다.
객체 포스트에서 잠깐 소개했지만, 객체 생성자에 대해 이야기한 적이 있다.
거기서는 객체 생성자를 만들고 그 객체 생성자로 만들어진 객체들에 새로운 속성이나 메서드를 추가하기 위해 프로토타입을 사용했다.
다음처럼 말이다.

```javascript
function Circle (radius) {
    this.radius = radius;
}

var simpleCircle = new Circle(10);
var colouredCircle = new Circle(5);
colouredCircle.colour = "red";

console.log(simpleCircle.colour); // undefined
console.log(colouredCircle.colour); // "red"

Circle.prototype.describe = function() {
    return "This circle has a radius of: " + this.radius;
};

console.log(simpleCircle.describe()); // This circle has a radius of: 10
console.log(colouredCircle.describe()); // This circle has a radius of: 5
```

객체 생성자 `Cricle`에는 원래 `describe`라는 메서드가 존재하지 않았다.
이 상태로 `simpleCricle`과 `colouredCircle`이라는 객체를 만들었다.
만약에 생성자가 생성자 안에 만들어진 속성을 모두 복사하는 방식으로 새로운 객체를 만들었다면,
위의 두 객체의 `describe` 메서드는 실행되어서는 안된다.
그런 메서드는 존재하지 않았기 때문이다.

그러나 프로토타입은 그런 방식을 사용하지 않는다.
복사하지 않고 단지 참조만 할 뿐이다.
그러니까 프로토타입에 새로운 메서드를 추가하면,
생성자 `Circle`로 만들어진 객체가 갖고있는 프로토타입(prototype) 참조를 사용해 메서드에 접근할 수 있는 것이다.

더 자세히 말하면, `simpleCircle.describe()`를 실행하면 먼저 `simpleCircle` 객체에서 `describe`라는 이름을 갖는 식별자를 찾는다.
당연히 못 찾았으므로, 이제는 프로토타입에서 `describe`를 찾는다.
이렇게해서 describe를 실행한 것이다.

위의 방식을 보니 좀 이해가 됐는지 모르겠다.
자바스크립트는 프로토타입을 사용해 클래스의 상속을 흉내내고 있다.

# Object.create() 메서드

위의 예에서 prototype에 직접 메서드를 추가하는 방식을 사용했다.
비슷한 일을 `Object.create()`를 사용해서 구현할 수 있다.

```javascript

function Circle (radius) {
    this.radius = radius;
    this.describe = function() {
        return "This circle has a radius of: " + this.radius;
    };
}

const circle = Circle(5);

var simpleCircle = Object.create(circle);
simpleCirCle.radius = 20;
var colouredCircle = Object.create(circle);
colouredCircle.colour = "red";

console.log(simpleCircle.colour); // undefined
console.log(colouredCircle.colour); // "red"

console.log(simpleCircle.describe()); // This circle has a radius of: 10
console.log(colouredCircle.describe()); // This circle has a radius of: 5
```

요약하면, `Object.create` 함수는 인자로 들어있는 객체를 프로토타입으로 하는 새로운 객체를 만들어내는 함수다.

# 프로토타입 연결(Prototype Link)과 프로토타입 객체(Prototype Object)



# 참고

- 코드스테이츠의 Immersive 코스 강의
- [오승환님의 블로그](https://medium.com/@bluesh55/javascript-prototype-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-f8e67c286b67)
- [[Youtube의 FunFunFunction 영상]`__proto__` vs `prototype`](https://www.youtube.com/watch?v=DqGwxR_0d1M)