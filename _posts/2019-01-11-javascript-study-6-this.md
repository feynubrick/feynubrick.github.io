---
layout: post
title: "자바스크립트 공부 // 6. this"
comments: true
author: feynubrick
date:   2019-01-11
tags: [JavaScript, Study]
---

# `this` 키워드
 - 모든 함수 스코프 내에서 자동으로 설정되는 특수한 식별자
 - 실행컨텍스트(execution context)의 생성단계에서 결정된다.

# `this` 를 binding하는 5가지 패턴

- global reference
- function invocation
- construction mode
- method invocation
- `.call()`  or `.apply()` invocation

## 패턴 1: global reference

- global object(전역 객체)인 `window` 객체에 `this` 바인딩

```javascript
var name = 'Global Variable';
console.log(this.name); // "Global Variable"

function foo() {
	console.log(this.name);
}

foo(); // "Global Variable"
```

## 패턴 2: function invocation 

- global object(전역 객체)인 `window` 객체에 `this` 바인딩
- 그러니까 함수 invocation이 일어나도 `this`는 여전히 `window`와 바인딩 되어있다는 말이다.
- 다른 언어들과는 좀 다르다.

```javascript
var name = 'Global Variable';
function outer() {
    function inner() {
        console.log(this.name); // "Global Variable"
    }
    
    inner();
}

outer();
```

##  패턴 3: method invocation

- 메서드: 객체 안에서 정의된 함수
- 이 메서드를 불러올 때, 메서드가 정의된 객체에 `this` 바인딩

```javascript
var counter = {
    val: 0,
    increment: function() {
        this.val += 1;
    }
};

counter.increment();
console.log(counter.val); // 1
counter['increment']();
console.log(counter.val); // 2

var obj = {
    fn: function(a, b) {
        return this;
    }
};
var obj2 = {
    method: obj.fn
};

console.log(obj2.method() === obj2); // true
console.log(obj.fn() === obj); // true
```

사실 패턴 2와 3은 같은 것을 의미한다.
글로벌 컨텍스트의 경우에는 `window.function()`에서 `window.` 이 생략된 것이라 볼 수 있기 때문이다.

## 패턴 4: construction mode

- new 연산자로 생성된 function 영역의 this
- 이 invocation으로 생성되는 객체에 `this` 바인딩

```javascript
function F(v) {
    this.val = v;
}

// create new instance of F
var f = new F('WooHoo!');

console.log(f.val); // WooHoo!
console.log(val); // ReferenceError
```

## 패턴 5: `.call()` or `.apply()` invocation

- 각 함수의 첫 argument인 객체에 `this` 바인딩
- 수동으로 `this` 바인딩을 서술하기 위해 사용 

```javascript
function identify() {
    return this.name.toUpperCase();
}
function speak() {
    var greeting = "Hello, I'm " + identify.call(this);
    console.log(greeting);
}
var me = { name: 'Kyle' };
var you = { name: 'Reader' };

identify.call(me); // "KYLE"
identify.call(you); // "READER"
speak.call(me); // "Hello, I'm Kyle"
speak.call(you); // "Hello, I'm Reader"
```

함수의 `call` 메서드는 this에 첫번째 argument를 넣고, 함수를 실행시킨다. 
`apply` 메서드 역시 비슷하다. 다만 argument를 배열로 넣어준다는 점만 다르다.

```javascript
var add = function (x, y) {
    this.val = x + y;
}
var obj = {
    val: 0
};

add.apply(obj, [2, 8]);
console.log(obj.val); // 10
add.call(obj, 2, 8);
console.log(obj.val); // 10
```

 