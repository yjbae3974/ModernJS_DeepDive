## Object 생성자 함수

```js
const person = new Object();
person.name = "Lee";
person.sayHello = function () {
    console.log(`Hi! My name is ${this.name}`);
};
person.sayHello(); // Hi! My name is Lee
```

### 생성자 함수

-   `new` 키워드와 함께 호출하는 함수

```js
function Person(name) {
    this.name = name;
    this.sayHello = function () {
        console.log(`Hi! My name is ${this.name}`);
    };
}
const me = new Person("Lee");
const you = new Person("Kim");
me.sayHello(); // Hi! My name is Lee
you.sayHello(); // Hi! My name is Kim
```

### this 바인딩

-   생성자 함수 내부에서 `this`는 생성자 함수가 생성할 인스턴스를 가리킨다.

```js
function Person(name) {
    console.log(this); // Person {}
    this.name = name;
    this.sayHello = function () {
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

inst = new createUser("Lee", "admin"); // TypeError: createUser is not a

// 함수가 생성한 객체를 반환
console.log(inst); // {name: 'Lee', role: 'admin'}
```

## this 바인딩의 모든 경우

### 1. 일반 함수 호출

-   `this`는 기본적으로 전역 객체를 가리킨다 (브라우저: window, Node.js: global)

```js
console.log(this); // window (브라우저) 또는 global (Node.js)
function foo() {
    console.log(this); // window (브라우저) 또는 global (Node.js)
}

foo();
```

### 2. 메서드 호출

-   `this`는 메서드를 호출한 객체를 가리킨다

```js
const obj = {
    name: "Lee",
    sayHello() {
        console.log(this); // obj 객체
        console.log(`Hi! My name is ${this.name}`);
    },
};

obj.sayHello(); // Hi! My name is Lee
```

### 3. 생성자 함수 호출

-   `this`는 생성자 함수가 생성할 인스턴스를 가리킨다

```js
function Person(name) {
    console.log(this); // Person {}
    this.name = name;
}

const me = new Person("Lee");
```

### 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출

-   `this`는 첫 번째 인수로 전달한 객체를 가리킨다

```js
function getThisBinding() {
    return this;
}

const thisArg = { a: 1 };

console.log(getThisBinding()); // window
console.log(getThisBinding.apply(thisArg)); // { a: 1 }
console.log(getThisBinding.call(thisArg)); // { a: 1 }
console.log(getThisBinding.bind(thisArg)()); // { a: 1 }
```

### 5. 화살표 함수

-   `this`는 상위 스코프의 `this`를 가리킨다 (lexical this)

```js
const obj = {
    name: "Lee",
    regularFunction() {
        console.log(this); // obj
        const arrowFunction = () => {
            console.log(this); // obj (상위 스코프의 this)
        };
        function notArrowFunction() {
            //놀랍게도, 메소드 내부의 함수내부에서 this는 전역객체를 가리킨다.
            //이는 의도한 동작이 아닐 수 있기에, 화살표 함수를 사용하는 것이 좋다.
            console.log(this); // window (또는 undefined in strict mode)
        }
        arrowFunction();
    },
};

obj.regularFunction();
```

### 6. 이벤트 핸들러

-   `this`는 이벤트를 발생시킨 DOM 요소를 가리킨다

```js
const button = document.querySelector("button");
button.addEventListener("click", function () {
    console.log(this); // button 요소
});
```

### 7. 클래스 내부

-   `this`는 클래스의 인스턴스를 가리킨다

```js
class Person {
    constructor(name) {
        this.name = name;
    }

    sayHello() {
        console.log(this); // Person 인스턴스
        console.log(`Hi! My name is ${this.name}`);
    }
}

const me = new Person("Lee");
me.sayHello();
```

### 8. strict mode에서의 일반 함수 호출

-   `this`는 `undefined`를 가리킨다

```js
"use strict";

function foo() {
    console.log(this); // undefined
}

foo();
```

### 9. 콜백 함수

