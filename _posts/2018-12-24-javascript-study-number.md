---
layout: post
title: "자바스크립트 공부 // 2. 수(Number)와 수학(Math)"
comments: true
author: feynubrick
date:   2018-12-24
tags: [JavaScript]
---
# 정리한 내용의 출처
여기 정리된 내용은 코드스테이츠와 MDN(Mozilla Developer Network Web Docs)에서 가져온 것을 나름대로 소화해본 것이다.
나의 '소화기관'이 좋지 않기 때문에 대부분은 원형 그대로다.
공부를 더 진행하면서 차차 더 정리를 하도록 할 것이다.
시작이 중요한 것이니 말이다.

코드스테이츠 프리코스 강의에서는 전체적인 얼개와 다뤄야 할 내용들의 목록과 그 내용의 일부를, MDN에서는 그 세부적인 내용을 가져오고 링크를 달아놨다.

# 수(Number)

자바스크립트에서 수는 문자열(string)과 마찬가지로 원시타입에 속하는 변수다.
자바스크립트도 프로그래밍 언어이므로 숫자를 잘 다룰 수 있어야 원하는 알고리듬(algorithm)을 구현할 수 있을 것이다.

아래에 정리된 내용은 수 변수에서 사용할 수 있는 메서드들에 관한 것이다.

## [isInteger()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/isInteger)

- 전달된 값이 정수인지 아닌지 판별한다.

```javascript
console.log(Number.isInteger(10)); // true
console.log(Number.isInteger(1.1)); // false
console.log(Number.isInteger('10')); // false
console.log(Number.isInteger([1]));
```

## [parseInt()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt)

- 정수로 형변환(type casting)
- 진법 변환에도 사용할 수 있다. 예를 들어, 16진수를 10진수를 바꿀 수 있다.
- 두번째 파라미터를 비워두면, 10진수로 변환한다.
  - 하지만, 어떤 진법을 사용하는지 헷갈릴 경우가 많다.
  - 따라서, 10진수로 변환하는 경우에도 두번째 파라미터에 10을 넣어주는 것이 좋다.

```javascript
parseInt(15.12345, 10); // 15
parseInt('ABC', 10); // NaN
parseInt('0xF', 16); // 15
parseInt(6.3e10, 10); // 6
parseInt(0.00001, 10);
parseInt(0.00000000000434, 10); // 4 -> wrong results for very big/small numbers
parseInt(4.7 * 1e22, 10); // 4
```

## [parseFloat()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/parseFloat)

- 부동소수점수(float)로 형변환.

```javascript
parseFloat(15.12345); // 15.12345
parseFloat('ABC'); // NaN
```

## [toFixed()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed)

- 원하는 소수점 아래 위치까지 수를 표현할 때 사용한다.

```javascript
1.53.toFixed(); // "2"
1.53.toFixed(1); // "1.5"
-2.34.toFixed(1); // -2.3을 반환. 연산자 - 때문에 문자열을 반환하지 않는다.
(-2.34).toFixed(1); // "-2.3" // 괄호를 사용해야 문자열로 반환한다.
```

# Math
일종의 수학 라이브러리라고 생각할 수 있다.
`Math`는 객체로서 `Math`에 포함된 메서드들을 사용해 수학과 관련된 계산을 수행할 수 있다.

아래에 정리된 메서드들은 자주 쓰이는 메서드들을 추려서 정리한 것이다.

## [min()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/min)

- 주어진 값 중 최소값을 찾아 반환한다.

```javascript
console.log(Math.min(1, 2, 3)); // 1
console.log(Math.min(1, 2, 'a')); // NaN
console.log(Math.min([1, 2, 3])); // NaN
console.log(Math.min()); // Infinity
```

## [max()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/max)

- 주어진 값 중 최대값을 찾아 반환한다.

```javascript
console.log(Math.max(1, 2, 3)); // 3
console.log(Math.max(1, 2, 'a')); // NaN
console.log(Math.max([1, 2, 3])); // NaN
console.log(Math.max()); // -Infinity
```

## [floor()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/floor)

- 주어진 수보다 작거나 같은, 가장 큰 정수를 반환한다.
- 즉, 버림이다.

```javascript
console.log(Math.floor(0.9)); // 0
console.log(Math.floor(1.1)); // 1
```

## [round()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/round)

- 주어진 수에 가장 근접한 정수를 반환한다.
- 즉, 반올림이다.

```javascript
console.log(Math.round(0.9)); // 1
console.log(Math.round(1.1)); // 1
```

## [random()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random)

- 0과 1 사이(0은 포함하고, 1은 포함하지 않음) 부동소수점(floating-point) 수를 반환한다.

```javascript
function getRandomInt(max) {
    return Math.floor(Math.random() * Math.floor(max));
}

console.log(getRandomInt(3)); // 0, 1, 2 중 하나
```

## [abs()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/abs)

- 주어진 수의 절대값을 반환한다.

```javascript
console.log(Math.abs(-1.2)); // 1.2
```

## [sqrt()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/sqrt)

- 주어진 수의 제곱근을 반환한다.

```javascript
console.log(Math.sqrt(4)); // 2
console.log(Math.sqrt(-4)); // NaN
```

## [pow()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/pow)

- 제곱수를 구한다.
- `base`의 `exponent` 제곱을 반환한다.
- 즉 `base^exponent`를 의미한다. (예: `2^4 = 16`)

```javascript
var base = 2;
var exponent = 4;
var result = Math.pow(base, exponent); // 2**4
console.log(result); // 16
console.log(2**4);
```