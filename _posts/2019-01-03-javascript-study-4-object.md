---
layout: post
title: "자바스크립트 공부 // 4. 객체(Object)"
comments: true
author: feynubrick
date:   2019-01-03
tags: [JavaScript, Study]
---

## 객체 초기자(Object initializer)

처음에 객체를 배울 때, 다음과 같이 써서 객체를 만들어준다고 배웠다.

```javascript
var obj = {
    arr: [1, 2, 3],
    val: 123,
    o: {key: 'value'}
};
```

[MDN의 객체 초기자](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer)(object initializer) 페이지를 보면,  객체는 `new Object()`, `Object.create()` 나 위에 사용한 것 같은 방법을 의미하는 literal notation 또는 initializer notation 이라는 방법으로 초기화할 수 있다.

속성이름(key)에 계산된 이름을 넣고 싶은 경우에는 [이 내용](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#Computed_property_names)에 따르면, 다음과 같은 방법으로 할 수 있다.

```javascript
// Computed property names (ES2015)
var i = 0;
var a = {
  ['foo' + ++i]: i,
  ['foo' + ++i]: i,
  ['foo' + ++i]: i
};

console.log(a.foo1); // 1
console.log(a.foo2); // 2
console.log(a.foo3); // 3
```

## 함수 속성(property)

```javascript
var meglomaniac = {
    mastermind: "Brain",
    henchman: "Pinky",
    battleCry: function(noOfBrains) {
        return "They are " + this.henchman + " and the" +
            Array(noOfBrains + 1).join(" " + this.mastermind);
    }
};
```

`battleCry` 라는 키 이름을 가진 속성의 값은 함수다.
여기에는 함수의 메모리상의 위치가 들어가게 된다.
이렇게 객체의 속성에 함수를 할당하면, 이 함수를 메서드처럼 활용할 수 있다.

`this`는 나중에 설명하기 때문에 일단 여기서는 객체 자신을 가리키는 것으로 생각하면 된다.

이에 더해 하나 공부해 볼 부분은 다음 부분이다.

```javascript
Array(noOfBrains + 1).join(" " + this.mastermind);
```

`" " + this.mastermind` 가 여러번 반복되는 코드를 짜고 싶을 때, for문을 직접 써서 하는 방법이 있지만, 이렇게 Array의 `join()` 메서드를 사용하면, 코드 길이를 줄일 수 있다.

## `in` 키워드(keyword)

```javascript
var meglomaniac = {
    mastermind: "The Monarch",
    henchwoman: "Dr Girlfriend",
    theBomb: true
};

var hasBomb = "theBomb" in meglomaniac;
console.log(hasBomb); // true
```

`in` [연산자](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/in)를 사용하면 object에 해당 property가 있는지 없는지 확인할 수 있다. `ture` 또는 `false`를 반환한다. 이 연산자는 배열에도 사용할 수 있다.

즉, 다음과 같이 할 수 있다.

```javascript
var trees = ['redwood', 'bay', 'cedar', 'oak', 'maple'];
0 in trees        // returns true
3 in trees        // returns true
6 in trees        // returns false
```

## `this` 키워드

어떤 객체에 함수가 들어있을 때, `this` 키워드는 그 객체를 의미한다. 그러니까 다음 같이 말이다.

```javascript
var currentDate = new Date()
var currentYear = (currentDate.getFullYear());
var meglomaniac = {
    mastermind: "James Wood",
    henchman: "Adam West",
    birthYear: 1970,
    calculateAge: function() {
        return currentYear - this.birthYear;
    }
};

expect(currentYear).toBe(2018);
expect(meglomaniac.calculateAge()).toBe(48);
```

## 객체 프로토타입(Object prototype)

객체 생성자(object constructor)를 사용해서 만든 객체에 속성을 추가할 수 없다.
이 내용을 이해하려면 먼저 객체 생성자에 대해서 알아야 한다.

## [객체 생성자 (object constructor)](https://www.w3schools.com/js/js_object_constructors.asp)
다음과 같이 일종의 청사진을 만들어 놓고 객체를 여러개 만들어 채울 수 있다.

```javascript
function Person(first, last, age, eye) {
    this.firstName = first;
    this.lastName = last;
    this.age = age;
    this.eyeColor = eye;
}

var myFather = new Person("John", "Doe", 50, "blue");
var myMother = new Person("Sally", "Rally", 48, "green");
```

이렇게 하면 같은 구조를 갖는 서로다른 객체 `myFather`와 `myMother`가 생긴다.
여기서 `this` 키워드는 코드를 소유한 객체를 말한다.
그러니까 `this`의 값은 객체 안에서 사용되면, 그 객체 자신이다.

존재하는 객체에 새 속성이나 메서드를 추가하는 것은 계속 해 왔듯이 다음과 같이 해주면 된다.

```javascript
myFather.nationality = "English";
myFather.name = function () {
    return this.firstName + " " + this.lastName;
};
```

그러나 새 속성을 객체 생성자에 위와 같이 추가할 수는 없다.

새 속성을 생성자에 추가하려면 생성자 함수에 추가해야 한다.

```javascript
function Person(first, last, age, eyecolor) {
    this.firstName = first;
    this.lastName = last;
    this.age = age;
    this.eyeColor = eyecolor;
    this.nationality = "English"; // added property
    this.name = function() {
        return this.firstName + " " + this.lastName;
    }; // added method
    this.changeName = function (name) {
        this.lastName = name;
    };
}
```

### 객체 프로토타입([Object prototype](https://www.w3schools.com/js/js_object_prototypes.asp))
자바스크립트에서 생성되는 모든 객체는 prototype이라는 속성(`__proto__`)을 갖고 있는데, 여기에는 해당 객체의 prototype에 해당하는 객체의 참조 주소가 있다. 자바스크립트에서는 모든 것이 객체라고 볼 수 있다. 모든 객체의 prototype 참조를 따라가면 결국 Object.prototype 라는 객체에 도달하기 때문이다. 

그런데 이 프로토타입은 왜 존재하는 것일까? 예를 들어 배열을 하나 선언하면, 메모리 공간에 배열 객체 하나가 생기고, 이 배열은 Array 객체의 메서드를 모두 사용할 수 있다. 만약 이 모든 것이, 프로토타입이라는 방식으로 '참조'되지 않고 그대로 복사되는 형식으로 되어 있었다면, 객체를 하나 만들 때마다 모든 내용을 함께 복사해야하기 때문에 많은 자원이 소모된다.

### 객체 생성자에 새 속성이나 메서드 추가

위에서 객체 생성자에 새 속성이나 메서드를 추가할 수는 없다고 했지만, `prototype` 키워드를 사용하면 가능하다.

```javascript
function Person(first, last, age, eyecolor) {
    this.firstName = first;
    this.lastName = last;
    this.age = age;
    this.eyeColor = eyecolor;
}
Person.prototype.nationality = "English";
Person.prototype.name = function() {
    return this.firstName + " " + this.lastName;
};
```

위에서 말한 두가지를 종합하면, prototype을 통해 추가한 속성 또는 메서드는 이 생성자를 통해 만들어진 모든 객체에 추가된다. 따라서 다음과 같은 상황이 가능하다.

```javascript
function Circle(radius) {
    this.radius = radius;
}

var simpleCircle = new Circle(10);
var colouredCircle = new Circle(5);
colouredCircle.colour = "red";

expect(simpleCircle.colour).toBe(undefined);
expect(colouredCircle.colour).toBe("red");

Circle.prototype.describe = function() {
    return "This circle has a radius of: " + this.radius;
};
```

## 객체 속성의 성격: Mutability

객체의 속성(property)이 외부에서 어떤 경우에 접근 가능한지 알아보자.
C++ 같은 객체지향 언어에서는 private, public 같은 것으로 변수의 외부로부터의 접근을 각각 막거나, 허용할 수 있다.
자바스크립트에서도 이와 같은 개념이 존재하는데, 이를 적절히 활용하면 객체지향언어와 같이 객체를 다룰 수 있다.

- public: 외부의 접근을 허용한다. 즉, 외부에서 맘대로 값을 바꿀 수 있다.
- private: 외부에서 접근 불가하다. 즉, 외부에서 참조자체가 안되기 때문에 값을 바꿀 수 없다.

### public

- 객체의 속성은 public이고 mutable하다.

```javascript
var aPerson = {firstname: 'John', lastname: 'Smith'};
aPerson.firstname = 'Alan';
console.log(aPerson.firstname); // "Alan"
```

- 객체의 생성자를 사용해 생성된 객체의 속성 또한 public 이고 mutable하다.

```javascript
function Person(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
}
var aPerson = new Person('John', 'Smith');
aPerson.firstname = 'Alan';
console.log(aPerson.firstname); // "Alan"
```

- prototype 속성은 public 이고, mutable 하다.

```javascript
function Person(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
}

Person.prototype.getFullName = function () {
	return this.firstname + ' ' + this.lastname;
};

var aPerson = new Person('John', 'Smith');
console.log(aPerson.getFullName()); // "John Smith"

aPerson.getFullName = function () {
    return this.lastname + ', ' + this.firstname;
};
console.log(aPerson.getFullName()); // "Smith, John"
```

### private

- 생성자 안의 변수와 생성자의 arguments 는 private하다.

```javascript
function Person (firstname, lastname) {
    var fullName = firstname + ' ' + lastname;
    
    this.getFirstName = function () { return firstname; };
    this.getLastName = function () { return lastname; };
    this.getFullName = function () { return fullName; };
}

var aPerson = new Person ('John', 'Smith');

aPerson.firstname = 'Penny'; // 1st argument
aPerson.lastname = 'Andrews'; // 2nd argument
aPerson.fullName = 'Penny Andrews'; // variable in the constructor

console.log(aPerson.getFirstName()); // "John"
console.log(aPerson.getLastName()); // "Smith"
console.log(aPerson.getFullName()); // "John Smith"
```