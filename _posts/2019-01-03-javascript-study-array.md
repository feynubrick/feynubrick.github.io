---
layout: post
title: "자바스크립트 공부 // 배열(Array)"
comments: true
author: feynubrick
date:   2019-01-03T12:00:00Z
tags: [JavaScript, Study]
---

# 들어가면서

여러 개의 변수를 배열이라는 변수 하나로 for loop을 통해 제어할 수 있기 때문에, 배열은 프로그래밍에서 정말 자주 쓰이는 요소다.
자바스크립트에서는 이 배열을 유용하게 사용할 수 있도록 미리 여러 함수들을 만들어 놓았다.
이 함수들은 for loop을 명시적으로는 사용하지 않았지만, for loop을 내부적으로 사용한 함수들임을 잊어서는 안된다.
그러니까 여기 있는 함수를 몰라도 for loop을 사용하면, 이 함수들의 기능을 구현해볼 수 있다.

배열은 참조타입에 해당하는 변수로, `typeof` 를 써서 알아보면 객체(object)라고 나온다.
그래서 메서드와 속성을 갖고 있다.
아래에서는 배열을 다룰 때 알아야할 속성과 자주쓰이는 메서드들을 소개할 것이다.

## 일러두기: immutable과 mutable

아래의 내용을 보다보면 \[**immutable**\]과 \[**mutable**\]로 표시된 것을 발견할 수 있는데 각각의 의미는 다음과 같다.

- \[**immutable**\]: 메서드 사용 후 원래 배열이 변하지 않음.
- \[**mutable**\]: 메서드 사용 후 원래 배열이 변함.

# 속성(Property)

## [length](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/length)

- `Array` 에 들어있는 항목들의 수를 반환한다.

```javascript
var numbers = [1, 2, 3, 4, 5];
console.log(numbers.length); // 5;
```

- length 값을 직접 변경할 수도 있다. 

```javascript
numbers.length = 6;
console.log(numbers); // [1, 2, 3, 4, 5, empty]
numbers.length = 3;
console.log(numbers); // [1, 2, 3]
```

# 메서드(Method)

## [isArray()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray)

- \[**immutable**\]
- `typeof`를 이용하면 객체라고 나오기 때문에, 어떤 변수가 배열인지 아닌지 감별할 때 이 메서드를 사용한다.

```javascript
Array.isArray([1, 2, 3]); // true
Array.isArray(123); // false
Array.isArray('123'); // false
```

## [forEach()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
- \[**immutable**\]
- 제공된 함수를 각각의 배열 항목마다 실행한다. 

```javascript
var numbers = [1, 2, 3];
numbers.forEach(function(number, i, arr) {
  console.log(i + ', ' + number + ', ' + arr.length);
});
// "0, 1, 3"
// "1, 2, 3"
// "2, 3, 3"
```

## [map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

- \[**immutable**\]
- 배열의 모든 항목 각각을 주어진 함수에 넣고, 새로운 배열의 각 항목에 넣는다. 이렇게 만들어진 새로운 배열을 반환한다. 

```javascript
var numbers = [1, 2, 3, 4];

var doubles = numbers.map(x => x * 2); // using arrow function
// var doubles = numbers.map( (num, i, arr) => 2 * num); 과 같다.

var doubles2 = numbers.map(function(num, i, arr) {
  return num * 2;
});
// doubles === doubles2

console.log(doubles); // [2, 4, 6, 8]
console.log(doubles2); // [2, 4, 6, 8]
```

## [filter()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)

- \[**immutable**\]
- 함수의 형태로 주어진 테스트를 통과한 모든 항목들로만 이루어진 새로운 행렬을 반환. 

```javascript
var numbers = [1, 2, 3, 4, 5, 6, 7, 8];

var result = numbers.filter(number => number % 2 === 0);

var result2 = numbers.filter(function (number, i, arr) {
    if(number % 2 === 0) {
        return true;
    } else {
        return false;
    }
});
// result === result2

console.log(result); // [2, 4, 6, 8]
console.log(result2); // [2, 4, 6, 8]
```

## [push()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push)

- \[**mutable**\]
- 행렬에 하나 또는 여러개의 항목을 추가하고, 추가되어 항목이 증가된 배열의 `length`를 반환. 

