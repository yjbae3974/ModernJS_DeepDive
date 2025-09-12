## 클로저란?

-   MDN에선 아래와 같이 정의.

```plaintext
A closure is the combination of a function and the lexical environment within which that function was declared.
```

-   즉, 함수와 그 함수가 선언된 렉시컬 환경의 조합.
-   외부 함수가 반환된 후에도 내부 함수가 외부 함수의 변수에 접근할 수 있는 현상.
-   먼저, '함수가 선언된 렉시컬 환경'에 대해 이해해보자.

```js
const x = 1;
function outerFunc() {
    const x = 10;
    function innerFunc() {
        console.log(x);
    }
    innerFunc();
}
outerFunc();
```

```js
const x = 1;
function outerFunc() {
    const x = 10;
    innerFunc();
}
function innerFunc() {
    console.log(x); // 1
}
outerFunc();
```

-   위 현상이 발생하는 이유는 js가 렉시컬 스코프를 따르는 프로그래밍 언어이기 때문이다.
-   `함수를 어디에서 정의했는지`가 중요하다.

### 렉시컬 스코프: 상위 스코프에 대한 참조는, 함수 정의가 평가되는 시점에 함수가 정의된 위치에 의해 정적으로 결졍된다.

-   이를 위해, 함수는 자신의 내부 슬롯 `[[Environment]]`에 상위 스코프의 참조를 저장한다.

## 클로저와 렉시컬 환경

```js
const x = 1;

// 1
function outer() {
    const x = 10;
    const inner = function () {
        console.log(x); // 2
    };
    return inner;
}
const innerFunc = outer(); // 3
innerFunc(); // 4
```

-   `3`에서 `outer`을 호출하면, `outer`은 중첩함수 `inner`을 반환하고 생명주기를 마감한다.
-   `outer` 함수의 실행이 종료되었으므로, `outer`함수의 실행 컨텍스트는 `실행 컨텍스트 스택`에서 제거된다.
-   따라서 `outer`의 지역변수 `x`도 생을 마감한다.
-   그러나, 위 코드(`4`번)의 실행결과는 10이다.

### 외부 함수보다 중첩 함수(inner)가 더 오래 유지되는경우, 중첩 함수는 외부 함수의 변수를 참조할 수 있다. 이를 `클로저`라고 한다.

-   outer함수의 실행이 종료되면, outer함수의 실행 컨텍스트는 제거되나, outer함수의 렉시컬 환경은 소멸되지 않는다.
    -   inner의 [[Environment]]에는 outer함수의 렉시컬 환경의 참조가 저장되어 있다. 따라서 garbage collector가 제거하지 않는다.

## 클로저의 활용

### 1. 데이터 은닉

-   좋지 못한 코드

```js
let num = 0;
function increase() {
    num++;
    console.log(num);
}
increase(); // 1
increase(); // 2
increase(); // 3
```

-   이렇게 하면, num은 전역 변수이므로, 어디서든 접근 가능하다.
-   increase함수 만이 num 변수를 참조하고 변경할 수 있게 하는 것이 바람직하다.
-   그럼 아래 코드는 어떨까?

```js
function increase() {
    let num = 0;
    num++;
    console.log(num);
}
increase(); // 1
increase(); // 1
increase(); // 1
```

-   이전 상태를 유지하지 못한다.
-   이전 상태를 유지할 수 있도록 클로저를 사용해보자.

```js
const increase = (function () {
    let num = 0;
    return function () {
        return ++num;
    };
})();
console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

-   이제 increase함수는 이전 상태를 유지하면서 증가한다.
-   이렇게 하면, num은 지역 변수이므로, 외부에서 접근할 수 없다.
-   이를 통해, 데이터 은닉을 구현할 수 있다.

```js
const counter = (function () {
    let num = 0;
    return {
        increase() {
            return ++num;
        },
        decrease() {
            return num > 0 ? --num : 0;
        },
    };
})();
console.log(counter.increase()); // 1
console.log(counter.increase()); // 2
console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```

-   함수형 프로그래밍에서의 클로저 사용

```js
function makeCounter(predicate) {
    let counter = 0;
    return function () {
        counter = predicate(counter);
        return counter;
    };
}
function increase(n) {
    return ++n;
}
function decrease(n) {
    return --n;
}
const increaser = makeCounter(increase);
const decreaser = makeCounter(decrease);
console.log(increaser()); // 1
console.log(increaser()); // 2
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

