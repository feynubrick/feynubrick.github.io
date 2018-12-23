[TOC]

## About [Underscore.js](https://underscorejs.org/#)

### functional programming

functional 프로그래밍은 일반적으로 코드를 짜던 방식, 즉 절차를 직접 쓰는 방식으로 코드를 짜는 것이 아니라, 이미 구현되어 있는 함수를 이용해서 코드를 짜는 방식을 말한다.

### Underscore.js

Underscore.js는 functional 프로그래밍을 하기위한 도구들을 자바스크립트에 내장된 객체를 확장하지 않고 사용할 수 있게 해준다.

여기서 드는 의문 이 모든게 각 type의 메서드와 같은 역할을 하는데, 굳이 Underscore.js를 쓰는 이유가 있는가?



## [filter()](https://underscorejs.org/#filter)

Array의 filter()와 같은 역할이다.

```javascript
var numbers = [1, 2, 3];
var odd = _(numbers).filter(function (x) { return x % 2 !== 0 });

console.log(odd); // [1, 3]
console.log(numbers); // [1, 2, 3]
```

## [map()](https://underscorejs.org/#map)

Array의 map()과 같은 역할이다.

```javascript

```



## [reduce()](https://underscorejs.org/#reduce)

Array의 reduce()와 같은 역할을 한다.

```javascript
var numbers = [1, 2, 3];
var reduction = _(numbers).reduce( function(memo, x) {
    return memo + x;
}, 0);

console.log(reduction); // 6
console.log(numbers); // [1, 2, 3]
```



## [forEach()](https://underscorejs.org/#each) === each()

## [all()](https://underscorejs.org/#every) === every()

## [any()](https://underscorejs.org/#some) === some()

## [range()](https://underscorejs.org/#range)

## [flatten()](https://underscorejs.org/#flatten)

## [chain()](https://underscorejs.org/#chain)...value()