```javascript
var numbers = [10, 20, 30];

console.log(numbers.push(40)); // 4
console.log(numbers); // [10, 20, 30, 40]

numbers.push(50);
console.log(numbers); // [10, 20, 30, 40, 50]
```

## [pop()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)

- \[**mutable**\]
- 행렬의 마지막 항목을 제거하고, 제거된 항목을 반환한다. 

```javascript
var numbers = [1, 2, 3, 4, 5];

console.log(numbers.pop()); // 5
console.log(numbers); // [1, 2, 3, 4]

numbers.pop();
console.log(numbers); // [1, 2, 3]
```

## [slice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

- \[**immutable**\]
- 배열은 건드리지 않고, 배열의 일부(`begin`부터 `end`(포함되지 않음) 까지)만 "복사해" 반환한다.

```javascript
var numbers = [1, 2, 3, 4, 5];

console.log(numbers.slice(2)); // [3, 4, 5]
console.log(numbers.slice(1, 3)); // [2, 3, 4]
console.log(numbers.slice(3, 100)); // [4, 5]
console.log(numbers.slice(3, 1)); // []
console.log(numbers.slice()); // [1, 2, 3, 4, 5]
```

## [splice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)

- \[**mutable**\]
- 배열의 항목을 제거하거나 추가한다. 

```javascript
var numbers = [1, 3, 4, 5];
var startIndex = 1;
var numOfElementsToRemove = 0;
var newElement = 2;
numbers.splice(startIndex, numOfElementsToRemove, newElement);
console.log(numbers); // [1, 2, 3, 4, 5]
```

## [reduce()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

- \[**immutable**\]
- `reducer` 함수를 배열의 각각의 항목에 대해 수행하고 하나의 값으로 반환한다. 

```javascript
var numbers = [1, 2, 3, 4];
// ES6 type, arrow function
var reducer = (accumulator, currentValue) => accumulator + currentValue; 
// 1 + 2 + 3 + 4
console.log(numbers.reduce(reducer)); // 10, 초기값이 첫번째 항목인 1
// 5 + 1 + 2 + 3 + 4
console.log(numbers.reduce(reducer, 5)); // 15, 초기값을 5로 지정

var result = numbers.reduce(function(accumulator, currentValue, currentIndex, array) {
    return accumulator + currentValue;
});
// numbers.reduce( (accumulator, currentValue, currentIndex, array) => accumulator + currentValue ); 와 같다.
console.log(result); // 10
// 초기값을 넣어주려면 다음과 같이 한다.
arr.reduce(callback, init);
```

`reducer` 함수의 반환된 값은 accumulator에 할당되고, 모든 항목에 대해 이 reducer 함수를 적용한다.

## [join()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join)

- \[**immutable**\]
- 행렬의 모든 항목을 주어진 `seperator`를 사용해 연결해 문자열로 표현한다. `separator`의 기본 값은 `,` 다. 

```javascript
var numbers = [1, 2, 3, 4];
console.log(numbers.join()); // "1,2,3,4"
console.log(numbers.join('')); // "1234"
console.log(numbers.join('-')); // "1-2-3-4"
```

## [indexOf()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)
- \[**immutable**\]
- 배열에서 주어진 값을 찾아 가장 처음 발견하는 인덱스를 반환. 못찾을 경우 `-1`을 반환

```javascript
var numbers = [1, 2, 3, 4, 5, 1, 2, 3, 4, 5];
console.log(numbers.indexOf(2)); // 1
console.log(numbers.indexOf(2, 4)); // 6 => 4번째 인덱스부터 찾음
console.log(numbers.indexOf(1000)); // -1
```

### 참고하면 좋은 메서드
- `includes()`: 구형 브라우저에서 사용할 수 없다.
- `find()`: callback 함수를 직접 넣어 원하는 조건을 만족하는 첫번째 항목을 찾아낼 수 있다.
- `findIndex()`: `find()`와 같은 역할을 하지만, 항목이 아닌 인덱스를 반환한다.

## [concat()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)

- \[**immutable**\]
- 두개 이상의 행렬을 합칠 때 사용한다.
- 합친 새로운 배열을 반환한다.

