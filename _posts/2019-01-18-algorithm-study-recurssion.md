---
layout: post
title: "알고리듬 공부 // 재귀(Recurssion)"
comments: true
author: feynubrick
date: 2019-01-18
tags: [JavaScript, Study, Algorithm]
---

# 재귀(Recurssion)?

재귀는 자기 자신을 내부에서 불러 문제를 해결하는 방법이다.

자바스크립트 코드로 표현하면 다음과 같다.

```javascript
function foo() {
    foo();
}
```

# 반복문과 어떻게 다르고 어떤 장점이 있는가?

팩토리얼(Factorial) 계산을 통해 두개가 어떻게 다른지 살펴보자.

## 방법 1: 반복문 사용

```javascript
var factorial = function(number) {
    var result = 1;
    for (var i = number; i > 0; i--) {
        result *= i;
    }
    return result;
}
```

## 방법 2: 재귀 사용

```javascript
var factorial = function(number) {
    if (number === 0) {
        return 1;
    }

    return (number * factorial(number - 1));
}
```

## 모든 재귀는 반복문으로 바뀔 수 있다

이 명제는 앨런 튜링(Alan Turing)이 자신의 논문에서 증명했다.

# 재귀 함수의 일반적인 구성

- 기본 케이스
- 재귀적인 케이스

```javascript
var function_name = function (input) {
    if (termination_condition) {
        return value;
    } else if (base_case) {
        return value;
    }

    return (expression_with_recursion_call);
}
```

# 사용 예

## 피보나치 수열(Fibonacci series)

피보나치 수열은 잘 알려져 있듯이,
앞의 두 수를 더한 결과가 다음 수로 정해지는 수열을 말한다.

```
0, 1, 1, 2, 3, 5, 8, 13, ...
```

두가지 방법을 사용하여 n번째 수를 구하는 함수를 만들어보자.

### 반복문 사용

```javascript
var fibonacci = function(n) {
    // base case
    if (n === 1) {
        return 0;
    } else if (n === 2) {
        return 1;
    }

    var left2 = 0;
    var left1 = 1;
    var num;
    for (var i = 3; i <= n; i++) {
        num = left2 + left1;
        left2 = left1;
        left1 = num;
    }

    return num;
}
```

### 재귀

```javascript
var fibonacci = function (n) {
    // base case
    if (n === 1) {
        return 0;
    } else if (n === 2) {
        return 1;
    }

    return fibonacci(n - 2) + fibonacci(n - 1);
}
```