-   이렇게 하면, `increaser`와 `decreaser`는 독립적인 클로저를 생성하므로, `counter` 변수를 공유하지 않는다.

### 2. 캡슐화

-   캡슐화: 프로퍼티와 메소드를 하나로 묶음. 객체의 특정 프로퍼티나 메소드를 감출 목적으로 사용. 이를 `정보 은닉`이라고 함.
-   객체가 적절치 못한 접근으로 상태가 변경되는 것을 방지.
-   `결합도` 낮추는 효과가 있음.
-   ES2022부터, `#`을 붙여 private 메소드나 변수를 선언할 수 있음.

## 핵심 개념 요약

### 클로저란?

-   **함수와 그 함수가 선언된 렉시컬 환경의 조합**
-   **외부 함수가 반환된 후에도 내부 함수가 외부 함수의 변수에 접근할 수 있는 현상**
-   **중첩 함수가 외부 함수보다 더 오래 유지되는 경우, 중첩 함수는 외부 함수의 변수를 참조할 수 있음**

### 클로저의 동작 원리

-   **렉시컬 스코프**: 함수를 어디에서 정의했는지가 중요
-   **[[Environment]] 내부 슬롯**: 함수는 자신의 상위 스코프 참조를 저장
-   **가비지 컬렉션**: 외부 함수의 렉시컬 환경이 참조되고 있어 소멸되지 않음

### 클로저의 활용

1. **데이터 은닉**: 전역 변수 오염 방지, 상태 유지
2. **캡슐화**: 정보 은닉, 결합도 감소
3. **함수형 프로그래밍**: 독립적인 클로저 생성

## 면접 예상 질문

### 1. 클로저 기본 개념

**Q: 클로저란 무엇인가요?**

-   함수와 그 함수가 선언된 렉시컬 환경의 조합
-   외부 함수가 반환된 후에도 내부 함수가 외부 함수의 변수에 접근할 수 있는 현상

**Q: 다음 코드의 실행 결과는?**

```js
function outer() {
    let x = 10;
    return function inner() {
        console.log(x);
    };
}
const fn = outer();
fn(); // ?
```

### 2. 클로저의 동작 원리

**Q: 클로저가 동작하는 이유는?**

-   JavaScript가 렉시컬 스코프를 따르기 때문
-   함수의 [[Environment]] 내부 슬롯에 상위 스코프 참조가 저장됨
-   가비지 컬렉터가 참조되고 있는 렉시컬 환경을 제거하지 않음

**Q: 렉시컬 스코프란?**

-   상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 위치에 의해 정적으로 결정
-   함수를 어디에서 정의했는지가 중요

### 3. 클로저 활용 사례

**Q: 클로저를 사용한 데이터 은닉 예시를 보여주세요.**

```js
const counter = (function () {
    let count = 0;
    return {
        increment() {
            return ++count;
        },
        decrement() {
            return --count;
        },
        getCount() {
            return count;
        },
    };
})();
```

**Q: 전역 변수 대신 클로저를 사용하는 이유는?**

-   전역 변수 오염 방지
-   상태를 안전하게 캡슐화
-   모듈 패턴 구현 가능

### 4. 클로저와 메모리 관리

**Q: 클로저 사용 시 주의사항은?**

-   메모리 누수 가능성 (참조가 남아있으면 가비지 컬렉션되지 않음)
-   불필요한 클로저 생성 방지
-   적절한 스코프 관리

**Q: 다음 코드에서 메모리 누수가 발생할 수 있나요?**

```js
function createHandler() {
    const largeData = new Array(1000000).fill("data");
    return function () {
        console.log("handler");
    };
}
```

### 5. 클로저와 모듈 패턴

**Q: 클로저를 이용한 모듈 패턴의 장점은?**

-   네임스페이스 오염 방지
-   정보 은닉 구현
-   독립적인 상태 관리

**Q: ES6 모듈과 클로저 모듈 패턴의 차이는?**

-   ES6 모듈: 정적 모듈 시스템, 컴파일 타임에 결정
-   클로저 모듈: 동적 모듈 시스템, 런타임에 생성
