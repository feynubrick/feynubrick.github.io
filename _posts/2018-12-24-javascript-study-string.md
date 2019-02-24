---
layout: post
title: "자바스크립트 공부 // 문자열(String)"
comments: true
author: feynubrick
date:   2018-12-24T12:00:00Z
tags: [JavaScript, Study]
---

# "자바스크립트 공부" 시리즈를 시작하면서
무언가를 배울 때 그 내용을 정리하는 것은 단순하지만 매우 효과적인 학습방법이다.
배운 것을 적어내려가는 동안 무의식적으로 내용이 정리가 되어 기억하는 데 도움을 준다.
나중에 그 정보를 잊은 경우에는, 나의 뇌 바깥의 어떤 곳에 적혀있는 정보를 보고 다시 기억을 떠올리거나 새로운 기억을 만들어 낼 수 있다.
결국 한번 배운 내용을 나중에 써먹을 때 아주 유용하다는 얘기다.
물론 적어놓은 내용을 '잃어'버린다면, 그 내용을 '잊은' 것과 다름없게 된다.
그래서 잃어버리기 힘든 이 공간에 배운 내용을 올리려고 한다.

어떻게 정리해 올릴 것인가?
기억해야할 내용을 올려두긴 했어도 다시 기억을 되살리려 할 때마다 새로 배우는 것처럼 시간을 많이 써야한다면, 적지 않으니만 못한 것이 된다.
따라서 최대한 간결하게 필요한 내용만 요약해서 올릴 것이다.
너무 간결하면 대체 이게 무슨 말인지 모를 수도 있으니, 아슬아슬한 줄타기를 잘 해서 "sweet spot"을 찾아내야 하는 어려운 과정이 필요할 것이다.

일단 나의 언어로 내가 궁금해할만한 것들 위주로 정리하는 과정이 필요할 것이다.
그렇게 하기 위해서는 주제에 대해 나 나름대로 이 내용을 최소한 한번은 '이해'해 볼 필요가 있다.
'이해'에 표시를 한 이유는 이해한다는 것이 우리가 스치듯 생각하는 것 보다는 어렵고 복잡한 과정이기 때문이다.
어떤 것을 이해했다고 하는 것이 무엇인지 철학적으로 진지하게 파고들 생각은 없다.
그럴 능력이 없기 때문이다.
다만 어디선가 들었던 어느 물리학자(아마 파인만이었던 것 같은데, 파인만도 누군가를 인용했던 것으로 기억하지만 그 사람의 이름을 기억하지는 못하고 있다)의 격언을 따라 이해에 도달해보려한다.

> "만약 당신이 무언가를 이해했다면, 그 내용을 모르는 보통 사람에게 평이한 말로 설명할 수 있어야 한다."

나의 공부가 이 경지에는 이르지 못했지만 그렇다고 가만히 있으면, 영원히 그 경지의 근처 혹은 그 방향으로라도 설 수 없게 될 것이다.
그러니 지금은 이 격언을 저 먼 곳의 별빛처럼 걸어두고 공부를 해 나가도록 하겠다.
일단 적어두고 고치면 되는 것 아닌가.

## 정리한 내용의 출처
이 시리즈에 정리된 내용은 코드스테이츠와 MDN(Mozilla Developer Network Web Docs)에서 가져온 것을 나름대로 소화해본 것이다.
나의 '소화기관'이 좋지 않기 때문에 대부분은 원형 그대로다.
공부를 더 진행하면서 차차 더 정리를 하도록 할 것이다.
시작이 중요한 것이니 말이다.

코드스테이츠 프리코스 강의에서는 전체적인 얼개와 다뤄야 할 내용들의 목록과 그 내용의 일부를, MDN에서는 그 세부적인 내용을 가져오고 링크를 달아놨다.

# 문자열

자바스크립트 변수의 한 종류다. 자바스크립트 변수는 다음의 두가지 유형(타입)으로 구분할 수 있다.
- 원시타입(primitive type): `null`, `undefined`, number, string, boolean
- 참조타입(reference type): 객체(object)

이 두 유형은 이 변수의 식별자(identifier)가 가리키는 메모리 주소에 담긴 정보가 무엇인지에 따라 구분된다.
원시타입의 경우에는 어떤 값(숫자, 글자 같은)이, 참조타입의 경우에는 메모리 주소가 담기게 된다.

문자열은 원시타입에 속하고, 그 담기는 내용은 문자인 변수다.
여기서는 문자열의 기본 사용 방법과 사용할 수 있는 메서드들을 간단한 예시와 함께 정리했다.

여기 적힌 내용들은 다음의 두가지를 보고 나름 소화를 한 다음 적어 놓은 것이다.
- 코드스테이츠의 프리코스 강의
- MDN(Mozilla Developer Network Web Docs)

이중 코드스테이츠의 프리코스 강의에서는 다루는 내용의 얼개와 일부분의 내용을 가져왔고, 각 얼개의 세세한 부분은 MDN에서 발췌하고 링크를 달았다.
가져온 모든 부분에 출처를 따로따로 표시하고 싶지만 그렇게 적기엔 너무 힘들기도하고, 이 글은 학술논문이 아니니 거기까지 하지 못하는(않는) 것을 양해해주시기를 부탁한다.

## 기본 용법

여기서는 문자열을 어떻게 사용하는 것인지에 대해 공부한다.
그러니까 어떤 특성을 갖는지에 대해 공부한다.

### 문자 하나에 접근, 그러나 읽기 전용

