[TOC]

# 들어가면서

이 노트는 코드스테이츠의 "런코"에서 배운 내용을 정리한 것이다.  따라서 이 부분의 내용은 내가 만들어낸 것이 아니라, 코드스테이츠가 만든 내용을 내가 나름대로 요약한 것임을 밝혀둔다.

# Execution context

프로그램이 실행되면, 프로그램은 변수와 그 값을 저장하기위한 저장공간을 만들게된다. 이런 메모리상의 스코프 구조를 실행 컨텍스트(execution context)라고 한다.

여기서 용어의 혼동이 올 것 같다. 

- 스코프(scope): lexical scope(정적인 스코프)를 의미
- 메모리 상의 스코프(in-memory scope): 실행 컨텍스트

다음 예를 보자.

```javascript
var hero = aHero(); // aHero(): 랜덤한 스트링을 반환
var newSaga = function () {
    var foil = aFoil(); // aFoil(): 랜덤한 스트링을 반환
    var saga = function () {
    	var deed = aDeed(); // aDeed(): 랜덤한 스트링을 반환
    	log(hero + deed + foil);
    };
    saga();
    saga();
};
newSaga();
newSaga();
```

한줄한줄 따라가면서 인터프리터가 코드를 어떻게 처리하는지 살펴보자. 

시작하자마자 인터프리터가 글로벌 실행 컨텍스트를 만든다.

* 1번 줄
  * 인터프리터가 변수 `hero`를 선언하고, 여기에 함수에 의해 랜덤하게 얻어진 `'Gal'`이라는 문자열을 할당한다.
* 2번 줄
  * 인터프리터가 변수 `newSaga`를 선언하고, 여기에 2번줄 부터 10번줄에 이르는 함수 정의를, 건드리지 않고 그대로 이 변수에 할당한다.
* 11번 줄
  * `newSaga()` 함수를 실행하면서 새로운 실행 컨텍스트를 만든다. 그리고 이 함수 호출에서 지역변수에 해당하는 새로운 변수들을 위한 공간을 만든다.
  * 이제 이 컨텍스트가 인터프리터가 현재 처리해야하는 컨텍스트가 된다.
* 3번 줄
  * 현재 스코프에 `foil`이라는 변수를 선언하고, 거기에 함수에 의해 랜덤하게 얻어진 `'Cow'`라는 문자열을 할당한다.
* 4번 줄
  * 현재 스코프에 `saga`라는 변수를 선언하고, 거기에 2번줄에서와 마찬가지로, 함수 정의를 그대로 할당한다.
* 8번 줄
  * 인터프리터가 현재 컨텍스트에서 `saga`라는 이름이 어떤 의미인지 찾는다.
  * `saga`의 정의를 찾아내고, 새 실행 컨텍스트를 만든 뒤, 이 새 컨텍스트를 현재 처리해야하는 컨텍스트에 둔다.
* 5번 줄
  * 현재 스코프에 `deed`라는 변수를 만들고, 함수에 의해 랜덤하게 얻어진  `'Eyes'`라는 문자열을 할당한다.
* 6번 줄
  * `hero`를 지역 실행 컨텍스트에서 찾지만, 실패한다.
  * 그 위의 컨텍스트에서 `hero`를 찾지만 역시 실패한다.
  * 이제 그 위의 컨텍스트인 글로벌 실행 컨텍스트에서 `hero` 변수를 찾아 거기에 할당된 값을 찾는다.
  * `log()`에 파라미터로 들어갈 문자열을 만들기 시작한다. 
  * `deed`와 `foil`을 `hero`와 같은 방식으로 찾아서 문자열에 더한다.
  * `log`를 실행한다.  `"GalEyesCow"`가 출력된다.
* 7번 줄
  * 함수의 끝 줄이므로, 현재 처리해야하는 컨텍스트에 바로 바깥 컨텍스트를 위치시키고 전에 처리한 마지막 줄로 돌아간다.
* 9번 줄
  * 함수 `saga()` 를 다시 invoke한다. 
  * 하지만 이 전에 실행했던 실행 컨텍스트로 가는 것이 아니라 아예 새로운 컨텍스트를 만들고 같은 방법으로 진행한다.
* 5번 줄
  * 현재 스코프에 `deed`라는 변수를 만들고, 여기에 함수에 의해 랜덤하게 얻어진 `'Tips'`라는 문자열을 할당한다.
* 6번 줄
  * `hero`, `deed`, `foil`을 스코프 체인을 안쪽부터 순서대로 돌면서 할당된 값을 찾는다.
  * 결국 `"GalTipsCow"`가 출력된다.
