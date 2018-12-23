[TOC]

# 들어가면서

과제를 하다가 함수 정의를 for문 안에 했을 때 이상한 현상이 생겼다. 우선 다음 코드를 보자.

```javascript
for (let i = 0, prefix; i < prefixes.length; i++) {
  prefix = prefixes[i];
  for (let length = 16; length <= 19; length++) {
    it('has a prefix of ' + prefix + ' and a length of ' + length,
      function() {
        detectNetwork(cardnum(prefix, length)).should.equal('China UnionPay');
      }
    );
  }
}
```

`detectNetwork()`라는 함수의 결과를 테스트하는 코드다. 그래서 `prefix`와 `length`의 모든 경우에 대해 테스트를 하려고 for loop을 돌렸는데, 테스트가 모든 경우의 수를 다루지 않았다는 메시지를 받았다.

이 이상한 문제를 해결하려면 IIFE라는 것을 이용해야한다고 했는데, 다음 코드는IIFE를 사용한 것으로 문제를 해결한다.

```javascript
for (let i = 0, prefix; i < prefixes.length; i++) {
  prefix = prefixes[i];
  for (let length = 16; length <= 19; length++) {
    (function(prefix, length) {
      it('has a prefix of ' + prefix + ' and a length of ' + length,
        function() {
          detectNetwork(cardnum(prefix, length)).should.equal('China UnionPay');
        });
    })(prefix, length);
  };
}
```

왜 IIFE를 써야하는 것인지 알기위해 공부를 해봤다.



# 실행 컨텍스트와 스택 (Execution Context and Stack)

이 문제를 이해하기 위해서는 자바 스크립트의 다음 개념들을 이해할 필요가 있다.

- 스코프(scope)
- 클로저(closure)
- IIFE(Immediately-invoked function expression)

이 모든 것을 이해하기 전에, 자바스크립트의 인터프리터(interpreter)가 어떤 방식으로 작동하는지 먼저 알 필요가 있다.

여기 정리한 내용은 [이 글](http://davidshariff.com/blog/what-is-the-execution-context-in-javascript/)을 보고 이해한 것을 내 나름대로 이해해 정리한 것이다. 따라서 여기에 서술된 내용은 정확한 사실이라기보다는 "이 글을 보고 내가 이해한 것에 따르면," 이라는 단서가 붙여진 것으로 이해하는 것이 적절할 것이다.



## 스코프

global execution context는 하나만 존재한다.

어떤 함수를 선언할 때마다 하나의 execution context가 만들어진다.

각각의 execution context는 "개념적"으로 다음과 같은 객체로 나타낼 수 있다.

```javascript
executionContextObj = {
    'scopeChain': { /* variableObject + all parent execution context's variableObject */ },
    'variableObject': { /* function arguments / parameters, inner variable and function declarations */ },
    'this': {}
}
```



## Execution Context Stack

자바스크립트 인터프리터는 기본적으로 싱글 스레드(single thread) 방식이다. 그러니까 한번에 하나의 Execution context를 처리할 수 밖에 없다. 이때 execution context를 올려두는 queue를 execution context stack 이라 한다.

execution context를 stack에서 처리할 때는 두가지 단계를 거친다.

1. Creation Stage (함수가 어딘가에서 불려지고, 아직 그 안의 어떤 코드도 실행되기 전)
   1. Scope chain 생성
   2. 함수, argument, 변수 생성
   3. "this" 값 결정
2. Activation / Code Execution Stage
   1. 값/레퍼런스 할당
   2. 코드 해석 및 실행

## 인터프리터가 코드를 검토하는 방식의 개요

1. 함수를 작동시키기위한 코드를 찾는다.
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



### 예: 각 단계 이후의 execution context object

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

생성 단계 이후의 execution context:

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

실행 단계 이후의 execution context:

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

이 부분의 내용은 위의 내용을 가져온 블로그의 [다음 포스트 내용](http://davidshariff.com/blog/javascript-scope-chain-and-closures/)을 내가 이해한 대로 정리한 것이다. 역시 주의하면서 읽는 것이 좋다. 중간 중간 [mdn에서 본 글](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)에 나온 예제를 놓고, 블로그에서 배운대로 분석해놓은 내용도 함께 있으니 참고하시길 바란다.

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

이 lexical scope는 많은 개발자들에게 혼동의 근원이다. 함수를 불러오면 새로운 execution context가 생기고 그에 맞는 VO가 생기는 것을 위에서 배웠다. 시간이 지나면서 변하는 실시간으로 계산되는 VO가 어휘적으로(정적으로)  정의된 각 context의 스코프와 연관되면, 예상치 못한 결과가 일어나는데, 바로 이것이 혼란의 근원이다. 다음 예를 보자.

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



# [Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)

자바스크립트에서 함수는 클로져라는 것을 만든다. 클로져는 함수와 그 함수가 선언된 lexical environment의 조합이다.  이 환경은 클로저가 생성되던 시점에 스코프 안에 있었던 지역 변수들로 구성된다.

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

이제 처음 이 공부를 시작하게 한 문제에 도전해보자. 먼 길을 돌아왔지만 전보다 훨씬 자바스크립트에 대해 이해하게 되었다.

```javascript
for (let i = 0, prefix; i < prefixes.length; i++) {
  prefix = prefixes[i];
  for (let length = 16; length <= 19; length++) {
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

이제 이 함수를 invoke하게 되면, 인터프리터는 `prefix`와 `length`를 찾기 시작한다. 스코프체인을 찾다보니, `prefix`와 `length`에 각각의 끝값이 할당되어 있다. 이를 인터프리터가 처리하고 나면, 의도한 것과는 다르게 모두 같은 카드 번호 값(`cardnum`)에 대해서 테스트한 꼴이 되는 것이다.

## 해법: IIFE 사용

해법은 함수가 정의되는 순간 invoke하는 IIFE를 사용하는 것이다. 이러면 클로저에 포함된 두 인자가 값을 바꾸기 전에 함수를 실행할 수 있어, 의도한 대로 모든 경우의 수에 대해 테스트를 진행할 수 있게 된다.

```javascript
for (let i = 0, prefix; i < prefixes.length; i++) {
  prefix = prefixes[i];
  for (let length = 16; length <= 19; length++) {
    (function(prefix, length) {
      it('has a prefix of ' + prefix + ' and a length of ' + length,
        function() {
          detectNetwork(cardnum(prefix, length)).should.equal('China UnionPay');
        });
    })(prefix, length);
  };
}
```

