---
layout: post
title: "자바스크립트 공부 // 5. 스코프(Scope), 클로저(Closure), 즉시실행함수(IIFE)"
comments: true
author: feynubrick
date:   2019-01-07
tags: [JavaScript, Study]
---

# 들어가면서

과제를 하다가 for문 안에 함수 정의를 했을 때 이상한 현상이 생겼다. 우선 다음 코드를 보자.

```javascript
for (var i = 0; i < prefixes.length; i++) {
  var prefix = prefixes[i];
  for (var length = 16; length <= 19; length++) {
    it('has a prefix of ' + prefix + ' and a length of ' + length,
      function() {
        detectNetwork(cardnum(prefix, length)).should.equal('China UnionPay');
      }
    );
  }
}
```

이 코드는 `detectNetwork()`라는 함수를 만들고, 이 함수의 결과를 테스트하기 위해 작성한 것이다.
it 함수는 mocha라는 자바스크립트 테스트 프레임워크에서, 테스트 항목을 등록하는 역할을 한다.
이 함수 안에 정의된 함수는 그 앞의 문자열과 연결되고, 나중에 비동기적으로 실행된다.
참고로 이 안에 사용된 `cardnum()` 함수는 인자 두개를 이용해 테스트용 카드번호를 문자열로 만들고 반환하는 함수다.

내가 이 코드를 작성하며 의도한 것은 `prefixes`라는 배열에 담긴 값들과 `length`의 정해진 범위에서 만들어지는 모든 카드번호를 자동으로 테스트하는 것이다.

그러나 결과는 내 생각과는 달랐다.
테스트가 모든 경우의 수를 다루지 않았다는 것이다!

위의 코드를 그냥 보면 모든 조합을 테스트로 등록한 것 같았는데, 무엇이 문제였으며 어떻게 내가 의도한 대로 만들 수 있을까?

