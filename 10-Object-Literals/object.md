## 객체란?

-   자바스크립트를 구성하는 거의 모든 것이 객체
-   원시 값을 제외한 나머지 값은 모두 객체
-   객체 타입은 다양한 하나의 타입의 값을 하나의 단위로 구성한 복합적인 자료구조.
-   변경 가능한 값이다!
-   객체는 키와 값의 쌍으로 이루어진 `프로퍼티`(property)로 구성
-   프로퍼티 값이 함수인 경우, 이를 `메서드`(method)라고 부름
    -   함수로 객체를 생성하기도 하며, 함수 자체가 객체이기도 함.

## 다양한 객체 생성법

1. 객체 리터럴
    1. 중괄호 `{}`를 이용한 객체 생성
2. Object 생성자 함수
    1. `new Object()`를 이용한 객체 생성
3. 생성자 함수
    1. `new` 연산자와 함께 호출하는 함수
4. Object.create 메서드
    1. `Object.create(proto, [propertiesObject])` 메서드를 이용한 객체 생성
5. 클래스
    1. `class` 키워드를 이용한 객체 생성(ES6에서 도입)

## 1. 객체 리터럴

-   가장 간단하고 직관적인 방법
-   중괄호 `{}` 안에 `키: 값` 쌍으로 프로퍼티를 정의

```js
const person = {
    name: "Alice",
    age: 30,
    greet: function () {
        console.log("Hello, " + this.name);
    },
};
console.log(person.name); // "Alice"
person.greet(); // "Hello, Alice"
```

-   `this` 키워드는 객체 자신을 가리킴
-   객체 리터럴 이외의 객체 생성 방식은 모두 함수를 사용한 생성 방법.

## 프로퍼티

-   객체는 `키: 값` 쌍으로 이루어진 프로퍼티로 구성
-   키는 문자열 또는 심볼이어야 함. js네이밍 규칙을 따르지 않는 키는 따옴표로 감싸야 함.
-   예약어를 키로 사용할 수 있음.(권장하지 않음)
-   키에 `0`이 들어가면, 내부적으로 문자열 `"0"`으로 변환됨.
-   값은 모든 타입이 가능

```js
const obj = {
    "first-name": "John", // 문자열 키
    age: 25, // 숫자 값
    isStudent: false, // 불리언 값
    address: { city: "New York", zip: "10001" }, // 객체 값
    hobbies: ["reading", "traveling"], // 배열 값
    greet: function () {
        console.log("Hello!");
    }, // 함수 값 (메서드). 함수는 객체이기에 값으로 올 수 있음
};
```

## 프로퍼티 접근

-   점 표기법(dot notation)

```js
console.log(obj.age); // 25
obj.greet(); // "Hello!"
```

-   대괄호 표기법(bracket notation)

```js
console.log(obj["first-name"]); // "John"
obj["greet"](); // "Hello!"
```

-   대괄호 표기법은 키가 변수에 저장되어 있거나, 공백이나 특수 문자가 포함된 경우에 유용

## 프로퍼티 추가, 수정, 삭제

-   추가 및 수정

```js
let obj = {};
obj.name = "Alice"; // 추가
obj.age = 30; // 추가
obj.age = 31; // 수정
```

-   삭제

```js
delete obj.age; // age 프로퍼티 삭제
console.log(obj.age); // undefined
```

## ES6 이후의 객체 리터럴 기능

-   프로퍼티 축약 표현

```js
let name = "Bob";
let age = 25;
let person = { name, age }; // { name: "Bob", age: 25 }
console.log(person);
```

-   계산된 프로퍼티 이름

```js
let propName = "score";
let obj = {
    [propName]: 100, // { score: 100 }
};
console.log(obj);
```

-   메서드 축약 표현

```js
let obj = {
    greet() {
        console.log("Hello!");
    },
};
obj.greet(); // "Hello!"
```

## 몰랐던 것 📝

-   자바스크립트를 구성하는 거의 모든 것이 객체이다
-   객체는 변경 가능한 값(mutable value)이다
-   객체는 키와 값의 쌍으로 이루어진 프로퍼티로 구성된다
-   프로퍼티 값이 함수인 경우를 메서드라고 부른다
-   객체 생성 방법이 5가지나 있다 (리터럴, Object 생성자, 생성자 함수, Object.create, 클래스)
-   키는 문자열 또는 심볼이어야 하고, 숫자 키는 내부적으로 문자열로 변환된다
-   ES6 이후 프로퍼티 축약, 계산된 프로퍼티 이름, 메서드 축약 표현이 추가되었다

## 기술 질문 대비 🤔

**Q: 객체란 무엇인가요?**<br />
A: 자바스크립트를 구성하는 거의 모든 것이 객체입니다. 원시 값을 제외한 나머지 값은 모두 객체이며, 키와 값의 쌍으로 이루어진 프로퍼티로 구성된 변경 가능한 값입니다.

**Q: 객체를 생성하는 방법은 어떤 것들이 있나요?**<br />
A: 1) 객체 리터럴(`{}`), 2) Object 생성자 함수(`new Object()`), 3) 생성자 함수(`new` 연산자와 함께), 4) Object.create 메서드, 5) 클래스(ES6) 등 5가지 방법이 있습니다.

**Q: 프로퍼티와 메서드의 차이점은 무엇인가요?**<br />
A: 프로퍼티는 객체의 키와 값의 쌍을 말하고, 프로퍼티 값이 함수인 경우를 메서드라고 부릅니다. 메서드는 객체의 동작을 정의하는 함수입니다.

**Q: 프로퍼티에 접근하는 방법은 무엇인가요?**<br />
A: 1) 점 표기법(`obj.property`), 2) 대괄호 표기법(`obj['property']`) 두 가지 방법이 있습니다. 대괄호 표기법은 키가 변수에 저장되어 있거나 공백이나 특수 문자가 포함된 경우에 유용합니다.

**Q: 프로퍼티를 추가, 수정, 삭제하는 방법은 무엇인가요?**<br />
A: 추가/수정은 `obj.property = value` 또는 `obj['property'] = value`로 하고, 삭제는 `delete obj.property`로 합니다. 삭제된 프로퍼티는 `undefined`를 반환합니다.

**Q: ES6의 객체 리터럴 기능에는 어떤 것들이 있나요?**<br />
A: 1) 프로퍼티 축약 표현(`{name, age}`), 2) 계산된 프로퍼티 이름(`{[propName]: value}`), 3) 메서드 축약 표현(`greet() {}`) 등이 있습니다.

**Q: 객체의 키로 사용할 수 있는 타입은 무엇인가요?**<br />
A: 문자열 또는 심볼만 사용할 수 있습니다. 숫자 키는 내부적으로 문자열로 변환되고, 예약어도 키로 사용할 수 있지만 권장하지 않습니다.

**Q: 객체와 원시 값의 차이점은 무엇인가요?**<br />
A: 객체는 변경 가능한 값(mutable)이고 참조에 의한 전달을 하며, 원시 값은 변경 불가능한 값(immutable)이고 값에 의한 전달을 합니다. 객체는 복합적인 자료구조이고, 원시 값은 단순한 값입니다.