-   콜백 함수의 `this`는 호출 방식에 따라 달라진다

```js
const obj = {
    name: "Lee",
    callback: function () {
        console.log(this); // 호출 방식에 따라 달라짐
    },
};

// 메서드로 호출
obj.callback(); // obj

// 일반 함수로 호출
const fn = obj.callback;
fn(); // window (또는 undefined in strict mode)

// setTimeout의 콜백
setTimeout(obj.callback, 100); // window (또는 undefined in strict mode)
```

### 10. 프로토타입 메서드

-   `this`는 메서드를 호출한 인스턴스를 가리킨다

```js
function Person(name) {
    this.name = name;
}

Person.prototype.sayHello = function () {
    console.log(this); // Person 인스턴스
    console.log(`Hi! My name is ${this.name}`);
};

const me = new Person("Lee");
me.sayHello(); // Hi! My name is Lee
```

## constructor vs non-constructor

-   constructor: `new` 키워드와 함께 호출할 수 있는 함수
    -   내부 메소드로 [[Construct]]이 호출된다.
-   non-constructor: `new` 키워드와 함께 호출할 수 없는 함수
    -   내부 메소드로 [[Call]]이 호출된다.

### constructor 예시

```js
// 1. 함수 선언문
function Person(name) {
    this.name = name;
}
const person1 = new Person("Lee"); // ✅ 정상 동작

// 2. 함수 표현식
const Animal = function (type) {
    this.type = type;
};
const animal1 = new Animal("dog"); // ✅ 정상 동작

// 3. 클래스
class Car {
    constructor(brand) {
        this.brand = brand;
    }
}
const car1 = new Car("BMW"); // ✅ 정상 동작

// 4. 빌트인 생성자 함수
const str = new String("hello"); // ✅ 정상 동작
const num = new Number(42); // ✅ 정상 동작
const arr = new Array(1, 2, 3); // ✅ 정상 동작
const obj = new Object({ a: 1 }); // ✅ 정상 동작
```

### non-constructor 예시

```js
// 1. 화살표 함수
const ArrowFunc = () => {
    this.name = "test";
};
// const arrow1 = new ArrowFunc(); // ❌ TypeError: ArrowFunc is not a constructor

// 2. 메서드 (ES6 축약 메서드)
const obj = {
    method() {
        this.name = "test";
    },
};
// const method1 = new obj.method(); // ❌ TypeError: obj.method is not a constructor

// 3. Generator 함수
function* generator() {
    yield 1;
}
// const gen1 = new generator(); // ❌ TypeError: generator is not a constructor

// 4. async 함수
async function asyncFunc() {
    return "async";
}
// const async1 = new asyncFunc(); // ❌ TypeError: asyncFunc is not a constructor

// 5. 빌트인 non-constructor 함수들
// const math1 = new Math(); // ❌ TypeError: Math is not a constructor
// const json1 = new JSON(); // ❌ TypeError: JSON is not a constructor
// const console1 = new console(); // ❌ TypeError: console is not a constructor

// 6. 메서드가 아닌 함수를 객체에서 참조
const obj2 = {
    name: "test",
    getName: function () {
        return this.name;
    },
};
// const getName1 = new obj2.getName(); // ❌ TypeError: obj2.getName is not a constructor
```

### constructor 확인 방법

```js
// constructor인지 확인하는 방법들

// 1. instanceof Function으로 확인
console.log(Person instanceof Function); // true
console.log(ArrowFunc instanceof Function); // true (함수이지만 constructor는 아님)

// 2. new.target으로 확인 (함수 내부에서)
function TestFunc() {
    if (new.target) {
        console.log("constructor로 호출됨");
    } else {
        console.log("일반 함수로 호출됨");
    }
}

new TestFunc(); // "constructor로 호출됨"
TestFunc(); // "일반 함수로 호출됨"

// 3. 함수의 prototype 프로퍼티 확인
console.log(Person.prototype); // Person {}
console.log(ArrowFunc.prototype); // undefined (화살표 함수는 prototype이 없음)

// 4. 직접 호출해보기 (try-catch 사용)
function isConstructor(func) {
    try {
        new func();
        return true;
    } catch (e) {
        return false;
    }
}

console.log(isConstructor(Person)); // true
console.log(isConstructor(ArrowFunc)); // false
console.log(isConstructor(Math)); // false
```

