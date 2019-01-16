---
layout: post
title: "자바스크립트 공부 // 6. this"
comments: true
author: feynubrick
date:   2019-01-11
tags: [JavaScript, Study]
---

# this 키워드
 - 모든 함수 스코프 내에서 자동으로 설정되는 특수한 식별자
 - 실행컨텍스트(execution context)의 생성단계에서 결정된다.

# this 를 binding하는 5가지 패턴

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

## 패턴 5: .call() or .apply() invocation

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

# call 과 apply

`call`과 `apply`는 모두 함수의 프로토타입 메서드(function prototype method)다. 
각각의 용법은 다음과 같다.

- `function.call(thisArg, arg1, arg2, ...)`
- `function.apply(thisArg, [argsArray])`

두개가 arguments를 separate 표현으로 넣느냐 배열로 넣느냐의 차이밖에 없으므로, 두개를 따로 설명하는 것이 아니라 `call`을 중심으로 설명하도록 하겠다.

```javascript
var obj = {
    string: 'origin',
    foo: function () {
        console.log(this.string);
    }
};

var obj2 = {
    string: 'what?'
};

obj.foo(); // Q1
obj.foo.call(obj2); // Q2
```

- Q1: `"origin"`
- Q2: `"what?"`

## call in action

뭔지는 알겠지만 왜 사용하는지는 모르겠다고?
다음 예를 한번 보자.

### arguments를 배열의 프로토타입 메서드를 사용해 처리

`arguments`는 함수의 인자를 담고있는 특별한 객체다.
이 객체는 배열은 아니지만 배열처럼 index와 `length` 값을 갖고 있어서,
배열 같은 객체(array-like object)라 부른다.

이 객체가 배열이 아니기 때문에 배열의 프로토타입 함수 역시 사용할 수 없는데,
`call`을 사용하면 `arguments`를 배열의 프로토타입 함수를 사용해서 처리할 수 있다.

```javascript
function getMax() {
    var argsArray = Array.prototype.slice.call(arguments);
    var maxValue = argsArray.reduce(function (max, val) {
        return (val > max) ? val : max;
    });

    return maxValue;
}

console.log(getMax(4, 5, 2, 7, 3)); // Q1
```

- Q1: `7`

### 한 객체에 여러 생성자를 엮기

```javascript
function Product(name, price) {
    this.name = name;
    this.price = price;
    this.print = function () {
        console.log(this.constructor.name + '// ' + this.name + '\t' + this.price + ' USD');
    };
}

function Food(name, price) {
    Product.call(this, name, price);
    this.category = 'food';
}

function Toy(name, price) {
    Product.call(this, name, price);
    this.category = 'toy';
}

var cheese = new Food('feta', 5);
var fun = new Toy('robot', 40);

cheese.print(); // Q1
fun.print(); // Q2
```

- Q1 => Food: feta	5 USD
- Q2 => Toy: robot	40 USD

# bind
인자로 넘겨준 객체와 연결된 새로운 함수를 반환한다.
콜백함수(callback function)을 특정 객체와 연결하고 싶을 때 사용한다.

`function.bind(thisArg[, arg1[, arg2[, ...]]])`

```javascript
function foo() {
    return this;
}

var boundFoo = foo.bind({ a:1 });
foo(); // Q1
boundFoo(); // Q2
```

- Q1: Window
- Q2: {a: 1}

```javascript
function Box(w, h) {
    this.width = w;
    this.height = h;
    this.getArea = function () {
        return this.width * this.height;
    }

    this.printArea = function() {
        console.log(this.getArea());
    }

    this.printArea();
    setTimeout(this.printArea, 1000);
}

var b = new Box(100, 50);
```

위의 코드를 실행하면 참조 에러(reference error)가 날 것이다.
`setTimeout`의 `this`는 `Window` 객체와 바인드 되기 때문이다.
(`this`를 바인드하는 다섯가지 방법 참조)

이 문제를 해결하려면 다음과 같이 `bind`를 사용해서 `this`를 명시해주면 된다.

```javascript
function Box(w, h) {
    this.width = w;
    this.height = h;
    this.getArea = function () {
        return this.width * this.height;
    }

    this.printArea = function() {
        console.log(this.getArea());
    }

    this.printArea();
    setTimeout(this.printArea.bind(this), 1000);
}

var b = new Box(100, 50);
```

실행시키면 문제없이 1초후에 결과(`5000`)가 계산돼서 나온다.

## currying using bind

"currying"은 [Haskell Brooks Curry](https://en.wikipedia.org/wiki/Haskell_Curry)의 라는 사람의 이름에서 유래했으며, 
함수를 각각이 하나의 인자를 가져가는 여러 함수로 쪼개는 과정을 말한다.

예를 들어,

```javascript
function sum3(x, y, z) {
    return x + y + z;
}

console.log(sum3(1, 2, 3)); // 6
```

을 다음의 모양으로 바꾸는 것을 말한다.

```javascript
function sum3(x) {
    return function (y) {
        return function (z) {
            return x + y + z;
        };
    };
}

console.log(sum3(1)(2)(3)); // 6
```

이 "currying"을 `bind`를 통해 구현해보자.
다음 예를 보면 어떻게 하는지 알 수 있다.

```javascript
function template(name, money) {
    return '<h1>' + name + '</h1><span>' + money + '</span>';
}

var templateIngi = template.bind(null, 'Ingi Kim');
var templateHoyong = template.bind(null, 'Hoyong Lee');
```

# 퀴즈

```javascript
var fn = function(one, two) {
    console.log(this, one, two);
};

var red = {r: 1}, green = {g: 1}, blue = {b: 1}, yellow = {y: 1};
red.method = fn;

red.method(green, blue); // Q1
fn.call(red, green, blue); // Q2
red.method.call(yellow, green, blue); // Q3
setTimeout(fn, 1000); // Q4
setTimeout(red.method, 1000); // Q5
setTimeout(function() {
    red.method(green, blue);
}, 1000); // Q6
new red.method(green, blue); // Q7
setTimeout(fn.bind(red), 1000); // Q8
setTimeout(red.method.bind(red), 1000); // Q9
```
- Q1: red, green, blue
- Q2: red, green, blue
- Q3: yellow, green, blue
- Q4: window, undefined, undefined
- Q5: window, undefined, undefined
- Q6: red, green, blue
- Q7: purple, green, blue
- Q8: red, green, blue
- Q9: red, green, blue

Q8에서 purple은 red, green, blue, yellow 가 아닌 다른 객체를 말한다.

# 참조

- 코드스테이츠의 프리코스 강의
- [currying](https://hackernoon.com/currying-in-js-d9ddc64f162e)