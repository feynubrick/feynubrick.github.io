---
layout: post
title: "자바스크립트 공부 // 스코프(Scope), 클로저(Closure), 즉시실행함수(IIFE)"
comments: true
author: feynubrick
date:   2019-01-11T14:00:00Z
tags: [JavaScript, Study]
---

# 들어가면서

과제를 하다가 for문 안에 함수 정의를 했을 때 이상한 현상이 생겼습니다.
우선 다음 코드를 보시죠.

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

이 코드는 `detectNetwork()`라는 함수를 만들고, 이 함수의 결과를 테스트하기 위해 작성한 것입니다.
`it` 함수는 "mocha"라는 자바스크립트 테스트 프레임워크에서 테스트 항목을 등록하는 역할을 하는데요.
이 함수 안에 정의된 함수는 그 앞의 문자열과 연결되고, 등록하는 시점이 아니라 나중에 실행됩니다.
참고로 이 안에 사용된 `cardnum()` 함수는 인자 두개를 이용해 테스트용 카드번호를 문자열로 만들고 반환하는 함수입니다.

제가 이 코드를 작성하며 의도한 것은 `prefixes`라는 배열에 담긴 값들과 `length`의 정해진 범위에서 만들어지는 모든 카드번호 조합을 자동으로 테스트하는 것입니다.
손으로 다 직접 치기에는 너무 귀찮으니 말이죠.

그러나 제가 의도한 대로 되지는 않았습니다.
테스트가 모든 경우의 수에대해 이루어지지 않았다고 합니다.
위의 코드를 그냥 보면 모든 조합을 테스트로 등록한 것 같았는데, 무엇이 문제였으며 어떻게 제가 의도한 대로 만들 수 있을까요?

# 문제 이해하기

이 문제를 이해하고 해결하기 위해서는 자바 스크립트의 다음 개념들을 이해할 필요가 있습니다.

- 실행컨텍스트(execution context)
- 스코프(Scope)
- 클로저(closure)
- IIFE(Immediately-invoked function expression)

저는 원리를 통해 이해하는 것이 혼동도 적고 기억도 잘나는 좋은 방법이라고 생각합니다.
그래서 이 글에서는 자바스크립트 엔진의 작동방식을 중심으로 이 개념들을 이해해보도록 하겠습니다.
물론, 문제도 해결해야겠죠!

## 자바스크립트 엔진

우리가 자바스크립트 코드를 짜면 이것을 텍스트 파일로 저장하게 돼죠.
그리고 이것을 자바스크립트 엔진이 들어가 있는 브라우저에 넣게 되는데요.
이 엔진이 이 텍스트 파일을 해석하고 실행하게 됩니다.
사람이 이해할 수 있는 텍스트를 컴퓨터가 이해할 수 있는 기계어로 "번역"하는 것이죠.
이런 과정을 컴파일(compile)이라고 합니다.

제가 배웠던 C 언어는 텍스트 파일을 컴파일러(compiler)를 사용해 컴파일하면 실행 가능한 파일이 나왔는데요.
즉, 텍스트 파일로 작성된 C 언어 코드를 컴파일 한 뒤에야 비로소 코드를 실행할 수 있습니다.

자바스크립트는 이런 방식으로 코드를 컴파일하지 않고, 코드를 실행하면서 줄마다(line by line) 컴파일하는 방식을 사용합니다.
이런 컴파일러를 JIT 컴파일러(Just-In-Time compiler)라고 하는데요.
그래서 자바스크립트를 인터프리터 언어(interpreted language)라고 흔히들 부릅니다.

## 실행컨텍스트(Execution context)

실행컨텍스트는 자바스크립트 코드가 엔진에 의해 실행되는 환경을 말하는 추상적인(abstract) 개념입니다.
그러니까 실재하지 않고 이해를 돕기 위한 개념이라는 것이죠.

ES5 공식문서에는 다음과 같이 나와 있습니다.
> "연관된 코드가 실행되는 진행 상태를 따라가기 위해 꼭 필요한 상태면 어떤 것이든지 포함한다"

그러니까 코드를 실행시키는데 필요한 것들을 몽땅 통틀어 실행컨텍스트라고 하는 것입니다.

