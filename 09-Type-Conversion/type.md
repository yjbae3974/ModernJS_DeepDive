## 타입 변환이란?

-   개발자가 의도적으로 타입 변환: explicit coercion or type casting
-   자바스크립트 엔진이 자동으로 타입 변환: implicit coercion or type coercion
    -   `암묵적 타입 변환`은, js엔진이 새로운 타입 값을 만들어 단 한 번 사용하고 버림.

## 문자열 타입으로 변환

-   '+' 연산자가 피연산자 중 하나가 문자열이면 문자열 연결 연산자로 동작하므로 이를 이용.

```js
console.log("1" + 2); // '12'
console.log(1 + "2"); // '12'
console.log("1" + true); // '1true'
console.log(1 + ""); // '1'
console.log(true + ""); // 'true'
console.log(NaN + ""); // 'NaN'
console.log(null + ""); // 'null'
console.log({} + ""); // '[object Object]'
```

## 숫자 타입으로 변환

-   `-`, `*`, `/` 연산자는 피연산자를 숫자 타입으로 암묵적 타입 변환 후 산술 연산을 수행.
-   `+` 연산자는 피연산자 중 하나가 문자열이면 문자열 연결 연산자로 동작하므로 주의.
    -   단, 단항 연산자로 쓰이면 숫자타입의 값으로 변환 수행.

```js
console.log("6" / "2"); // 3
console.log(+""); // 0
console.log(+"123"); // 123
console.log(+true); // 1
console.log(+false); // 0
console.log(+null); // 0
```

## boolean 타입으로 변환

-   JS 엔진은 boolean이 아닌 값을, Truthy한 값과 Falsy한 값으로 구분.
-   Falsy한 값: false, undefined, null, 0, -0, NaN, ''
-   Truthy한 값: Falsy한 값을 제외한 모든 값.

```js
function isTruthy(value) {
    return !!value; // not falsy.
}
console.log(isTruthy(1)); // true
console.log(isTruthy("hello")); // true
console.log(isTruthy([])); // true
console.log(isTruthy({})); // true
console.log(isTruthy(function () {})); // true
```

## 지금까지는 암묵적 타입 변환에 대해 알아보았고, 다음은 명시적 타입 변환에 대해 알아봅시다.

### 문자열로 반환

1. String 생성자 함수를 new 연산자 없이 호출.

```js
console.log(String(123)); // '123'
```

2. Object.prototype.toString 메서드 호출.

```js
console.log((123).toString()); // '123'
```

3. 문자열 연결 연산자를 이용하는 방법

```js
console.log(123 + ""); // '123'
```

### 숫자로 반환

1. Number 생성자 함수를 new 연산자 없이 호출.

```js
console.log(Number("123")); // 123
```

2. parseInt, parseFloat 함수를 이용하는 방법

```js
console.log(parseInt("123")); // 123
console.log(parseFloat("123.45")); // 123.45
```

3. 단항 덧셈 연산자(+)를 이용하는 방법

```js
console.log(+"123"); // 123
console.log(+true); // 1
```

4. (\*) 산술 연산자를 이용하는 방법

```js
console.log("123" * 1); // 123
```

### 예제

```js
console.log(1 + "2" * 1); //3
console.log(1 + +"2"); //3
```

### boolean으로 반환

1. Boolean 생성자 함수를 new 연산자 없이 호출.

```js
console.log(Boolean(1)); // true
```

2. ! 부정 논리 연산자를 두 번 적용.

```js
console.log(!!1); // true
```

## 단축 평가

```js
console.log("Cat" && "Dog"); // 'Dog'
```

-   논리 연산자 `&&`, `||`는 피연산자를 boolean 타입으로 암묵적 타입 변환하지 않고, 피연산자를 그대로 반환.
-   `&&` 연산자는 좌항이 Truthy한 값이면 우항을 반환, 좌항이 Falsy한 값이면 좌항을 반환.

```js
console.log(NaN && "Dog"); // NaN
console.log("Cat" && "Dog"); // 'Dog'
console.log(0 && "Dog"); // 0
```

-   `||` 연산자는 좌항이 Truthy한 값이면 좌항을 반환, 좌항이 Falsy한 값이면 우항을 반환.

```js
console.log("Cat" || "Dog"); // 'Cat'
console.log(NaN || "Dog"); // 'Dog'
console.log(0 || "Dog"); // 'Dog'
```

-   단축 평가를 통해 if문을 대체할 수도 있다.

### 사용법(중요)

1. 객체가 가리키기를 기대하는 변수가 null 혹은 undefined는 아닌지 판단하고 프로퍼티 참조

```js
let elem = null;
let value = elem && elem.value; // elem이 null이면 elem.value를 평가하지 않음.
console.log(value); // null
```

2. 기본값 설정

```js
function getStringLength(str) {
    str = str || ""; // str이 Falsy한 값이면 ''로 설정.
    return str.length;
}
console.log(getStringLength()); // 0
```

## 옵셔널 체이닝 연산자

-   좌항의 피연산자가 null or undefined이면 우항의 피연산자를 평가하지 않고 undefined를 반환.

