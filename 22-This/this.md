## this

-   메서드는 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.
-   객체 리터럴 방식으로 생성한 객체는 재귀적으로 자신 참조 가능.

```js
const Circle = {
    radius: 5,
    getDiameter() {
        return 2 * Circle.radius; // 가능은 하나, 비권장
    },
};
console.log(Circle.getDiameter());
```

-   근데, 생성자 함수 방식은?

```js
function Circle(radius) {
    //이 시점에서 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
    ????.radius = radius;
}
Circle.prototype.getDiameter = function () {
    //이 시점에서도 마찬가지다.
    return 2 * ????.radius;
}
const circle = new Circle(10); // 이 작업이 이루어져야 인스턴스가 생성된다.
```

-   생성자 함수로 인스턴스를 생성하려면 생성자 함수가 존재해야하는데, 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
-   이를 위해, `this`라는 특수한 식별자를 사용한다.
-   `this`는 생성자 함수가 생성할 인스턴스, 혹은 자신이 속한 객체를 가리킨다.

## this 바인딩

-   바인딩이란, 식별자와 값을 연결하는 과정을 의미한다.
-   따라서 `this` 바인딩은, `this`와 값을 연결하는 과정을 의미한다.
-   이는 함수 호출 방식에 의해 동적으로 결정된다.
-   객체 리터럴
    -   객체 자신 가리킴
-   생성자 함수
    -   생성자 함수가 생성할 인스턴스를 가리킴
-   일반 함수
    -   전역 객체를 가리킴

### prototype의 apply/call/bind

#### call - 즉시 실행, 인수를 개별적으로 전달

```js
const person = {
    name: "John",
    greet: function (greeting, punctuation) {
        console.log(`${greeting}, ${this.name}${punctuation}`);
    },
};

const anotherPerson = { name: "Jane" };

// person의 greet 메서드를 anotherPerson의 this로 실행
person.greet.call(anotherPerson, "Hello", "!"); // "Hello, Jane!"
```

#### apply - 즉시 실행, 인수를 배열로 전달

```js
const numbers = [1, 2, 3, 4, 5];

// Math.max를 배열에 적용
const max = Math.max.apply(null, numbers);
console.log(max); // 5

// 또는 스프레드 문법 사용 (ES6+)
const max2 = Math.max(...numbers);
```

#### bind - 새로운 함수 생성, 나중에 실행

```js
const person = {
    name: "John",
    greet: function () {
        console.log(`Hello, ${this.name}!`);
    },
};

const anotherPerson = { name: "Jane" };

// 새로운 함수 생성 (아직 실행하지 않음)
const boundGreet = person.greet.bind(anotherPerson);

// 나중에 실행
boundGreet(); // "Hello, Jane!"

// 이벤트 핸들러에서 유용
button.addEventListener("click", person.greet.bind(anotherPerson));
```

#### 요약

-   **call**: 함수를 즉시 실행, 인수를 개별적으로 전달
-   **apply**: 함수를 즉시 실행, 인수를 배열로 전달
-   **bind**: 새로운 함수를 생성, 나중에 실행 가능

## 면접 예상 질문

### 1. this 바인딩 관련

**Q: JavaScript에서 this가 결정되는 방식은?**

-   함수 호출 방식에 따라 동적으로 결정
-   일반 함수: 전역 객체 (strict mode에서는 undefined)
-   메서드: 호출한 객체
-   생성자 함수: 생성할 인스턴스
-   화살표 함수: 상위 스코프의 this

**Q: 다음 코드의 결과는?**

```js
const obj = {
    name: "John",
    greet: function () {
        console.log(this.name);
    },
};

const greet = obj.greet;
greet(); // ?
```

### 2. call, apply, bind 관련

**Q: call과 apply의 차이점은?**

-   call: 인수를 개별적으로 전달
-   apply: 인수를 배열로 전달

**Q: bind의 특징과 사용 사례는?**

-   새로운 함수를 생성하여 반환
-   this 바인딩을 고정
-   이벤트 핸들러, 콜백 함수에서 유용

**Q: 다음 코드에서 bind를 사용하지 않으면 어떻게 될까?**

```js
const obj = {
    name: "John",
    greet: function () {
        console.log(this.name);
    },
};

setTimeout(obj.greet, 1000); // ?
setTimeout(obj.greet.bind(obj), 1000); // ?
```

### 3. 화살표 함수와 this

**Q: 화살표 함수의 this는 어떻게 결정되나?**

-   상위 스코프의 this를 상속
-   자신만의 this를 가지지 않음

**Q: 다음 코드의 결과는?**

```js
const obj = {
    name: "John",
    greet: () => {
        console.log(this.name);
    },
};

obj.greet(); // ?
```

### 4. 클래스와 this

**Q: 클래스 메서드에서 this는 무엇을 가리키나?**

-   클래스의 인스턴스를 가리킴
-   메서드를 다른 변수에 할당하면 this 바인딩이 사라짐

**Q: 클래스 메서드를 이벤트 핸들러로 사용할 때 주의사항은?**

-   bind를 사용하거나 화살표 함수로 정의해야 함