엔진은 코드를 실행할 때 가장 먼저 전역 실행컨텍스트(global execution context)를 만드는데,
이것은 코드가 종료될 때까지 하나만 존재합니다.
코드 안에서 함수를 불러올(function call을 발견할) 때마다 하나의 실행컨텍스트가 만들어집니다.
이 실행컨텍스트를 함수 실행컨텍스트(functional execution context)라 부릅니다.

그럼 이 실행컨텍스트는 어떻게 처리될까요?

자바스크립트 인터프리터는 기본적으로 싱글 스레드(single thread) 방식입니다.
그러니까 한번에 하나의 실행컨텍스트만 처리된다는 말이죠.

이때 실행컨텍스트가 차곡차곡 쌓이는 곳을 실행컨텍스트 스택이라 하는데요.
실행컨텍스트를 스택에서 처리할 때는 크게 두가지 단계를 거칩니다.

1. 생성 단계(Creation Stage)
2. 활성화 / 코드 실행 단계(Activation / Code Execution Stage)

생성 단계에서는 다음의 세가지 일이 일어납니다.

1. "ThisBinding"의 상태 요소(state component)가 결정됨
2. "LexicalEnvironment" 상태 요소가 결정됨
3. "VariableEnvironment" 상태 요소가 결정됨

첫번째, ThisBinding 상태 요소는 현재 실행컨텍스트의 `this` 키워드의 값을 말한다는 것만 알아두고,
자세한건 다른 포스트를 참조하시면 될 것 같습니다.

두번째인 LexicalEnvironment는 차차 알아보기로 합시다.

세번째인 VariableEnvironment는 LexicalEnvironment와 거의 같은데요.
`with` 문을 사용할 때만 이 둘은 서로 달라집니다.
`with` 문은 [MDN 문서](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/with)에서 혼동을 줄수 있다는 이유로 사용을 추천하지 않고 있으니 그냥 그렇다고만 알고 넘어가도록 합시다.

## 어휘 환경(Lexical Environment)

생성단계의 두번째에서 LexcialEnvironment의 상태요소가 결정된다고 말했는데요.
변수, 함수 각각을 식별하는데 사용되는 식별자와 그 식별자의 어휘적인 계층구조(lexical nesting structure)가 저장되는 곳입니다.
이것도 실행컨텍스트와 마찬가지로 추상적인 개념입니다.
이 어휘 구조는 코드를 엔진이 처음 해석하려고 시도하는 순간인, "lexing time"에 결정됩니다.
그러니까 말 그대로 어휘를 쓴 대로 구조를 갖는다는 말인 것이죠.

이 환경에서는 식별자 해소(identifier resolution)가 일어나는데요.
이 식별자 해소라는 것은 풀어서 설명하면 다음과 같습니다.

우리가 코드에 적는 변수(함수)명은 사실 어떤 메모리 주소를 상징하는 것입니다.
이 변수명은 메모리 공간의 주소를 식별하는 용도로 사용되기 때문에 식별자로 불리는데, 주소와 짝지어져서 어떤 메모리 공간에 저장됩니다.
나중에 이 메모리 주소를 참조하기 위해 식별자를 사용하면, 이 식별자가 가리키는 것이 무엇인지 찾는 작업이 필요하겠죠?
이 작업이 완료되면 식별자가 가리키는 주소를 찾는 것이기 때문에 식별자 해소라고 부릅니다.

어휘적 계층구조는 말이 좀 복잡하긴 하지만 다음의 예를 보면 쉽게 알 수 있습니다.
흔히 말하는 스코프 체인(scope chain)입니다.

```javascript
let foo = function (a) { 
  let b = 10;  // 'b' 와 'bar' 는 'foo'의 어휘구조 안에 있습니다.
  let bar = function(a) {
    let c = 20;        // 하지만, 'c'는 'bar'의 어휘 구조 안에서만 존재합니다. 
    return a + b + c;  // 왜냐하면 'c'가 'bar' 함수 안에 써 있기 때문이죠.
  };
};
```

어휘 환경은 다음의 두가지 요소로 이루어져 있습니다.

- 환경 기록(environment record)
- 바깥 환경 참조(reference to the outer environment)

환경 기록은 변수와 함수 선언이 저장되는 곳이고, 바깥 환경 참조는 이 어휘 환경을 둘러싼 다른 어휘 환경을 가리킵니다.
환경 기록을 사용해서 못하면 바깥 환경 참조를 반복해가면서 식별자 해소를 수행하게 됩니다.