* 7번 줄
  * 함수의 끝 줄이므로, 현재 처리해야하는 컨텍스트에 바로 바깥 컨텍스트를 위치시키고 전에 처리한 마지막 줄로 돌아간다. 
* 10번 줄
  * `newSaga` 함수의 끝이므로, 현재 처리해야하는 컨텍스트에 바로 바깥 컨텍스트인 글로벌 실행 컨텍스트를 위치시키고, 전에 처리한 마지막줄로 돌아간다.
* 12번 줄
  * `newSaga`를 현재 컨텍스트에서 찾아서 함수를 invoke하고, 이전에 `newSaga()`에 의해 만들어졌던 컨텍스트와는 다른 새로운 컨텍스트를 만들고 같은 작업을 반복한다.
* 마지막 줄
  * 프로그램의 실행이 끝난다.



# this

## Execution context 가 생성될 때 만들어지는 것들

- 어떤 함수가 호출되면, 실행 컨텍스트가 만들어진다.
  - 만들어진 컨텍스트는 call stack에 push 된다.
  - 함수를 벗어나면, call stack에서 pop 된다.
- 이 실행 컨텍스트는 스코프 별로 생성된다.
- 이 실행 컨텍스트에는 다음의 것들이 담긴다.
  - scope 내 변수 및 함수 (local, global)
  - 전달 인자 (`arguments`)
  - 실행 컨텍스틑를 호출한 것 (caller)
  - `this`



## `this` 를 binding하는 5가지 패턴

- global reference
- function invocation
- construction mode
- method invocation
- `.call()`  or `.apply()` invocation



### 패턴 1: global reference

- global object(전역 객체)인 `window` 객체에 `this` 바인딩

```javascript
var name = 'Global Variable';
console.log(this.name); // "Global Variable"

function foo() {
	console.log(this.name);
}

foo(); // "Global Variable"
```



### 패턴 2: function invocation 

- global object(전역 객체)인 `window` 객체에 `this` 바인딩

- 그러니까 함수 invocation이 일어나도 `this`는 여전히 `window`와 바인딩 되어있다는 말이다.

- 다른 언어들과는 좀 다르다.

```javascript
var name = 'Global Variable';
function outer() {
    function inner() {
        console.log(this.name); // "Global Variable"
    }
    
    inner();
}

outer();
```



###  패턴 3: method invocation

- 메서드: 객체 안에서 정의된 함수

- 이 메서드를 불러올 때, 메서드가 정의된 객체에 `this` 바인딩

```javascript
var counter = {
    val: 0,
    increment: function() {
        this.val += 1;
    }
};

counter.increment();
console.log(counter.val); // 1
counter['increment']();
console.log(counter.val); // 2

var obj = {
    fn: function(a, b) {
        return this;
    }
};
var obj2 = {
    method: obj.fn
};

console.log(obj2.method() === obj2); // true
console.log(obj.fn() === obj); // true
```



사실 패턴 2와 3은 같은 것을 의미한다.

글로벌 컨텍스트의 경우에는 `window.function()`에서 `window.` 이 생략된 것이라 볼 수 있기 때문이다.



### 패턴 4: construction mode

- new 연산자로 생성된 function 영역의 this

- 이 invocation으로 생성되는 객체에 `this` 바인딩

```javascript
function F(v) {
    this.val = v;
}

// create new instance of F
var f = new F('WooHoo!');

console.log(f.val); // WooHoo!
console.log(val); // ReferenceError
```



### 패턴 5: `.call` or `.apply` invocation

- 각 함수의 첫 argument인 객체에 `this` 바인딩

- 수동으로 `this` 바인딩을 서술하기 위해 사용 

```javascript
function identify() {
    return this.name.toUpperCase();
}
function speak() {
    var greeting = "Hello, I'm " + identify.call(this);
    console.log(greeting);
}
var me = { name: 'Kyle' };
var you = { name: 'Reader' };

identify.call(me); // "KYLE"
identify.call(you); // "READER"
speak.call(me); // "Hello, I'm Kyle"
speak.call(you); // "Hello, I'm Reader"
```

함수의 `call` 메서드는 this에 첫번째 argument를 넣고, 함수를 실행시킨다. 

`apply` 메서드 역시 비슷하다. 다만 argument를 배열로 넣어준다는 점만 다르다.

```javascript
var add = function (x, y) {
    this.val = x + y;
}
var obj = {
    val: 0
};

add.apply(obj, [2, 8]);
console.log(obj.val); // 10
add.call(obj, 2, 8);
console.log(obj.val); // 10
```

 