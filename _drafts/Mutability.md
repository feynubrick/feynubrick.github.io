## public and mutable

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



## private

* 생성자 안의 변수와 생성자의 arguments 는 private하다.

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