### new 연산자

-   함수를 생성자로써 동작하게 해준다.
-   만약 함수 내부에 `this`가 있는데 생성자로 호출하지 않으면, `this`는 전역 객체를 가리키게 된다.

```js
function Person(name) {
    this.name = name;
}
const me = Person("Lee"); // new 없이 호출
console.log(name); // Lee (전역 객체의 프로퍼티가 됨)
console.log(me); // undefined (함수가 반환한 값이 없으므로)
```

-   이를 막기 위해 `new.target`이 도입되었다.

### new.target

-   생성자 함수가 `new` 키워드와 함께 호출되었는지 확인할 수 있다.(ES6)

```js
function Person(name) {
    if (!new.target) {
        return new Person(name);
    }
    this.name = name;
}
const me = Person("Lee"); // new 없이 호출
console.log(me.name); // Lee
const you = new Person("Kim"); // new 키워드와 함께 호출
console.log(you.name); // Kim
```

-   대부분의 빌트인 함수는 new 없이도 호출할 수 있다.

```js
const str1 = String(123); // new 없이 호출
console.log(str1); // "123"
const str2 = new String(123); // new 키워드와 함께 호출
console.log(str2); // [String: '123']
```

## 📋 요약

### 주요 내용

**1. Object 생성자 함수**

-   `new Object()`로 객체 생성
-   생성된 객체에 프로퍼티와 메서드 추가 가능

**2. 생성자 함수**

-   `new` 키워드와 함께 호출하는 함수
-   인스턴스를 생성하는 템플릿 역할
-   `this`는 생성할 인스턴스를 가리킴

**3. this 바인딩의 10가지 경우**

1. **일반 함수 호출**: 전역 객체 (window/global)
2. **메서드 호출**: 메서드를 호출한 객체
3. **생성자 함수 호출**: 생성할 인스턴스
4. **apply/call/bind**: 첫 번째 인수로 전달한 객체
5. **화살표 함수**: 상위 스코프의 this (lexical this)
6. **이벤트 핸들러**: 이벤트를 발생시킨 DOM 요소
7. **클래스 내부**: 클래스의 인스턴스
8. **strict mode**: undefined
9. **콜백 함수**: 호출 방식에 따라 달라짐
10. **프로토타입 메서드**: 메서드를 호출한 인스턴스

**4. Constructor vs Non-constructor**

-   **Constructor**: `new` 키워드로 호출 가능, `[[Construct]]` 내부 메서드
-   **Non-constructor**: `new` 키워드로 호출 불가, `[[Call]]` 내부 메서드

**5. new 연산자와 new.target**

-   `new` 없이 생성자 함수 호출 시 `this`는 전역 객체를 가리킴
-   `new.target`으로 `new` 키워드 사용 여부 확인 가능
-   빌트인 함수는 `new` 없이도 호출 가능

## 🎯 면접 질문

### 기본 개념 질문

**Q1. JavaScript에서 `this`가 가리키는 값은 언제 결정되나요?**

-   A: `this`는 함수가 **호출될 때** 결정됩니다. 함수가 정의된 위치가 아닌 호출 방식에 따라 달라집니다.

**Q2. 생성자 함수와 일반 함수의 차이점을 설명해주세요.**

-   A: 생성자 함수는 `new` 키워드와 함께 호출하여 인스턴스를 생성하는 함수입니다. 내부적으로 `[[Construct]]` 메서드를 사용하며, `this`는 새로 생성될 인스턴스를 가리킵니다.

### this 바인딩 심화 질문

**Q3. 다음 코드의 실행 결과를 예측해주세요.**

