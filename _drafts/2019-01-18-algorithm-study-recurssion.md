---
layout: post
title: "알고리듬 공부 // 재귀(Recurssion)"
comments: true
author: feynubrick
date: 2019-01-18
tags: [JavaScript, Study, Algorithm]
---

https://www.cs.odu.edu/~toida/nerzic/content/recursive_alg/rec_alg.html

https://hackernoon.com/algorithms-explained-recursion-54831247dd85

https://medium.freecodecamp.org/recursion-is-not-hard-858a48830d83

# 재귀(Recurssion)?

재귀 함수는 기본조건이 참일 때까지 자기 자신을 호출하고, 실행을 종료하는 함수를 말한다.

```javascript
function recursive (n) {
    if (n !== 0) {
        return recursive(n - 1);
    } else {
        return 0;
    }
}
```

여기서, 기본 조건은 `n === 0`에 해당하고, 조건이 이 기본조건에 도달하지 않을 때는 한단계씩 기본조건에 다가가게 하면서 자기자신을 부른다.

## Example 1: N 번째 짝수 구하기

`N` 번째 짝수를 구하는 함수 `getNthEvenNumber(N)`를 재귀를 사용해 만들어보자.

```javascript
function getNthEvenNumber (N) {
    if (N === 1) {
        return 0;
    } else {
        return getNthEvenNumber(N-1) + 2;
    }
};

getNthEvenNumber(3); // 4
```

`getNthEvenNumber(3)`을 실행하면, 다음의 순서로 문제를 해결한다.

- `getNthEvenNumber(3)`: `N`이 `3`이군, `getNthEvenNumber(2)`야 결과 좀 보내줘!
  - `getNthEvenNumber(2)`: `N`이 `2`이군, `getNthEvenNumber(1)`야 결과 좀 보내줘!
    - `getNthEvenNumber(1)`: `N`이 `1`이군, `0`을 반환하고 쉬어야지.
  - `getNthEvenNumber(2)`: `getNthEvenNumber(1)`에서 반환 값이 왔군. `0`이라는데?
  - `getNthEvenNumber(2)`: 그럼 `2`를 반환하고 쉬어야지.
- `getNthEvenNumber(3)`: `getNthEvenNumber(2)`에서 응답이 왔군. `2`이라는데?
- `getNthEvenNumber(3)`: 그럼 `4`를 반환하고 쉬어야지.

## Example 2: 2의 N 제곱 계산하기

`2`의 `N` 제곱을 계산하는 `getPowerOf2(N)`을 재귀함수를 사용해 만들어보자.

- `N`이 `0` 이라면, `1`을 반환한다.
- 아니라면, `2 * getPowerOf2(N - 1)`을 반환한다.

코드로 쓰면 다음과 같다.

```javascript
function getPowerOf2 (N) {
    if (N === 0) {
        return 1;
    } else {
        return 2 * getPowerOf2(N - 1);
    }
}

getPowerOf2(3); // 8
```

## Example 3: 순차 검색(sequential search)

- 입력: `array`가 배열이고, `i`, `j`는 `i` $<=$ `j`를 만족하는 양의 정수이고, `x`는 배열 `array`에서 검색될 키를 말한다.
- 출력: `x`가 `array`의 `i` 번째, `j` 번째 사이에 있다면, 결과는 `x`가 들어있는 인덱스이고, 없으면, -1이다.

```javascript
function searchArraySequentially (array, i, j, x) {
    if (i <= j) {
        if (array[i] === x) {
            return i;
        } else {
            return searchArraySequentially(array, i + 1, j, x);
        }
    } else {
        return -1;
    }
}

var array = ['a', 'b', 'c', 'd', 'e'];
var result1 = searchArraySequentially(array, 0, 4, 'e');
var result2 = searchArraySequentially(array, 0, 3, 'e');
console.log(result1); // 4;
console.log(result2); // -1;
```

## Example 4: 자연수 판별
```javascript
function isNaturalNumber (x) {
    if (x < 0) {
        return false;
    } else {
        if (x === 0) {
            return true;
        } else {
            return isNaturalNumber(x - 1);
        }
    }
}

console.log(isNaturalNumber(-1)); // false
console.log(isNaturalNumber(0)); // true
console.log(isNaturalNumber(100)); // true
console.log(isNaturalNumber(12.4)); // false
```
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