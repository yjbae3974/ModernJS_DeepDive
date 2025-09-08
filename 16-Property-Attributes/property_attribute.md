## 내부 슬롯과 내부 메서드

-   ECMAScript 사양에서 객체의 동작을 정의하는 데 사용되는 pseudo 개념
-   `[[...]]`로 표기
-   직접 접근 불가, 자바스크립트 코드에서 직접 호출 불가

## 프로퍼티 어트리뷰트, 프로퍼티 디스크립터 객체

-   JS엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 property attributes(프로퍼티 어트리뷰트)를 함께 생성
-   `[[Value]]`, `[[Writable]]`, `[[Enumerable]]`, `[[Configurable]]` 4가지 기본 어트리뷰트
-   `Object.getOwnPropertyDescriptor` 메서드를 사용해 `프로퍼티 디스크립터 객체`로 확인 가능

## 데이터 프로퍼티와 접근자 프로퍼티

-   데이터 프로퍼티
    -   키와 값으로 구성. 지금까지 살펴본 모든 프로퍼티가 해당.
-   접근자 프로퍼티
    -   자체적으로 값을 갖지 않고, 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성
    -   `getter`와 `setter` 함수로 구성
    -   `Object.defineProperty` 메서드를 사용해 생성 가능

```js
const person = {
    firstName: "YeunJoon",
    lastName: "Bae",

    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    },
    set fullName(name) {
        [this.firstName, this.lastName] = name.split(" ");
    },
};

console.log(person); // {firstName: 'YeunJoon', lastName: 'Bae'}
person.fullName = "Lee JaeHoon";
console.log(person); // {firstName: 'Lee', lastName: 'JaeHoon'}
console.log(person.fullName); // Lee JaeHoon

let descriptor = Object.getOwnPropertyDescriptor(person, "firstName");
console.log(descriptor);
// {value: 'YeunJoon', writable: true, enumerable: true, configurable: true}
descriptor = Object.getOwnPropertyDescriptor(person, "fullName");
console.log(descriptor);
// {get: ƒ, set: ƒ, enumerable: true, configurable: true}
```

-   여기서 `fullName`이 접근자 프로퍼티.
-   `get`, `set`이 `getter`, `setter` 함수.

## Prototype

-   `프로토타입`은 어떤 객체의 상위 객체의 역할을 하는 객체다.
-   모든 객체는 `프로토타입`을 갖는다.
-   프로토타입은 하위 객체엑 자신의 프로퍼티와 메소드를 상속한다.
-   `프로토타입 체인`: 객체가 자신의 프로퍼티를 검색할 때, 자신의 프로퍼티에 없으면 자신의 프로토타입에서 검색하고, 또 없으면 그 프로토타입에서 검색하는 식으로 계속 올라가는 것.

## 프로퍼티 정의

```js
const person = {};

Object.defineProperty(person, "firstName", {
    value: "YeunJoon",
    writable: true,
    enumerable: true,
    configurable: true,
});
Object.defineProperty(person, "lastName", {
    value: "Bae",
});
let descriptor = Object.getOwnPropertyDescriptor(person, "firstName");
console.log(descriptor);
// {value: 'YeunJoon', writable: true, enumerable: true, configurable: true}

//디스크립터 객체에 프로퍼티를 누락시키면 undefined, false가 기본값.
descriptor = Object.getOwnPropertyDescriptor(person, "lastName");
console.log(descriptor);
// {value: 'Bae', writable: false, enumerable: false, configurable: false}
```

-   [[Enumerable]]이 false라면, for...in문이나 Object.keys 메서드로 프로퍼티를 열거할 수 없다.
-   [[Writeable]]이 false라면, 프로퍼티의 값을 변경할 수 없다.
-   [[Configurable]]이 false라면, 프로퍼티 어트리뷰트를 변경하거나 프로퍼티를 삭제할 수 없다.
    -   삭제를 시도하면, 에러를 뱉지 않고 그냥 무시된다.

```js
Object.defineProperty(person, "fullName", {
    get() {
        return `${this.firstName} ${this.lastName}`;
    },
    set(name) {
        [this.firstName, this.lastName] = name.split(" ");
    },
    enumerable: true,
    configurable: true,
});
descriptor = Object.getOwnPropertyDescriptor(person, "fullName");
console.log(descriptor);
// {get: ƒ, set: ƒ, enumerable: true, configurable: true}
```

-   `Object.defineProperties` 메서드를 사용하면, 여러개의 프로퍼티를 한꺼번에 정의할 수 있다.

## 객체 변경 감지

-   js는 객체 변경을 방지하는 다양한 메서드 제공
-   `Object.preventExtensions`: 객체 확장 금지. 프로퍼티 추가 불가.
-   `Object.seal`: 객체 밀봉. 프로퍼티 추가, 삭제 불가. 기존 프로퍼티의 어트리뷰트 변경 불가.
-   `Object.freeze`: 객체 동결. 프로퍼티 추가, 삭제 불가. 기존 프로퍼티의 어트리뷰트 변경 불가. 값 변경 불가.

```js
const person = {
    name: "Lee",
};
Object.preventExtensions(person);
person.age = 20; // 무시
console.log(Object.isExtensible(person)); // false
delete person.name; // 삭제는 가능.
console.log(person); // {}
Object.defineProperty(person, "age", {
    value: 20,
}); // TypeError: Cannot define property age, object is not extensible
```

```js
const person = {
    name: "Lee",
};
Object.seal(person);
console.log(Object.isSealed(person)); // true
person.age = 20; // 무시
delete person.name; // 무시
person.name = "Kim"; // 값 변경은 가능.
console.log(person); // {name: 'Lee'}
```