```js
const obj = {
    name: "Lee",
    regularFunction() {
        console.log(this);
        function innerFunction() {
            console.log(this);
        }
        innerFunction();
    },
};
obj.regularFunction();
```

-   A: 첫 번째 `console.log(this)`는 `obj` 객체를 출력하고, 두 번째는 전역 객체(window)를 출력합니다. 메서드 내부의 일반 함수는 `this`가 전역 객체를 가리키기 때문입니다.

**Q4. 화살표 함수의 `this` 바인딩이 일반 함수와 다른 이유는 무엇인가요?**

-   A: 화살표 함수는 `this`를 바인딩하지 않고, 상위 스코프의 `this`를 그대로 사용합니다(lexical this). 이는 함수가 정의된 시점의 `this`를 참조하므로 예측 가능한 동작을 보장합니다.

### 실무 적용 질문

**Q5. `new` 키워드 없이 생성자 함수를 호출했을 때 발생하는 문제와 해결 방법을 설명해주세요.**

-   A: `this`가 전역 객체를 가리키게 되어 의도하지 않은 전역 변수가 생성됩니다. 해결 방법으로는 `new.target`을 사용하여 `new` 키워드 사용 여부를 확인하고, 필요시 자동으로 `new`를 붙여 재귀 호출하는 방법이 있습니다.

**Q6. `call`, `apply`, `bind` 메서드의 차이점과 사용 사례를 설명해주세요.**

-   A:
    -   `call`: 즉시 호출, 인수를 개별적으로 전달
    -   `apply`: 즉시 호출, 인수를 배열로 전달
    -   `bind`: 새로운 함수를 반환, 나중에 호출 가능
    -   모두 `this` 바인딩을 명시적으로 제어할 때 사용합니다.

### 고급 개념 질문

**Q7. strict mode에서 일반 함수의 `this`가 `undefined`인 이유는 무엇인가요?**

-   A: strict mode에서는 `this`의 기본값이 `undefined`로 설정됩니다. 이는 의도하지 않은 전역 객체 접근을 방지하고, 더 안전한 코드 작성을 가능하게 합니다.

**Q8. 프로토타입 메서드에서 `this`가 어떻게 동작하는지 설명해주세요.**

-   A: 프로토타입 메서드의 `this`는 메서드를 호출한 인스턴스를 가리킵니다. 프로토타입 체인을 통해 메서드를 찾더라도, `this`는 원래 호출한 객체를 유지합니다.

**Q9. 이벤트 핸들러에서 `this`가 DOM 요소를 가리키는 이유는 무엇인가요?**

-   A: 이벤트 핸들러는 이벤트가 발생한 DOM 요소에 의해 호출되기 때문에, `this`는 해당 DOM 요소를 가리키게 됩니다. 이는 이벤트 처리 시 요소에 접근하기 쉽게 해줍니다.

**Q10. 클래스 메서드와 일반 객체 메서드의 `this` 바인딩 차이점은 무엇인가요?**

-   A: 클래스 메서드는 자동으로 strict mode로 실행되며, `this`는 클래스의 인스턴스를 가리킵니다. 일반 객체 메서드도 비슷하지만, 클래스는 더 엄격한 `this` 바인딩 규칙을 가집니다.

## 📝 추가 학습 포인트

### 핵심 암기 사항

1. **`this`는 호출 시점에 결정** - 정의 위치가 아닌 호출 방식이 중요
2. **화살표 함수는 lexical this** - 상위 스코프의 `this`를 그대로 사용
3. **strict mode에서는 `this`가 `undefined`** - 전역 객체 접근 방지
4. **`new.target`으로 생성자 함수 호출 확인** - 실수 방지 가능

### 실무에서 주의할 점

-   메서드 내부의 일반 함수에서 `this` 사용 시 주의
-   콜백 함수에서 `this` 바인딩 문제 해결을 위해 `bind` 사용
-   화살표 함수와 일반 함수의 `this` 동작 차이 이해
-   `new` 키워드 누락으로 인한 버그 방지