```javascript
var numbers1 = [1, 2, 3, 4, 5];
var numbers2 = [6, 7, 8];
var alpha = ['a', 'b', 'c'];
var numbers3 = numbers1.concat(numbers2);
console.log(number3); // [1, 2, 3, 4, 5, 6, 7, 8]
var alphanumeric = alpha.concat(numbers1, numbers2);
console.log(alphanumeric); // ["a", "b", "c", 1, 2, 3, 4, 5, 6, 7, 8]
```

## [every()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every)

- \[**immutable**\]
- 행렬의 모든 항목이, 주어진 함수로 만들어진 테스트를 통과했는지 아닌지 확인한다.
- `true` 혹은 `false`를 반환한다.

```javascript
function isOdd(element, index, array) {
    return element % 2 === 1;
}

var odds = [1, 3, 5, 7, 9];
var numbers = [1, 3, 5, 7, 8];
console.log(odds.every(isOdd)); // true
console.log(odds.every( element => element % 2 === 1)); // true
console.log(numbers.every( (element, index, array) => element % 2 === 1)); // false
console.log(numbers.every( function (element) {
    return element % 2 === 1;
})); // false
```

## [some()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some)

- \[**immutable**\]
- 행렬의 단 한 항목이라도 테스트를 통과했는지 아닌지 확인한다.
- `true` 혹은 `false`를 반환한다.
 
```javascript
function isEven(element, index, array) {
    return element % 2 === 0;
}

var odds = [1, 3, 5, 7, 9];
var numbers = [1, 3, 5, 7, 8];
console.log(numbers.some(isEven)); // true
console.log(numbers.some( (element, index, array) => element % 2 === 0)); // true
console.log(odds.some( (element, index, array) => element % 2 === 0)); // false
console.log(numbers.some( function (element) {
    return element % 2 === 0;
})); // false
```

## [fill()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/fill)

- \[**mutable**\]
- 배열의 시작 인덱스부터 끝 인덱스까지 정적인 값으로 채운 새로운 행렬을 반환한다.

```javascript
var numbers = [1, 2, 3, 4, 5];
var fillWith = 0;
var start = 1;
var end = 3;
console.log(numbers.fill(fillWith, start, end)); // [1, 0, 0, 4, 5] 
console.log(numbers.fill(fillWith, start)); // [1, 0, 0, 0, 0]
console.log(numbers.fill(fillWith)); // [0, 0, 0, 0, 0]
```

## [shift()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)

- \[**mutable**\]
- 배열의 첫번째 항목을 제거하고, 이 제거된 항목을 반환한다.

```javascript
var numbers = [1, 2, 3, 4, 5];
var firstNumber = numbers.shift();
console.log(numbers); // [2, 3, 4, 5]
console.log(firstNumber); // 1
```

## [unshift()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift)

- \[**mutable**\]
- 하나 또는 그 이상의 항목들을 배열의 처음에 추가한다.
- 추가한 뒤 변하게된 배열의 길이 값을 반환한다.

```javascript
var numbers = [3, 4, 5];
console.log(numbers.unshift(1, 2)); // 5
console.log(numbers); // [1, 2, 3, 4, 5]
```

## [reverse()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)

- \[**mutable**\]
- 배열을 역순으로 배열해 반환한다.

```javascript
var numbers = [1, 2, 3, 4, 5];
var reversed = numbers.reverse();
console.log('reversed: ', reversed); // [5, 4, 3, 2, 1]
console.log('numbers: ', numbers); // [5, 4, 3, 2, 1]
```

## [sort()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

- \[**mutable**\]
- 배열의 항목들을 정렬하고, 그 행렬을 반환한다.

```javascript
var numbers = [1, 30, 4, 21];
numbers.sort();
console.log(numbers); // [1, 21, 30, 4]
console.log(numbers.sort(function (a, b) {
    return a - b;
})); // [1, 4, 21, 30]
console.log(numbers.sort(function (a, b) {
    return b - a;
})); // [30, 21, 4, 1]

var months = ['March', 'Jan', 'Feb', 'Dec'];
months.sort(months);
console.log(months); // ["Dec", "Feb", "Jan", "March"]
```

