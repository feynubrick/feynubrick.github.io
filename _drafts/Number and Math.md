[TOC]

# Number

## [isInteger()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/isInteger)

전달된 값이 정수인지 아닌지 판별한다.

```javascript
console.log(Number.isInteger(10)); // true
console.log(Number.isInteger(1.1)); // false
console.log(Number.isInteger('10')); // false
console.log(Number.isInteger([1]));
```

## [parseInt()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt)

정수로 형변환(type casting)

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

부동소수점수로 형변환.

```javascript
parseFloat(15.12345); // 15.12345
parseFloat('ABC'); // NaN
```

## [toFixed()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed)

원하는 소수점 아래 위치까지 수를 표현할 때 사용한다.

```javascript
1.53.toFixed(); // "2"
1.53.toFixed(1); // "1.5"
-2.34.toFixed(1); // -2.3을 반환. 연산자 - 때문에 문자열을 반환하지 않는다.
(-2.34).toFixed(1); // "-2.3" // 괄호를 사용해야 문자열로 반환한다.
```

# Math

## [min()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/min)

주어진 값 중 최소값을 찾아 반환한다.

```javascript
console.log(Math.min(1, 2, 3)); // 1
console.log(Math.min(1, 2, 'a')); // NaN
console.log(Math.min([1, 2, 3])); // NaN
console.log(Math.min()); // Infinity
```

## [max()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/max)

주어진 값 중 최대값을 찾아 반환한다.

```javascript
console.log(Math.max(1, 2, 3)); // 3
console.log(Math.max(1, 2, 'a')); // NaN
console.log(Math.max([1, 2, 3])); // NaN
console.log(Math.max()); // -Infinity
```

## [floor()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/floor)

주어진 수보다 작거나 같은, 가장 큰 정수를 반환한다.

```javascript
console.log(Math.floor(0.9)); // 0
console.log(Math.floor(1.1)); // 1
```

## [round()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/round)

주어진 수에 가장 근접한 정수를 반환한다.

```javascript
console.log(Math.round(0.9)); // 1
console.log(Math.round(1.1)); // 1
```

## [random()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random)

0과 1 사이(0은 포함하고, 1은 포함하지 않음) 부동소수점(floating-point) 수를 반환한다.

```javascript
function getRandomInt(max) {
    return Math.floor(Math.random() * Math.floor(max));
}

console.log(getRandomInt(3)); // 0, 1, 2 중 하나
```

## [abs()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/abs)

주어진 수의 절대값을 반환한다.

```javascript
console.log(Math.abs(-1.2)); // 1.2
```

## [sqrt()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/sqrt)

주어진 수의 제곱근을 반환.

```javascript
console.log(Math.sqrt(4)); // 2
console.log(Math.sqrt(-4)); // NaN
```

## [pow()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/pow)

`base`의 `exponent` 제곱을 반환한다.

```javascript
var base = 2;
var exponent = 4;
var result = Math.pow(base, exponent); // 2**4
console.log(result); // 16
console.log(2**4);
```