문자 하나에 `[ ]`와 index를 사용해 배열에서 항목을 접근하듯 할 수 있다. 그러나 문자열에서는, 각 항목이 읽기 전용이라 값을 바꿀 수는 없다.

```javascript
var str = 'Star Wars';
console.log(str[0]); // "S"
str[0] = 'Z'; // str의 0번째 인덱스에 "Z" 대입 시도
console.log(str); // "Star Wars", str은 읽기 전용이라 윗줄의 시도가 헛되이 끝났다.
```

### 문자열 이어붙이기

문자열을 이어붙이려면, `+` 연산자나 `concat()` 메서드를 사용한다.

```javascript
var str1 = 'The ';
var str2 = 'Godfather ';
var str3 = '2';
console.log(str1 + str2); // "The Godfather "
console.log(str3 + 7); // "27" 숫자인 7을 문자열로 자동 변환한 뒤 이어붙인다.
var str4 = str1.concat(str2, str3);
console.log(str4); // 'The Godfather 2'
```

### 길이 속성

문자열의 길이를 알고 싶을 때 사용한다. 문자열에 포함된 모든 문자(공백문자 포함)의 갯수와도 같다.

```javascript
var title = "The Hitchhiker's Guide to the Galaxy";
console.log(title.length); // 36
```

## 메서드(methods)

메서드는 객체지향 언어인 C++을 공부할 때 배웠었다.
메서드는 어떤 객체(C++에서는 class에 의해 만들어진)의 속성(property)를 조작해 어떤 결과를 만들어내는 객체에 딸린 함수라고 이해하고 있다.

내가 배웠던 C나 C++과는 다르게 자바스크립트의 모든 변수는 객체라고 볼 수 있다(어디선가 들은 말이다. 모든 변수가 객체라는 주장이 참인지 아닌지에 판별하려면, 근거를 찾는 등 추가로 공부를 해야한다.).
따라서 문자열 변수도 역시 객체여서, 여기에 딸린 여러 메서드를 적용할 수 있다.
여기서는 문자열을 조작할 수 있는 메서드들 중 자주쓰이고 유용한 메서드에 대해 공부해본다.

### [indexOf()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf)

- 문자열에서 어떤 문자열(`searchValue`)의 시작 index를 찾을 때 사용한다.
- 여러 개가 일치할 경우 처음 것의 index를 반환한다.
- 찾지 못하면, `-1`을 반환한다.

```javascript
var message = 'Blue Whale Blue Whale';
console.log(message.indexOf('Whale')); // 5
var fromIndex = 10;
console.log(message.indexOf('Whale', fromIndex)); // 16
console.log(message.indexOf('Red')); // -1
console.log(message.indexOf()); // -1
```

### [split()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split)

- String을 문자열 안의 어떤 글자를 기준으로 쪼개서 배열로 만든다.
- 두번째 파라미터에 숫자를 넣어서 결과로 나오는 배열의 길이를 조절할 수 있다.

```javascript
'a b c'.split(' '); // ["a", "b", "c"]
'a b c'.split(); // ["a b c"] // 배열
'a,b,c,d,e,f,g'.split(',', 4); // ["a", "b", "c", "d"]
```

### [substring()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substring)

- 문자열의 일정 부분을 문자열로 꺼낼 때 사용한다. 범위는 index로 지정한다.
- 끝 인덱스를 지정할 경우 지정한 index 바로 ***직전***까지의 문자를 의미한다는 것에 주의한다. 
    - 끝 인덱스를 반환될 하위 문자열에서 제외할 첫번째 문자의 인덱스라고 기억하면 된다.

```javascript
'Mozilla'.substring(1, 3); // "oz"
'Mozilla'.substring(2); // "zilla"
```

### [toLowerCase()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase) 

- 문자열을 소문자로 변환할 때 사용.

```javascript
'AbCd'.toLowerCase(); // "abcd"
```

### [toUpperCase()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase)
- 문자열을 대문자로 변환할 때 사용.

```javascript
'AbCd'.toUpperCase(); // "ABCD"
```


### [repeat()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/repeat)

- 말 그대로 문자열을 반복해서 덧붙인다.

```javascript
var chorus = 'Because I\'m happy. ';

console.log('Chorus lyrics for "Happy": ' + chorus.repeat(27));

// expected output: "Chorus lyrics for "Happy": Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. "
```

### [trim](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/Trim)

- 문자열의 양쪽 끝의 공백을 제거할 때 사용한다.
- "공백"에 포함되는 문자
    - whitespace: space, tab, no-break space, ...
    - line terminator: LF, CR, ...

```javascript
'   Hello World!   '.trim(); // "Hello World!"
```

### [match](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/match)

- 문자열에서 정규식([regular expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions))과 일치하는 문자열을 찾아 배열로 반환한다.

```javascript
'AbCdEfGhIjKlMnOpQrSvWxYz'.match(/[A-Z]/g);
// ["A", "C", "E", "G", "I", "K", "M", "O", "Q", "S", "W", "Y"]
```

### [replace](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace)

- 문자열 내의 패턴과 일치하는 모든 문자열을 찾아 새 내용으로 바꾼다.

```javascript
var p = 'The quick brown fox jumped over the lazy dog. If the dog reacted, was it really lazy?';
var regex = /dog/gi; // 정규표현식
console.log(p.replace(regex, 'ferret')); // "The quick brown fox jumped over the lazy ferret. If the ferret reacted, was it really lazy?"
```