## 스코프(Scope)

어휘 환경보다는 스코프라는 말이 더 익숙할 것입니다.
자주 사용되는 용어이므로 여기서 정리하고 넘어가도록 하겠습니다.
스코프를 잘 아시면, 읽지 않으셔도 상관 없습니다.

여기서는 스코프의 성질을 중심으로 설명하도록 하겠습니다.
어휘 환경을 이해했다면 이 모든 성질들이 왜 생기는 것인지 이해할 수 있을 것입니다.

스코프는 변수 접근 규칙에 따른 식별자의 유효범위를 지칭하는 용어입니다.
스코프는 중첩이 가능합니다만, 집합개념으로 표현하면, 교집합이 없이 중첩됩니다.
그러니까 스코프가 하위 구조를 가질 수 있다는 말이고,
위에서 언급했듯이 이것을 스코프 체인(scope chain)이라고 부릅니다.

이 체인에서 하위 스코프는 상위 스코프의 변수에 접근할 수 있지만, 그 반대는 불가능합니다.
사방이 막힌 방을 생각하면 도움이 되겠네요.
이 방을 하위 스코프라고 합시다.
제가 만약 방 안에 있다면, 방 안의 의자가 어떤 모양이고 벽지는 무슨 색인지, 개미가 있는지 알 수 있습니다.
그런데 방 밖에 있었다면, 이게 얼마나 넓은 방인지 뭐가 들어있는지 전혀 알 수가 없을 것이겠죠.

### 지역 스코프와 전역 스코프(local scope and global scope)

전역 스코프(global scope)는 이 스코프 체인 구조의 최상위에 위치하는 스코프입니다.

자바스크립트에서는 함수가 선언되는 동시에 자신만의 스코프를 가지는데요.
이런 스코프를 지역 스코프(local scope)라 부릅니다.
하지만 이 스코프의 상위에 어떤 것이 위치하는지는 함수가 어떤 흐름에서 호출되었는가와는 상관없이 코드에 기록된 대로 정해집니다.
이것을 어휘적 스코프(lexical scope)라고 부릅니다.

전역 스코프와 지역 스코프 각각에서 선언된 변수를 각각 전역 변수, 지역 변수라고 부르는데요.
지역 변수는 함수 내에서 전역변수보다 높은 우선순위를 갖습니다.
이게 무슨 얘기인지는 다음 예를 보시면 이해하실 수 있을 겁니다.

```javascript
var movieName = 'Arrival';

var callNameInKorea = function () {
  var movieName = '컨텍트';
  console.log(movieName);
}

var callNameInGlobal = function () {
  console.log(movieName);
}

callNameInKorea(); // "컨텍트"
callNameInGlobal(); // "Arrival"
```

2017년 개봉했던 영화 "컨텍트"는 제작 국가인 미국에서 제목이 "Arrival"이었습니다.
무슨 이유인지는 모르겠습니다만, 국내 배급사가 영화 제목을 컨텍트로 바꿔버려 이 영화를 언급할 때마다 조디 포스터 주연의 1997년 작인 "콘텍트(Contact)"와 구분하느라 애써야 됐습니다. 아무튼 두 영화 다 매우 좋은 영화이니 보시는 것을 추천해드립니다.

얘기가 좀 샜지만 다시 돌아와서, 위의 예를 살펴봅시다.
`movieName`은 전역 스코프와 `callNameInKorea` 함수의 지역 스코프에 하나씩 선언되어 있습니다.
이 지역 스코프에서 `movieName`에 저장된 값을 출력하면, 지역 변수의 우선순위가 전역 변수의 우선순위 보다 높기 때문에 `"컨텍트"`가 출력됩니다.

### 블록 수준의 스코프(block level scope)와 함수 수준의 스코프(function level scope)

블록마다 스코프가 형성되는 다른 언어와 달리 자바스크립트에서는 스코프가 함수마다 만들어집니다.
그러니까 자바스크립트는 블록 수준의 스코프가 아니라 함수 수준의 스코프 규칙을 따릅니다.

ES6에서는 이렇게 블록 수준의 스코프를 만들 수 있도록 `let`과 `const`라는 키워드가 추가되었습니다.
그러니까 블록 수준의 스코프를 원하면 이 두 키워드를 통해 만들 수 있습니다.

### 전역변수, window 객체

