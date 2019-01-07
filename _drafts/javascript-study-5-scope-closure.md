---
layout: post
title: "자바스크립트 공부 // 5. 스코프와 클로져(Scope and Closure)"
comments: true
author: feynubrick
date:   2019-01-03
tags: [JavaScript, Study]
---

# Scope
## 스코프(scope)란?
- 변수 접근 규칙에 따른 유효범위
- 자바스크립트에서는 함수가 선언되는 동시에 자신만의 스코프를 가진다.

### lexical(static) scope vs. dynamic scope
- lexical(static) scope: 유효범위가 코드를 작성될 때 결정된다.
- dynamic scope: 유효범위가 실행 순서에 의해 결정된다.

## local scope와 global scope
- 스코프는 중첩이 가능: 스코프가 하위 구조를 가질 수 있다는 뜻
- 하위 스코프는 상위 스코프의 변수에 접근할 수 있다.
- 지역변수는 함수 내에서 전역변수보다 높은 우선순위를 갖는다.

## block level scope vs. function level scope
- 자바스크립트는 function level의 scoping 규칙을 따른다.
- 예외: `let`, `const` 키워드(ES6)
  - block level scope

## 전역변수, window 객체
- 함수의 외부에서 선언된 모든 변수는 전역 범위
- 전역 범위를 대표하는 객체: window(브라우저에서)

## 선언 없이 초기화된 변수는 전역변수
- 선언 없이 변수를 초기화해도 에러는 나지 않지만, 전역 변수로 선언되기 때문에 뜻하지 않은 문제를 만들 수 있다.

## Hoisting
- 변수 선언은 범위에 따라 선언과 할당으로 분리된다.
- 자바스크립트 엔진이 내부적으로 변수 선언을 scope의 상단으로 끌어올린다(hoist).
- 함수의 경우
  - 함수 선언식은 항상 상단에
  - 표현식은 변수 선언만 상단으로 hoisting

# Closure

## 클로저란?
- 외부함수의 변수에 접근할 수 있는 내부 함수
- scope chain으로 표현되기도 한다.
- 보통 함수를 return해 사용
- return하는 내부 함수를 closure 함수라고 지칭
- 