```js
const person = {
    name: "Lee",
};
Object.freeze(person);
console.log(Object.isFrozen(person)); // true
person.age = 20; // 무시
delete person.name; // 무시
person.name = "Kim"; // 무시
console.log(person); // {name: 'Lee'}
```

### 불변 객체

-   지금까지는 얕은 변경 방지.
-   깊은 변경 방지를 위해서는 재귀적으로 모든 중첩 객체에 대해 변경 방지 메서드를 호출해야 한다.

## 몰랐던 것 📝

-   내부 슬롯과 내부 메서드는 ECMAScript 사양의 pseudo 개념으로 `[[...]]`로 표기되며 직접 접근 불가
-   프로퍼티 어트리뷰트는 4가지: `[[Value]]`, `[[Writable]]`, `[[Enumerable]]`, `[[Configurable]]`
-   데이터 프로퍼티는 키와 값으로 구성, 접근자 프로퍼티는 getter/setter 함수로 구성
-   접근자 프로퍼티는 `Object.defineProperty`로 생성 가능하며 자체 값 없이 다른 프로퍼티 값 읽기/저장
-   프로토타입은 객체의 상위 객체 역할을 하며 프로퍼티와 메서드를 상속
-   프로토타입 체인은 객체가 자신의 프로퍼티를 찾지 못하면 상위 프로토타입에서 검색하는 과정
-   객체 변경 방지 메서드 3가지: `preventExtensions`(확장 금지), `seal`(밀봉), `freeze`(동결)
-   불변 객체 구현을 위해서는 중첩 객체에 대해 재귀적으로 변경 방지 메서드 호출 필요

## 기술 질문 대비 🤔

**Q: 내부 슬롯과 내부 메서드란 무엇인가요?**<br />
A: ECMAScript 사양에서 객체의 동작을 정의하는 데 사용되는 pseudo 개념입니다. `[[...]]`로 표기되며, 자바스크립트 코드에서 직접 접근하거나 호출할 수 없습니다. 예를 들어 `[[Prototype]]`, `[[Value]]` 등이 있습니다.

**Q: 프로퍼티 어트리뷰트의 4가지 종류는 무엇인가요?**<br />
A: 1) `[[Value]]`: 프로퍼티의 값, 2) `[[Writable]]`: 값 변경 가능 여부, 3) `[[Enumerable]]`: 열거 가능 여부, 4) `[[Configurable]]`: 어트리뷰트 변경 및 삭제 가능 여부입니다.

**Q: 데이터 프로퍼티와 접근자 프로퍼티의 차이점은 무엇인가요?**<br />
A: 데이터 프로퍼티는 키와 값으로 구성된 일반적인 프로퍼티이고, 접근자 프로퍼티는 자체적으로 값을 갖지 않고 getter/setter 함수를 통해 다른 데이터 프로퍼티의 값을 읽거나 저장하는 프로퍼티입니다.

**Q: 접근자 프로퍼티를 어떻게 생성하나요?**<br />
A: `Object.defineProperty` 메서드를 사용하거나 객체 리터럴에서 get/set 키워드를 사용합니다. getter는 값을 읽을 때, setter는 값을 저장할 때 호출됩니다.

**Q: 다음 코드의 실행 결과를 설명해주세요.**<br />

```javascript
const person = {
    firstName: "YeunJoon",
    lastName: "Bae",
    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    },
    set fullName(name) {
        [this.firstName, this.lastName] = name.split(" ");
    }
};
person.fullName = "Lee JaeHoon";
console.log(person.fullName);
```
<br />
A: "Lee JaeHoon"이 출력됩니다. setter가 호출되어 firstName과 lastName이 "Lee", "JaeHoon"으로 변경되고, getter가 호출되어 변경된 값들이 조합되어 출력됩니다.

**Q: 프로토타입과 프로토타입 체인이란 무엇인가요?**<br />
A: 프로토타입은 객체의 상위 객체 역할을 하며 프로퍼티와 메서드를 상속합니다. 프로토타입 체인은 객체가 자신의 프로퍼티를 찾지 못하면 상위 프로토타입에서 검색하고, 또 없으면 그 상위 프로토타입에서 검색하는 과정을 말합니다.

**Q: 객체 변경 방지 메서드 3가지를 설명해주세요.**<br />
A: 1) `Object.preventExtensions`: 객체 확장 금지, 프로퍼티 추가 불가, 2) `Object.seal`: 객체 밀봉, 프로퍼티 추가/삭제 불가, 어트리뷰트 변경 불가, 3) `Object.freeze`: 객체 동결, 프로퍼티 추가/삭제/값 변경 불가, 어트리뷰트 변경 불가

**Q: Object.seal과 Object.freeze의 차이점은 무엇인가요?**<br />
A: `Object.seal`은 프로퍼티 추가/삭제와 어트리뷰트 변경을 금지하지만 기존 프로퍼티의 값 변경은 가능합니다. `Object.freeze`는 모든 변경을 금지하며 값 변경도 불가능합니다.

**Q: 불변 객체를 구현하려면 어떻게 해야 하나요?**<br />
A: 기본적으로는 얕은 변경 방지만 가능합니다. 깊은 변경 방지를 위해서는 재귀적으로 모든 중첩 객체에 대해 `Object.freeze` 같은 변경 방지 메서드를 호출해야 합니다.

**Q: 프로퍼티 어트리뷰트를 확인하는 방법은 무엇인가요?**<br />
A: `Object.getOwnPropertyDescriptor` 메서드를 사용하여 프로퍼티 디스크립터 객체로 확인할 수 있습니다. 이 객체에는 해당 프로퍼티의 모든 어트리뷰트 정보가 포함됩니다.

````
