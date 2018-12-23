[TOC]

# 문자열

## 기본 용법

### 문자 하나에 접근

문자 하나에 []와 index를 사용해 배열에서 항목을 접근하듯 할 수 있다. 그러나 문자열에서는, 각 항목이 읽기 전용이라 값을 바꿀 수는 없다.

```javascript
var str = 'CodeStates';
str[0] = 'G';
console.log(str); // 'CodeStates' not 'GodeStates'
```

### 문자열 이어붙이기

문자열을 이어붙이려면, `+` 연산자나 `concat()` 메서드를 사용한다.

```javascript
var str1 = 'Code';
var str2 = 'States';
var str3 = '1';
console.log(str1 + str2); // 'CodeStates'
console.log(str3 + 7); // '17' 문자열로 자동 변환한 뒤 이어붙인다.
var str4 = str1.concat(str2, str3);
console.log(str4); // 'CodeStates1'
```

### 길이 속성

문자열의 길이를 알고 싶을 때 사용한다. 문자열에 포함된 모든 문자(공백문자 포함)의 갯수와도 같다.

```javascript
var str = 'CodeStates';
console.log(str.length); // 10
```

## 메서드

### [indexOf()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf)

문자열에서 어떤 문자열(`searchValue`)의 시작 index를 찾을 때 사용한다.

여러 개가 일치할 경우 처음 것의 index를 반환한다.

```javascript
var message = 'Blue Whale Blue Whale';
console.log(message.indexOf('Whale')); // 5
var fromIndex = 10;
console.log(message.indexOf('Whale', fromIndex)); // 16
console.log(message.indexOf('Red')); // -1
console.log(message.indexOf()); // -1
```

### [split()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split)

String을 어떤 글자를 기준으로 쪼개, 배열로 만드는 메서드

```javascript
'a b c'.split(' '); // ["a", "b", "c"]
'a b c'.split(); // ["a b c"] // 배열
'a,b,c,d,e,f,g'.split(',', 4); // ["a", "b", "c", "d"]
```

### [substring()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substring)

문자열의 일정 부분을 문자열로 꺼낼 때 사용한다. 범위는 index로 지정한다.

끝 인덱스를 지정할 경우 지정한 index 바로 ***직전***까지의 문자를 의미한다는 것에 주의한다. 

- 끝 인덱스를 반환될 하위 문자열에서 제외할 첫번째 문자의 인덱스라고 기억하면 된다.

```javascript
'Mozilla'.substring(1, 3); // "oz"
'Mozilla'.substring(2); // "zilla"
```

### [toLowerCase()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase) 

문자열을 소문자로 변환할 때 사용.

```javascript
'AbCd'.toLowerCase(); // "abcd"
```

### [toUpperCase()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase)

문자열을 대문자로 변환할 때 사용.

```javascript
'AbCd'.toUpperCase(); // "ABCD"
```



### [repeat()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/repeat)

문자열을 반복

```javascript

```



## 공부할거리

### [trim](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/Trim)

문자열의 양쪽 끝의 공백을 제거할 때 사용한다.

"공백"에 포함되는 문자

- whitespace: space, tab, no-break space, ...
- line terminator: LF, CR, ...

```javascript
'   Hello World!   '.trim(); // "Hello World!"
```

### [match](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/match)

문자열에서 정규식과 일치하는 문자열을 찾아 배열로 반환한다.

```javascript
'AbCdEfGhIjKlMnOpQrSvWxYz'.match(/[A-Z]/g);
// ["A", "C", "E", "G", "I", "K", "M", "O", "Q", "S", "W", "Y"]
```

### [replace](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace)

문자열 내의 패턴과 일치하는 모든 문자열을 찾아 새 내용으로 바꾼다.

```javascript
var p = 'The quick brown fox jumped over the lazy dog. If the dog reacted, was it really lazy?';

var regex = /dog/gi;

console.log(p.replace(regex, 'ferret')); // "The quick brown fox jumped over the lazy ferret. If the ferret reacted, was it really lazy?"
```

### [Regular Expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)