```js
let elem = null;
console.log(elem?.value); // undefined
```

-   중요한 점! 좌항의 피연산자가 falsy한 값이어도 null or undefined가 아니면 우항의 피연산자를 평가.

```js
let elem = 0;
console.log(elem?.value); // TypeError
```

## null 병합 연산자

-   좌항의 피연산자가 null or undefined이면 우항의 피연산자를 반환.

```js
let foo = null ?? "default string";
console.log(foo); // 'default string'
let bar = undefined ?? "default string";
console.log(bar); // 'default string'
let baz = 0 ?? 42;
console.log(baz); // 0
```

-   ||와 다른 점: ||는 좌항이 falsy한 값이면 우항을 반환하지만, ??는 좌항이 null or undefined일 때만 우항을 반환.

```js
let foo = "" || "default string";
console.log(foo); // 'default string'
let bar = "" ?? "default string";
console.log(bar); // ''
```

-   즉, 실제 코드에서 사용할 때 기본값을 설정할 때, 우리가 의도한 값인 falsy가 올 수 있으므로, null 병합 연산자(??)를 사용하는 것이 더 안전하다.
-   예시) 좌항에 변수를 넣었을 때, 0 이나 ''가 의도한 상황일 수도 있다!

## 몰랐던 것 📝

-   타입 변환은 명시적 타입 변환(개발자 의도)과 암묵적 타입 변환(엔진 자동)으로 나뉜다
-   암묵적 타입 변환은 새로운 타입 값을 만들어 단 한 번 사용하고 버린다
-   `+` 연산자는 피연산자 중 하나가 문자열이면 문자열 연결 연산자로 동작한다
-   `-`, `*`, `/` 연산자는 피연산자를 숫자 타입으로 암묵적 변환 후 산술 연산을 수행한다
-   Falsy 값은 false, undefined, null, 0, -0, NaN, ''이다
-   논리 연산자 `&&`, `||`는 boolean 변환 없이 피연산자를 그대로 반환한다
-   옵셔널 체이닝(`?.`)은 null/undefined일 때만 undefined를 반환한다
-   null 병합 연산자(`??`)는 null/undefined일 때만 우항을 반환한다

## 기술 질문 대비 🤔

**Q: 명시적 타입 변환과 암묵적 타입 변환의 차이점은 무엇인가요?**<br />
A: 명시적 타입 변환은 개발자가 의도적으로 타입을 변환하는 것이고, 암묵적 타입 변환은 자바스크립트 엔진이 자동으로 타입을 변환하는 것입니다. 암묵적 타입 변환은 새로운 타입 값을 만들어 단 한 번 사용하고 버립니다.

**Q: 문자열로 타입 변환하는 방법은 무엇인가요?**<br />
A: 1) `String()` 생성자 함수 사용, 2) `toString()` 메서드 사용, 3) 문자열 연결 연산자(`+`) 사용, 4) 템플릿 리터럴 사용 등이 있습니다.

**Q: 숫자로 타입 변환하는 방법은 무엇인가요?**<br />
A: 1) `Number()` 생성자 함수 사용, 2) `parseInt()`, `parseFloat()` 함수 사용, 3) 단항 덧셈 연산자(`+`) 사용, 4) 산술 연산자(`*`, `/`) 사용 등이 있습니다.

**Q: Falsy 값에는 어떤 것들이 있나요?**<br />
A: false, undefined, null, 0, -0, NaN, ''(빈 문자열)이 있습니다. 이들을 제외한 모든 값은 Truthy 값입니다.

**Q: 단축 평가란 무엇인가요?**<br />
A: 논리 연산자 `&&`, `||`가 피연산자를 boolean으로 변환하지 않고 그대로 반환하는 것을 말합니다. `&&`는 좌항이 Truthy면 우항을, Falsy면 좌항을 반환하고, `||`는 좌항이 Truthy면 좌항을, Falsy면 우항을 반환합니다.

**Q: 옵셔널 체이닝 연산자(`?.`)의 특징은 무엇인가요?**<br />
A: 좌항의 피연산자가 null 또는 undefined일 때만 우항의 피연산자를 평가하지 않고 undefined를 반환합니다. 다른 Falsy 값(0, '', false 등)은 평가를 계속 진행합니다.

**Q: null 병합 연산자(`??`)와 논리 OR 연산자(`||`)의 차이점은 무엇인가요?**<br />
A: `||`는 좌항이 Falsy 값이면 우항을 반환하지만, `??`는 좌항이 null 또는 undefined일 때만 우항을 반환합니다. 0이나 '' 같은 의도한 Falsy 값을 보존하려면 `??`를 사용해야 합니다.

**Q: 타입 변환 시 주의사항은 무엇인가요?**<br />
A: 1) 암묵적 타입 변환의 예측 어려움, 2) `+` 연산자의 문자열 연결 동작 주의, 3) Falsy 값의 정확한 이해, 4) 단축 평가의 활용, 5) 옵셔널 체이닝과 null 병합 연산자의 적절한 사용이 중요합니다.
