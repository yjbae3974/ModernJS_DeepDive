## Object 생성자 함수
```js
const person = new Object();
person.name = 'Lee';
person.sayHello = function() {
    console.log(`Hi! My name is ${this.name}`);
};
person.sayHello(); // Hi! My name is Lee
```
### 생성자 함수
-  `new` 키워드와 함께 호출하는 함수
```js
function Person(name) {
    this.name = name;
    this.sayHello = function() {
        console.log(`Hi! My name is ${this.name}`);
    };
}
const me = new Person('Lee');
const you = new Person('Kim');
me.sayHello(); // Hi! My name is Lee
you.sayHello(); // Hi! My name is Kim
```

### this 바인딩
- 생성자 함수 내부에서 `this`는 생성자 함수가 생성할 인스턴스를 가리킨다.
```js
function Person(name) {
    console.log(this); // Person {}
    this.name = name;
    this.sayHello = function() {
        console.log(`Hi! My name is ${this.name}`);
    };
}
```

```js
function foo() {}

// [[Call]]: 일반적인 함수 호출
foo();

// [[Construct]]: 생성자 함수 호출
new foo();
```

```js
// 생성자 함수로서 정의하지 않은 일반 함수
function add(x, y) {
    return x + y;
}

let inst = new add(); // TypeError: add is not a constructor

console.log(inst); // {}

function createUser(name, role) {
    return { name, role };
}

inst = new createUser('Lee', 'admin'); // TypeError: createUser is not a 

// 함수가 생성한 객체를 반환
console.log(inst); // {name: 'Lee', role: 'admin'}
```