전역변수는 모든 함수의 스코프에서 접근할 수 있습니다.
브라우저에서 전역 변수들은 최상위 스코프, 그러니까 전역 스코프를 대표하는 `window` 객체에 들어 있습니다.

그리고 하나 주의할 것은 변수를 선언 없이 사용하면,
그러니까 코드에서 `var`를 한번도 사용하지 않고 사용한 변수는 전역변수로 취급됩니다.
에러가 나면 항상 고쳐야하니 신경쓰지 않아도 괜찮지만, 자바스크립트 엔진은 에러를 내보내지 않습니다.
뭐, 설정에 따라 다르긴 하겠습니다만...
어쨌든 의도하지 않게 변수가 전역 변수로 취급되면 여러 문제가 생길 수 있습니다.
의도했더라도 다른 사람이 볼 때는 헷갈릴 수 있겠죠.
항상 변수를 꼼꼼히 선언하는 습관을 들이는 것이 좋을 것 같네요.

### Hoisting

변수 선언은 범위에 따라 선언과 할당으로 분리됩니다.
그러니까 변수 선언을 scope에 상단으로 끌어올리고 나서 모든 계산을 진행합니다.

```javascript
console.log(number);
var number = 1;
```

위의 예를 실행하면, `undefined`가 출력됩니다.
호이스팅(hoisting)은 다음처럼 변수 선언을 스코프의 상단으로 끌어 올리는(hoist) 것을 의미합니다..

```javascript
var number;
console.log(number);
number = 1;
```

따라서 콘솔에 `number`를 출력하는 부분이 할당 이전에 있기 때문에 `undefined`가 출력되는 것입니다.

함수의 경우, 두가지 방식으로 만들 수 있는데요.
선언식과 표현식입니다.

```javascript
// 표현식
var printA = function() {
  // function content
}

// 선언식
function printB() {
  // function content
}
```

표현식의 경우 변수 선언과 같은 형식으로 되어있으므로, 호이스팅이 일어납니다.
하지만 선언식은 항상 스코프의 상단에서 처리됩니다.
그러니까 다음과 같이 말이죠.

```javascript
// 선언식
function printB() {
  // function content
}

var printA;

// 표현식
printA = function() {
  //function content
}
```

## 클로저(Closures)

위에서 실행컨텍스트가 처리될 때, 거치는 두단계 중 두번째는 실행 단계였습니다.
실행 단계가 끝나면 실행컨텍스트는 종료되고 소멸하는데요.
이 실행컨텍스트에 속한 어휘 환경은 어떻게 될까요?

처음 생각해보기엔 실행컨텍스트가 소멸하면 같이 없어질 것 같다는 생각이 듭니다.
그러나 이는 반만 맞는 생각입니다.
사실 실행컨텍스트가 소멸되면 어휘 환경은 같이 소멸됩니다.
단, 이 어휘 환경의 식별자가 다른 어휘 환경에서 참조되지 않는다면 말이죠.

이런 것 때문에 만들어지는 것이 바로 클로져입니다.
그래서 [MDN 페이지](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)에서 다음과 같이 설명합니다.

> "클로저는 함수와 그 함수가 선언된 어휘 환경의 조합입니다"

다음 예를 보면서 클로저를 이해해봅시다.

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

여기서 전역 실행컨텍스트는 함수 `foo()` 와 함수의 반환 값을 갖고 있는 `callAllert`이라고 이름 붙여진 변수를 갖고 있습니다.

처음 `foo` 함수를 불러오는 곳을 살펴봅시다.
`foo` 함수의 실행컨텍스트가 만들어집니다.
이 실행 컨텍스트의 생성 단계에서 `foo` 함수 내부에 선언된 변수들이 어휘 환경의 어휘 기록에 들어있습니다.
바깥 환경 참조에는 전역(global) 어휘 환경이 들어있겠죠.

이제 실행 단계에 들어가서 함수 `bar`를 반환합니다.
전역변수인 `callAlert`는 `bar` 함수를 가리키게 됩니다.
그리고 이 단계가 끝나면 `foo`함수의 실행컨텍스트는 소멸합니다.
하지만 살펴보면 `bar` 함수에서 이 어휘 환경을 참조하고 있습니다.
따라서 이 어휘 환경은 소멸되지 않고 남게 됩니다.