# 자바스크립트 인터프리터의 작동 방식
- [참고](http://davidshariff.com/blog/what-is-the-execution-context-in-javascript/)

이 문제를 이해하기 위해서는 자바 스크립트의 다음 개념들을 이해할 필요가 있다.

- 실행 컨텍스트(execution context)
- 스코프(Scope)
- 클로저(closure)
- IIFE(Immediately-invoked function expression)

이 모든 것을 이해하기 전에, 자바스크립트의 인터프리터(interpreter)가 어떤 방식으로 작동하는지 먼저 알아보자.

## 실행 컨텍스트(Execution context)

자바스크립트
코드가 자바스크립트에서 실행될 때 이게 실행되는 환경이 매우 중요하며, 다음 중 하나로 계산된다.

- 글로벌 코드(Global code): 코드가 처음으로 실행되는 기본 환경을 말한다.
- 함수 코드(Function code): 실행의 흐름이 함수 안에 들어갈 때마다
- Eval 코드

이것은 프로그램을 구동시키는데 필요한 환경에 관한 것이라고 볼 수 있는데
가장 global execution context는 하나만 존재한다.
어떤 함수를 선언할 때마다 하나의 execution context가 만들어진다.
각각의 execution context는 "개념적"으로 다음과 같은 객체로 나타낼 수 있다.

```javascript
executionContextObj = {
    'scopeChain': { /* variableObject + all parent execution context's variableObject */ },
    'variableObject': { /* function arguments / parameters, inner variable and function declarations */ },
    'this': {}
}
```

## 실행 컨텍스트 스택(Execution Context Stack)

자바스크립트 인터프리터는 기본적으로 싱글 스레드(single thread) 방식이다. 그러니까 한번에 하나의 실행 컨텍스트를 처리할 수 밖에 없다. 이때 실행 컨텍스트를 올려두는 queue를 실행 컨텍스트 스택 이라 한다.

실행 컨텍스트를 스택에서 처리할 때는 두가지 단계를 거친다.

1. 생성 단계(Creation Stage): 함수가 어딘가에서 불려지고, 아직 그 안의 어떤 코드도 실행되기 전
   1. Scope chain 생성
   2. 함수, argument, 변수 생성
   3. "this" 값 결정
2. 활성화 / 코드 실행 단계(Activation / Code Execution Stage)
   1. 값/레퍼런스 할당
   2. 코드 해석 및 실행

## 인터프리터가 코드를 검토하는 방식의 개요

1. 함수를 작동시키기 위한 코드를 찾는다.
2. `function` 코드를 실행하기 전에 `execution context`를 만든다.
3. 생성 단계(creation stage)로 들어간다.
   1. `Scope chain`을 초기화한다.
   2. `arguments object`를 만들고, 파라미터의 context를 확인하고 변수의 이름과 값을 초기화한 뒤, 참조 복사본(reference copy)을 만든다.
   3. 함수 선언에 대한 context를 훑는다.
      1. 찾아낸 함수 각각에 대해 `variable object` 안에 그 함수 이름 그대로의 키로 하고, 메모리 상의 함수에 대한 참조 포인터를 값으로 갖는 속성을 생성한다.
      2. 만약 함수 이름이 이미 존재하면, 참조 포인터는 덮어씌운다.
   4. 변수 선언의 context를 훑는다.
      1. 찾아낸 변수 선언 각각에 대해, `variable object` 안에  그 변수 이름 그대로를 키로 하고, `undefined`를 값으로 갖는 속성을 생성한다.
      2. 변수 이름이 이미 `variable object`에 존재할 경우에는 아무것도 하지 않고 계속 훑는다.
   5. `this` 값을 context 안에서 결정한다.
4. 활성화 / 코드 실행 단계(Activation / Code Execution Stage)
   1. context 안의 함수를 실행 / 해석하고 코드를 한줄한줄 실행하면서 변수 값을 할당한다.

### 예: 각 단계 이후의 실행 컨텍스트 객체

```javascript
function foo(i) {
    var a = 'hello';
    var b = function privateB() {

    };
    function c() {

    }
}

foo(22);
```

생성 단계 이후의 실행 컨텍스트는 다음과 같다.

```javascript
fooExecutionContext = {
    scopeChain: { ... },
    variableObject: {
        arguments: {
            0: 22,
            length: 1
        },
        i: 22,
        c: pointer to function c()
        a: undefined,
        b: undefined
    },
    this: { ... }
}
```

실행 단계 이후의 실행 컨텍스트는 다음과 같다.

```javascript
fooExecutionContext = {
    scopeChain: { ... },
    variableObject: {
        arguments: {
            0: 22,
            length: 1
        },
        i: 22,
        c: pointer to function c()
        a: 'hello',
        b: pointer to function privateB()
    },
    this: { ... }
}
```

# Scope

Scope는 `variable object(VO)` 와 모든 부모 context의 VOs 를 포함한다.

```javascript
function one() {

    two();

    function two() {

        three();

        function three() {
            alert('I am at function three');
        }

    }

}

one();
```

이 예에서 `three()`의 스코프 체인은 다음과 같이 쓸 수 있다.

```
three() Scope Chain = [ [three() VO] + [two() VO] + [one() VO] + [Global VO] ];
```

## Lexical scope

Lexical scope라는 용어는 모든 내부 함수들이 정적으로(statically, or lexically) 부모 context에 묶여있다는 것을 말하는 복잡한 방법일 뿐이다.

이 lexical scope는 많은 개발자들에게 혼동의 근원이다. 함수를 불러오면 새로운 execution context가 생기고 그에 맞는 VO가 생기는 것을 위에서 배웠다. 시간이 지나면서 변하는 실시간으로 계산되는 VO가 어휘적으로(정적으로) 정의된 각 context의 스코프와 연관되면, 예상치 못한 결과가 일어나는데, 바로 이것이 혼란의 근원이다. 다음 예를 보자.

```javascript
var myAlerts = [];

for (var i = 0; i < 5; i++) {
    myAlerts.push(
        function inner() {
            alert(i);
        }
    );
}

myAlerts[0](); // 5
myAlerts[1](); // 5
myAlerts[2](); // 5
myAlerts[3](); // 5
myAlerts[4](); // 5
```

11번째 줄부터 있는 `myAlerts` 부분이 각각 0, 1, 2, 3, 4 를 출력해야할 것 같은데 그렇지 않다.

함수 `inner()` 는 for문 때문에 헷갈릴 수도 있지만, global context에 만들어져 있다. 따라서 이 함수의 스코프 사슬(scope chain)은 global context에 정적으로 묶여있다.

따라서 11번째 줄부터 `inner` 함수를 실행시킬 때, function execution context를 만들고, 생성 단계에서 `i` 를 찾게 되는데,  `i` 값은 global context에서 이미 `5` 로 바뀌어 있는 상황이기 때문에 이런 결과가 생긴 것이다.

# [클로저(Closures)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)

자바스크립트에서 함수는 클로져라는 것을 만든다. 클로져는 함수와 그 함수가 선언된 lexical environment의 조합이다.
이 환경은 클로저가 생성되던 시점에 스코프 안에 있었던 지역 변수들로 구성된다.

다음 예를 한번 살펴보자.

```javascript
function foo() {
    var a = 'private variable';
    return function bar() {
        alert(a);
    }
}

var callAlert = foo();

callAlert(); // private variable
```

여기서 글로벌 컨텍스트는 함수 `foo()` 와 함수의 반환 값을 갖고 있는 `callAllert`이라고 이름 붙여진 변수를 갖고 있다. 

여태까지처럼 이해해보면, `foo()` 함수의 함수 컨텍스트가 처리되고 난 뒤 변수 `a`는 사라져서 함수 `bar()`를 행할  때 문제가 생길 것 같다. 하지만 그렇지 않다. 바로 클로저(closure) 때문이다.

각 컨텍스트를 자세히 살펴보자.

```javascript
// Global Context when evaluated
global.VO = {
    foo: pointer to foo(),
    callAlert: returned value of global.VO.foo
    scopeChain: [global.VO]
}

// Foo Context when evaluated
foo.VO = {
    bar: pointer to bar(),
    a: 'private variable',
    scopeChain: [foo.VO, global.VO]
}

// Bar Context when evaluated
bar.VO = {
    scopeChain: [bar.VO, foo.VO, global.VO]
}
```

`callAlert()` 함수를  invoke 하면, 함수 `foo()`를 얻고 이 함수는 `bar()`를 향한 포인터를 반환한다. `bar()`의 함수 컨텍스트에 들어가면, `bar.VO.scopeChain` 은 `[bar.VO, foo.VO, global.VO]`다.

`a`를 `alert()` 함수에서 사용하기 위해서 인터프리터는 스코프체인에서 `a`를 찾는다. `bar.VO` 에서는 못찾았으므로 `foo.VO`로 넘어가고, 바로 여기서 `a`의 값을 찾게된다.

그러니까 클로저란 함수 컨텍스트가 생겼을 때, 그 함수의 바깥 컨텍스트 모두의 변수들이 모두 저장되어 있는 것을 의미한다.

# [IIFE](https://developer.mozilla.org/en-US/docs/Glossary/IIFE)

Immediately-Invoked Function Expression (즉시 작동되는 함수 표현)

여기서 'invoke' 는 "to put into effect or operation" (in [Merriam-Webster dictionary](https://www.merriam-webster.com/dictionary/invoke))의 뜻으로, "효과를 내거나 작동하도록 만드는 것"의 의미를 갖고 있다.

함수 선언(function declaration), 함수 표현(function expression)으로 만든 함수는 나중에 이 함수를 불러야 실행된다. 그러나 IIFE로 작성된 함수는 정의될 때 즉시 실행된다. 이렇게 작성된 함수의 내부에서 선언되거나 정의된 변수는 바깥에서 접근할 수 없다. 따라서 전역(global scope, 전체 영역)에 영향을 끼치지도 않아 scope를 깔끔하게 유지할 수 있다.

## 사용법

```javascript
(function () { /* ... */ })();
(function () { /* ... */ }());
(() => { /* ... */ })(); // ES6 arrow functions
// OR
!function () { /* ... */ }();
~function () { /* ... */ }();
-function () { /* ... */ }();
+function () { /* ... */ }();
void function () { /* ... */ }();
// OR
var f = function () { /* ... */ }();
true && function () { /* ... */ }();
0, function () { /* ... */ }();
// Passing variables
(function (a, b) { /* ... */ })("hello", "world");
// defensive semicolon
a = b + c
;function () {
	// code
}(); // to avoid being parsed as c()

```

# 다시, 문제로

이제 처음 이 공부를 시작하게 한 문제에 도전해보자. 먼 길을 돌아왔지만 전보다 자바스크립트를 잘 이해하게 되었다.

```javascript
for (var i = 0; i < prefixes.length; i++) {
  var prefix = prefixes[i];
  for (var length = 16; length <= 19; length++) {
    it('has a prefix of ' + prefix + ' and a length of ' + length,
      function() {
        detectNetwork(cardnum(prefix, length)).should.equal('China UnionPay');
      }
    );
  }
}
```

이제 여기서 문제가 생긴 이유를 이해할 수 있다. 요약하면, it 안에 정의된 함수는 클로저를 만들고, 여기에 사용된 두 변수, `prefix`와 `length`는 lexically(정적으로) 정의된 것이 아니기 때문에 문제가 된 것이다.

좀 더 자세히 설명해보자. 부모 컨텍스트 안에 정의된 `it()` 함수의 안에 새 함수를 정의했다. 이때 이 함수의 클로저가 생성되는데, 여기에는 `prefix`와 `length`가 포함된 스코프체인이 포함된다. 어쨌든 정의만 되고 아직 실행은 되지 않았으니 이 for문은 계속 진행되어 `i`와 `length`는 각각의 끝값에 도달해 있다.

이제 이 함수를 invoke하게 되면, 인터프리터는 `prefix`와 `length`를 찾기 시작한다. 스코프체인을 찾다보니, `prefix`와 `length`에 for loop에서 각각의 끝 값이 할당되어 있다. 이를 인터프리터가 처리하고 나면, 의도한 것과는 다르게 모두 같은 카드 번호 값(`cardnum`)에 대해서 테스트한 꼴이 되는 것이다.

## 해법 1: IIFE 사용

해법은 함수가 정의되는 순간 invoke하는 IIFE를 사용하는 것이다. 이러면 클로저에 포함된 두 인자가 값을 바꾸기 전에 함수를 실행할 수 있어, 의도한 대로 모든 경우의 수에 대해 테스트를 진행할 수 있게 된다.

```javascript
for (var i = 0; i < prefixes.length; i++) {
  var prefix = prefixes[i];
  for (var length = 16; length <= 19; length++) {
    (function(prefix, length) {
      it('has a prefix of ' + prefix + ' and a length of ' + length,
        function() {
          detectNetwork(cardnum(prefix, length)).should.equal('China UnionPay');
        });
    })(prefix, length);
  };
}
```

## 해법 2: `let` 키워드 사용

ES6에서 추가된 `let` 키워드는 블록 수준의 스코프를 만든다. 
아래와 같이 for문에 `let`을 사용하게되면, 반복할 때마다 새로운 바인딩(binding)을 생성한다.
그러니까 it의 클로저가 반복될 때마다 `i`, `prefix`, `length`의 다르게 만들어진다는 것을 의미한다.
따라서 위에서 IIFE로 해결했던 것과 마찬가지로 모든 경우의 수에 대해 테스트를 만들 수 있게 된다.


```javascript
for (let i = 0; i < prefixes.length; i++) {
  let prefix = prefixes[i];
  for (let length = 16; length <= 19; length++) {
    it('has a prefix of ' + prefix + ' and a length of ' + length, function() {
        detectNetwork(cardnum(prefix, length)).should.equal('China UnionPay');
    }); 
  }
}
```

# 참고한 내용


- [스코프 체인과 클로저](http://davidshariff.com/blog/javascript-scope-chain-and-closures/)
- [let 키워드](https://hacks.mozilla.org/2015/07/es6-in-depth-let-and-const/)