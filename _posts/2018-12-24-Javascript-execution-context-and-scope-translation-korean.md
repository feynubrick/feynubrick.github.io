---
layout: post
comments: true
title: 자바스크립트 // 참고. 실행 컨텍스트와 스코프 [번역]
author: feynubrick
date:   2018-12-24
---

자바스크립트를 배우다 만나게 되는 첫번째 난관. 스코프와 실행 컨텍스트라는 개념이다.
이 개념을 더 잘 이해해 보기 위해 검색을 하다가 [좋은 내용이 들어있는 글](https://weeklywebwisdom.com/2017/09/08/javascript-execution-context-and-scope/)을 발견했다.
영어로된 글이라서 나 자신에게 도움이 될까 해서 공부하는 겸 번역을 해봤다.
이 과정에서 내용을 이해하려해보니 실행 컨텍스트와 스코프에 대해 조금 더 잘 이해할 수 있게 되었다.

필자는 영어를 잘하는 것은 아니라 읽으면서 매끄럽지 않고, 더 심각하게는 잘못 번역한 내용이 있을 수도 있으니, 원문을 주로 읽으시고 이 조잡한 번역글은 참고만 하시는 것을 추천드린다.

# 실행 컨텍스트와 스코프에 대해 뭘 알고 있나요?

시작하기 전에, 재밌는 예제를 풀고 시작해보자. 아래의 코드를 보자. 그리고 아래로 스크롤을 내리지 않고, 표시된 6개 줄 각각에서 프로그램이 어떤 것을 출력할지 예상해보라.

```javascript
var myString = "I'm outside";

function f() {
    console.log(myString); // 2
    var myString = "I'm in function";
    console.log(myString); // 3
 
    for (var i = 0; i < 1; ++i) {
        var myString = "I'm in for";
        console.log(myString); // 4
    }
    console.log(myString); // 5
 }

console.log(myString); // 1 
f();
console.log(myString); // 6
```



그럼 결과를 확인하자.

```
1. "I'm outside" 
2. undefined
3. "I'm in function"
4. "I'm in for"
5. "I'm in for" 
6. "I'm outside"
```



확실히 자바스크립트는 지루할 틈이 없다. 당신은 진정한 컨텍스트의 달인이라 이 글에 나온 내용은 식은 죽 먹기일 것이다. 만점을 받았다면 말이다. 아니라면, 틀린 번호를 기억해두시라. 나중에 설명하면서 이 번호들을 다시한번 찾게될 것이다.



# 실행 컨텍스트(execution context)

실행 컨텍스트(execution context)는 기본적으로 코드가 실행되는 환경이다. 어떤 순간에든 활성화된 실행 컨텍스트는 다음 중 하나가 될 수 있다.

- global code: 가장 바깥. 이 컨텍스트를 나타내는 객체는 어떤 호스트 환경이냐에 따라 달라진다. 브라우저에서 global scope는 `window` 객체로 나타내진다.
- function code: 각각의 함수 호출은 자신의 실행 컨텍스트를 갖는다. 글로벌 코드 또는 다른 함수 안에서 함수가 호출될 때마다 새 컨텍스트가 만들어진다. 
- eval code: `eval()` 명령을 실행시키면 새 실행 컨텍스트를 만든다. `eval()`을 사용하는 것은 보통 추천하지 않는다. 성능과 발생할 수 있는 보안의 여러 문제 때문이다. 따라서 이 글에서는 다루지 않는다. 

모든 실행 컨텍스트는 이에 속하는 모든 변수와 함수가 들어있는 변수 객체와 관련되어 있다. 이 변수 객체는 코드에서 접근할 수는 없지만 데이터를 다루는 뒷단에서 사용된다.



# 자바스크립트의 실행 흐름(context flow)

자바스크립트의 실행 흐름은 스택(stack)같다. 스택에 가장 바닥에는 항상 글로벌 컨텍스트가 있다. 이 글로벌 컨텍스트는 프로그램이 끝날 때만 파괴된다.

새로운 함수가 호출될 때마다, 새로운 컨텍스트가 만들어지고 스택의 제일 위에 놓여진다. 어떤 시점에서든, 스택 안에는 호출되었지만 아직 실행이 완료되지 않은 함수가 있을 것이다.

```javascript
function printer(index) {
    if (index === 0) {
        return;
    }
    printer(index - 1);
    console.log(index);
}

function main(index) {
    printer(index);
}

main(2);
```



이 코드가 실행되면 실행 흐름은 어떻게 보일까?

- 프로그램이 시작하고 `main(2)` 함수를 호출하기 전에 우리는 달랑 글로벌 컨텍스트 하나만 스택에 갖고 있다.
- `main(2)`를 호출하면, 또 다른 컨텍스트가 제일 위에 놓이게 된다.
- `main` 함수 안에서, `printer` 함수를 호출했다. 따라서 새로운 컨텍스트가 스택의 제일 위에 간다.
- `printer` 함수 안에 들어갔다. `index`가 2이기 때문에 if를 지나치고 `printer`의 또 다른 호출을 찾는다. 또 다른 컨텍스트가 생성된다.
- > !!! 새로운 컨텍스트는 함수가 자기 자신을 호출해도 생성된다. 왜냐하면 컨텍스트는 함수 호출과 연결된 것이지 함수 그 자체와 연결된 것은 아니기 때문이다. 
- 이번에 호출되는 `printer` 함수는 `index`로 `1`을 갖고 있다. 따라서 이 새 컨텍스트에서 `index`는 `1`이 될 것이다.
- `printer`를 다시 `index` 를 `0`으로 해서 호출한다. 새로운 컨텍스트가 만들어진다.
- 이번에는, `if (index === 0)` 문 안에서 함수가 반환한다. 따라서 스택에서 가장 마지막 항목이 빠진다.
- `index`가 `1`인 `printer`함수로 돌아가고 `index`가 출력된다.
- 이 함수에서도 실행이 종료되었으므로 이 컨텍스트는 빠진다.
- 이제 `printer` 함수의 `index = 2`인 첫번째 호출에 있다.
- `2`가 출력되고 함수가 끝난다. `main`으로 돌아간다.
- `main`으로 부터 `index`의 값인 2를 출력하고 호출을 끝낸다.
- `main`을 완료하고 나서 달랑 글로벌 실행 컨텍스트 하나만 남는다. 프로그램 자체가 종료되면 이것도 스택에서 빠지게된다.

함수가 `var`를 사용해 선언되었을 때는 가능한 가장 가까운 시간에 만들어진 컨텍스트에 추가된다. 함수 안에서, 그건 함수의 지역(local) 컨텍스트가 될 것이다.

# 블록 수준(block level)의 컨텍스트는 없다

실행 컨텍스트를 생각할 때 발생하는 혼란의 주 근원지 중 하나는 자바스크립트에 블록 수준의 실행 컨텍스트가 없다는 것이다. 예를 들어, C++, Java, C# 같은 다른 주류 언어에서, for 문 (또는 while이나 다른 블록) 안에 변수를 선언하면, 그 변수를 블록이 끝나고 나서는 접근할 수 없다(그 블록의 스코프에 속해있기 때문). 이런 일은 자바스크립트에서는 일어나지 않는다. 따라서 위 예제의 (5)번이 헷갈릴 수 있다.

EcmaScript2015에서 블록 안에서만 보이는 변수를 선언하는 키워드 `let`이 소개되었다. `let`에 의해 선언된 변수는 자신이 정의된 블록을 자신의 스코프로 갖는다. 이는 포함된 하위 블록들(sub-blocks)에서도 적용된다. 이 방법으로, `let`은 `var`와 아주 비슷하게 작동한다. 가장 큰 차이는 `var` 변수의 스코프는 이 변수를 담고 있는 함수 전체라는 것이다.

```javascript
function varTest() {
  var x = 1;
  if (true) {
    var x = 2;  // same variable!
    console.log(x);  // 2
  }
  console.log(x);  // 2
}

function letTest() {
  let x = 1;
  if (true) {
    let x = 2;  // different variable
    console.log(x);  // 2
  }
  console.log(x);  // 1
}
```

# 변수 스코프(variable scope)

스코프는 변수의 접근가능여부(보이는지 아닌지)를 결정한다. 자바스크립트에는 2개 유형의 스코프가 있다.

- 전역: 코드의 바깥쪽
- 지역: 함수의 안쪽

함수가 호출될 때마다, 새로운 스코프가 만들어진다. 지역 변수들은 **지역 스코프**를 갖는다. 즉, 이것들은 함수 안에서만 접근 가능하다.

변수가 `var`를 사용해서 선언될 때는 자동으로 가장 가까운 스코프에 더해진다. 함수에서는 가장 가까운 것은 함수의 지역 컨텍스트일 것이다. 지역 변수들은 그 함수 내에서만 인식되기 때문에, 같은 이름을 가진 변수들은 다른 함수들에서 사용될 수 있다.

각 함수는 자신의 스코프를 갖는다. 그리고 함수 내에서 선언된 모든 변수는 그 함수 내에서만 접근 가능하다. 하지만 전역 변수는 함수 안에서 사용될 수 있다. 그 함수 안에서 같은 식별자를 갖는 또다른 변수가 있지 않다면 말이다.

하지만 함수 안에 또 함수가 있는 경우에는 어떤 일이 일어날까? 다음의 예를 보자.

```javascript
var global = 1;
var mainVariable = 0;

function main() {
    var mainVariable = 2;
    function nested() { 
        var nestedVariable = 3;
        console.log(nestedVariable);
        console.log(mainVariable); 
        console.log(global);
   }
   nested();
}

main();
```

이 프로그램을 실행하면, 순서대로 3, 2, 1을 출력할 것이다. 함수 안 함수 구조는 `global` 변수와 `main`함수 안의 `mainVariable` 변수에 접근할 수 있는 것을 볼 수 있다. 심지어 이 둘이 같은 컨텍스트에 속하지도 않는데 말이다. 어떻게 이것이 가능한가? 그 답은 스코프 체인(scope chain)이다.

# 스코프 체인(scope chain)

코드가 컨텍스트 안에서 실행될 때, 변수의 스코프 체인이 만들어진다. 스코프 체인의 목적은 실행 컨텍스트가 접근할 수 있는 모든 변수와 함수에 정돈된 방식으로 접근하는 것이다. 스코프 체인의 맨 앞은 항상 코드가 실행 중인 컨텍스트의 변수 객체다. 스코프 체인의 맨 뒤에는 항상 전역 스코프(모두가 접근 가능한)가 있다.

이 예제에서, `console.log` 문에 다다르면, 우리는 다음의 스코프체인을 갖는다.

> 전역 스코프 -> `main()` 함수 스코프 -> `nested()` 함수 스코프

`nested` 함수 안에서 우리는  `main()`부터 전역까지 모든 변수에 접근이 가능하다. 하지만, 반대 방향으로는 안된다. `main`은 `nested` 안쪽의 변수를 볼 수 없다. 이건 매우 강력한 기능이다.

식별자(identifier)를 만날 때마다, 이것을  블록 체인 전체 안에서 자바스크립트 엔진이 찾는다. 이 과정을 변수 찾아보기(variable lookup)라 부른다. 이것을 위의 예제를 실전 삼아 살펴보자.

- `console.log(nestedVariable)`
  - 자바스크립트 엔진이 식별자 `nestedVariable`을 `nested()` 함수 스코프에서 먼저 찾는다.
- `console.log(mainVariable)`
  - `mainVariable` 이름을 `nested()` 스코프에서 찾는다. 아무것도 없다.
  - 체인에서 다음 스코프인 `main()` 스코프에서 찾는다. 여기 있다.

아마 당신은 여기서 이 블록 체인에 `mainVariable`이라는 이름을 갖는 또 다른 변수가 있다는 것을 알아챘을 것이다.

> !!! 변수 찾아보기는 첫번째 식별자를 찾으면 종료한다. 만약 전역변수와 이름이 같은 변수를 지역적으로 선언하면, 당신은 전역변수에 대한 접근을 잃을 것이다.

- `console.log(globalVariable)`
  - `nested()` 스코프 내에서 식별자를 찾지 못함
  - `main()` 스코프 내에서 식별자를 찾지 못함
  - 전역 스코프에서 식별자를 찾음

자바스크립트 엔진이 스코프 체인의 끝에 도달하고 맞는 이름을 찾지 못하면, `undefined`를 반환한다.

# 변수 호스팅

이 글의 첫번째 예제로 돌아가보자. (2)번이라 표시된 print에서 무슨 일이 일어나는가?

이 변수는 전역으로 선언되고 한 줄 아래 함수 안에서 다시 선언된다. 하지만, 결과는 `undefined`다.

짧게 답하면, 변수 호스팅 때문이다. 전에 이미 말했지만, `var`로 함수 안에 변수를 하나 선언하면, 이것은 지역 스코프에 붙고 더이상 같은 이름의 전역 변수에는 접근할 수 없다.

호스팅은 모든 선언을 현재 스코프 제일 위(현재 스크립트 또는 현재 함수의 제일 위)에 옮기는 자바스크립트의 기본 행동이다.  따라서 우리가 함수 안쪽 어느 곳에서 변수를 선언했는지는 상관없고, 이 변수는 실행 컨텍스트 시작부터 이 함수 스코프에 속한다.

하지만, 자바스크립트는 초기화가 아니라, 오로지 선언만 끌어올린다(hoisting). 그래서 명시적으로 이 변수에 값을 추가하지 않을 때까지, 이 값은 `undefined`가 된다.

이 예제에서, 우리는 함수 `f()`의 첫번째 줄에서 `myString`을 사용한다. `myString`이 끌어올려진걸 아니까, 이 변수는 `f()` 스코프에 속한다. 변수 찾아보기에서, 식별자를 거기서 찾는다. 명시적인 값이 거기에 더해지지 않았기 때문에, 이 값은 여전히 `undefined`다. 

# 결론

이게 다다. 우리는 이 여정의 첫번째 진짜 도전을 통과했다.

나는 많은 개발자들이 자바스크립트가 이상하고 예측불가라고 불평하는 것을 알고있다. 이건 그들이 자바스크립트가 다른 언어들(특히 자바)과 정확히 같은 방식으로 행동하리라고 생각하기 때문이다. 이들 사이에는 어떤 핵심적인 차이가 있고 우리가 이 글에서 이야기한 것은 그중 하나다. 당신이 이런 핵심 개념들을 이해하고 나면, 나머지는 누워서 떡먹기다.

이제 첫번째 예를 살펴보자. 이제는 여기나온 말들이 이해될 것이다.

```
1. "I'm outside"  // variable from global scope

2. undefined  // hoisted local variable from function scope 
              // the value is not added yet, so it's undefined

3. "I'm in function" // same variable as above, but this time a 
                     // value was explicitly added

4. "I'm in for" // same local variable, initialized again

5. "I'm in for" // since we don't have block level scope, the same 
                // local variable is printed

6. "I'm outside" // the variable from the global scope again (like 1)
```

Further reading:

- scotch.io: [자바스크립트 스코프 이해하기](https://scotch.io/tutorials/understanding-scope-in-javascript)
- ryanmorr.com: [자바스크립트 스코프와 컨텍스트 이해하기](http://ryanmorr.com/understanding-scope-and-context-in-javascript/)
- Javascriptissexy.com: [변수 스코프와 끌어올리기 설명하기](http://javascriptissexy.com/javascript-variable-scope-and-hoisting-explained/)