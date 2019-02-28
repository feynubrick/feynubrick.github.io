---
layout: post
title: "자바스크립트 공부 // ES6 문법"
comments: true
author: feynubrick
date: 2019-02-28
tags: [JavaScript, Study, CodeStates]
---

# ES6

ES6는 ECMAScript 2015와 같은 것을 말한다.
이 글에서는 이때 추가된 문법 중 기억해야할 것들을 간단하게 요약해봤다.

# 추가된 기능

## 비구조화 할당(Destructuring Assignment)

### ES5
```javascript
var user = {
    first: 'Toni',
    last: 'Kroos'
};
var numbers = ['one', 'two', 'three', 'four'];

var first = user.first;
var last = user.last;

var eins = numbers[0];
var zwei = numbers[1];
var drei = numbers[2];

console.log('first: ', first); // "Steven"
console.log('last: ', last); // "Gerrard"

console.log('eins means ', eins); // "eins means one"
console.log('zwei means ', zwei); // "zwei means two"
console.log('drei means ', drei); // "drei means three"
```

### ES6
```javascript
var user = {
    first: 'Toni',
    last: 'Kroos'
};
var numbers = ['one', 'two', 'three', 'four'];

var {first, last} = user;
var [eins, zwei, drei] = numbers;

console.log('first: ', first); // "Steven"
console.log('last: ', last); // "Gerrard"

console.log('eins means ', eins); // "eins means one"
console.log('zwei means ', zwei); // "zwei means two"
console.log('drei means ', drei); // "drei means three"
```

## Spread Operator

### ES5

```javascript
function someFunction(a, b, c, d) {
    // do something
}

var args = [1, 2, 4, 16];

someFunction.apply(null, args);
```

### ES6

```javascript
function someFunction(a, b, c, d) {
    // do something
}

var args = [1, 2, 4, 16];

someFunction(...args);
```

## Rest Parameters

### ES5

```javascript
var multiplyByNum = function (x) {
    var nums = Array.prototype.slice.call(arguments, 1);
    nums.forEach(function(num) {
        console.log(x * num);
    });
};
```

### ES6

```javascript
var multiplyByNum = function (x, ...nums) {
    nums.forEach(function(num) {
        console.log(x * num);
    });
};
```

## Default Parameters

### ES5

```javascript
var generateAddress = function (city, state, country) {
    country = country === undefined ? 'USA' : country;
    return city + ', ' + state.toUpperCase() + ', ' + country; 
};

generateAddress('Oakland', 'CA'); // "Oakland, CA, USA"
generateAddress('Calgary', 'AB', 'Canada'); // "Calgary, AB, Canada"
```

### ES6

```javascript
var generateAddress = function (city, state, country = 'USA') {
    return city + ', ' + state.toUpperCase() + ', ' + country; 
};

generateAddress('Oakland', 'CA'); // "Oakland, CA, USA"
generateAddress('Calgary', 'AB', 'Canada'); // "Calgary, AB, Canada"
```

## Template Literals

### ES5

```javascript
var generateAddress = function (city, state, country) {
    country = country === undefined ? 'USA' : country;
    return city + ', ' + state.toUpperCase() + ', ' + country; 
};

generateAddress('Oakland', 'CA'); // "Oakland, CA, USA"
generateAddress('Calgary', 'AB', 'Canada'); // "Calgary, AB, Canada"
```

### ES6

```javascript
var generateAddress = function (city, state, country = 'USA') {
    return `${city}, ${state.toUpperCase()}, ${country}`;
};

generateAddress('Oakland', 'CA'); // "Oakland, CA, USA"
generateAddress('Calgary', 'AB', 'Canada'); // "Calgary, AB, Canada"
```

## 애로우 함수(Arrow Function)

애로우 함수는 `function` 키워드로 만드는 함수와 표현법만 다른 것이 아니라 다음과 같은 특징을 갖고 있다.

- this를 바인드 하지 않는다.
- arguments를 바인드하지 않는다.
- prototype 속성을 갖지 않는다.
- constructor로 사용할 수 없다. 그래서 new 연산자(operator)를 사용할 수 없다.

### ES5

```javascript
var multiplier = function (x, y) {
    return x * y;
}
```

### ES6

```javascript
var multiplier = (x, y) => {
    return x * y;
}
// OR
var multiplier = (x, y) => x * y;
```

## for ... of loop

### ES5

```javascript
var numbers = [1, 2, 3, 4];

for (var i = 0; i < numbers.length; i++) {
    var number = numbers[i]
    console.log(number);
}
```

### ES6

```javascript
var numbers = [1, 2, 3, 4];

for (var number of numbers) {
    console.log(number);
}
```

# 참고

- 코드스테이츠의 이머시브 강의
- MDN web docs