이제 이 `callAlert` 함수를 불러오면,
참조된 어휘 환경을 통해 식별자 `a` 가 가리키는 값인 `'private variable'`을 의도대로 참조할 수 있게 됩니다.

# 다시, 문제로

처음에 이 공부를 시작하게 했던 문제로 돌아갑시다.
참 먼 길을 돌아왔네요.

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

이제 어떤 문제가 생겼던 것인지 감이 올 것입니다.
`it` 함수에 등록된 함수가 정의될 때, 식별자 `prefix`, `length`를 통해 참조하고 있는 어휘 환경은 `i`, `prefix`, `length`가 선언된 어휘 환경입니다.

그런데 나중에 이 함수를 실행하는 시점에서 보면 이 어휘 환경의 `i`, `prefix`, `length` 값은 각각 `prefixes.length`, `prefixes[prefixes.length]`, `20`가 되어 있습니다.

따라서 for loop을 돈 횟수만큼 정의된 함수들이 모두 이 끝 값들로 테스트를 진행한 것이죠.
이래서 문제가 생긴 것이었습니다.

문제를 알았으니 이젠 어떻게 해결하면 될까요?

## 해법 1: IIFE 사용

### 즉시실행함수(IIFE)

[Immediately-Invoked Function Expression](https://developer.mozilla.org/en-US/docs/Glossary/IIFE) (IIFE, 즉시 작동되는 함수 표현)

여기서 'invoke'는 영영 사전을 찾아보면 다음과 같은 뜻으로, "효과를 내거나 작동하도록 만드는 것"의 의미를 갖고 있습니다.

> "to put into effect or operation" 
> (in [Merriam-Webster dictionary](https://www.merriam-webster.com/dictionary/invoke))

즉시 작동하게 만드는 함수 표현이라는 말입니다.
그런데 즉시라는게 어떤 때를 말하는 걸까요?
바로 정의되는 그 시점을 말합니다.

함수 선언(function declaration), 함수 표현(function expression)으로 만든 함수는 나중에 이 함수를 불러야 실행되는데요.
그러나 IIFE로 작성된 함수는 정의될 때 즉시 실행됩니다.

IIFE는 다음과 같은 표현을 말합니다.

```javascript
(function () { /* ... */ })();
```

이 표현을 사용해서 문제를 해결해봅시다.
함수가 정의되는 순간 invoke하는 IIFE를 사용해서 말이죠.
이러면 클로저에 포함된 변수들이 값을 바꾸기 전에 함수를 실행할 수 있어, 의도한 대로 모든 경우의 수에 대해 테스트를 진행할 수 있게 되겠죠.

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

자, 제출해보니 통과됩니다. 문제가 해결됐습니다!

## 해법 2: `let` 키워드 사용

두번째 해법은 ES6에서 추가된 `let` 키워드를 사용하는 것입니다.
이 키워드는 블록 수준의 스코프를 만드는데요.
즉, 어휘 환경을 `let` 키워드가 사용된 블록에 대해 새로 하나 만드는 것입니다.
따라서 아래와 같이 for문에 `let`을 사용하게되면, 반복할 때마다 새로운 어휘 환경이 생성됩니다.
그러니까 `it`이 반복될 때마다 `it` 내부에 정의된 함수의 클로저가 다르게 만들어진다는 것이죠.

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

이 경우에도 제출해보니 잘 통과됩니다.

# 도움받은 내용

이 글에서 정리된 내용은 구글링을 통해 찾은 여러 페이지의 내용을 제 문제를 해결하는 데 맞게 짜깁기한 것입니다.
자바스크립트 초보인 제가 알 수 없는 내용은 다음의 페이지들을 만든 "거인의 어깨 위에 올라서서" 알게된 것입니다.
문제를 제외한 모든 예제는 다음 글에서 따온 것입니다.

더 자세하고 정확한 공부는 다음 링크들을 참조하시는 것이 좋습니다.

- 코드스테이츠 프리코스 강의
- [스코프 체인과 클로저](http://davidshariff.com/blog/javascript-scope-chain-and-closures/)
- [실행컨텍스트](http://davidshariff.com/blog/what-is-the-execution-context-in-javascript/)
- [실행 컨텍스트와 어휘 환경](https://medium.com/dailyjs/javascript-basics-the-execution-context-and-the-lexical-environment-3505d4fe1be2)
- [let 키워드](https://hacks.mozilla.org/2015/07/es6-in-depth-let-and-const/)
