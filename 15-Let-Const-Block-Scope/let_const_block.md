## var의 문제점

1. 중복 선언 가능
2. 블록 레벨 스코프 미지원. 함수 레벨 스코프만 지원.
3. 변수 호이스팅

## 대안: let, const

## let

1. 변수 중복 선언 불가
2. 블록 레벨 스코프 지원
3. 변수 호이스팅 발생, 단, 초기화 이전에 참조 불가(Temporal Dead Zone)

```js
console.log(x); // ReferenceError
console.log(foo); // undefined
let x = 3;
var foo;
```

```js
let foo = 1;
{
    console.log(foo); // ReferenceError
    let foo = 2;
}
```

-   위 예제에서 만약 호이스팅이 일어나지 않는다면, 블록 내부의 `console.log(foo)`는 외부 스코프의 `foo`를 참조하여 `1`을 출력해야 한다.
-   그러나, `let` 키워드로 선언된 변수는 호이스팅이 발생하지만, 초기화 이전에 참조할 수 없으므로, `ReferenceError`가 발생한다.

### ES6에서 도입된 `let`, `const`, `class` 키워드는 모두 호이스팅이 발생하지만, 초기화 이전에 참조할 수 없다.

### 잊지 말아야할 것은, 결국 js는 모든 변수 선언을 `호이스팅`한다는 것이다.

### 전역 객체와 let

-   `var` 키워드로 선언된 전역 변수는 전역 객체(window)의 프로퍼티가 된다.
-   `let`, `const`, `class` 키워드로 선언된 전역 변수는 전역 객체의 프로퍼티가 되지 않는다.
-   `let`은 보이지 않는 개념적인 블록 내에 존재하게 된다.

```js
var x = 1;
let y = 2;
console.log(window.x); // 1
console.log(window.y); // undefined
```

## const

-   상수를 선언하기 위해 사용.
-   그러나, 상수만을 위해 사용하지는 않음

### 특징

-   반드시 선언과 동시에 초기화

```js
const x; // SyntaxError
const x = 1; // ok
```

-   블록 레벨 스코프 가지고, 변수 호이스팅이 발생하지 않는 것 처럼 동작.
-   재할당 금지.
-   일반적으로 상수 이름은 대문자로 선언해서 상수임을 명확히 나타낸다.

```js
const PI = 3.14;
let area_of_circle_5 = PI * 5 * 5;
```

### const와 객체

-   `const` 키워드로 선언된 변수에 객체를 할당할 수 있다.
-   변경 불가능한 값인 원시값은 재할당 없이 변경할 수 있는 방법이 없지만, 객체는 프로퍼티를 추가, 삭제, 변경할 수 있다.

```js
const person = {
    name: "Lee",
};
person.name = "Kim"; // ok
person.age = 20; // ok
```

### var vs let vs const

1. ES6를 사용한다면, `var` 키워드는 사용하지 않는다.
2. 재할당이 필요 없는 경우에는 `const` 키워드를 사용한다.
3. 재할당이 필요한 경우에는 `let` 키워드를 사용한다. 단, 스코프는 최대한 좁게 만든다.

Q)

```ts
export const satisfactionLevel = ["좋았어요", "괜찮아요", "아쉬워요"] as const;
```

이때 `as const`는 무엇을 의미하는가?<br />
A) `as const`는 TypeScript에서 사용하는 구문으로, 객체나 배열을 리터럴 타입으로 변환합니다. 즉, 해당 객체나 배열의 모든 프로퍼티가 읽기 전용(`readonly`)이 되고, 값 자체가 변경될 수 없도록 만듭니다.
따라서, `satisfactionLevel` 배열의 각 요소는 더 이상 변경할 수 없으며, 배열 전체도 변경할 수 없습니다. 이는 코드의 안정성을 높이고, 의도치 않은 변경을 방지하는 데 도움이 됩니다.

## 몰랐던 것 📝

-   var의 3가지 문제점: 중복 선언 가능, 함수 레벨 스코프만 지원, 변수 호이스팅
-   let과 const는 ES6에서 도입된 블록 레벨 스코프를 지원하는 변수 선언 키워드
-   let은 중복 선언 불가, 블록 레벨 스코프, 호이스팅 발생하지만 TDZ로 초기화 이전 참조 불가
-   const는 선언과 동시에 초기화 필수, 재할당 금지, 객체의 프로퍼티는 변경 가능
-   let/const/class는 호이스팅되지만 초기화 이전에 참조할 수 없는 TDZ(Temporal Dead Zone) 존재
-   var로 선언된 전역 변수는 window 객체의 프로퍼티가 되지만, let/const는 그렇지 않음
-   ES6 사용 시 var 대신 let/const 사용, 재할당 불필요하면 const, 필요하면 let 사용 권장

## 기술 질문 대비 🤔

**Q: var의 문제점은 무엇인가요?**<br />
A: 1) 중복 선언 가능: 같은 스코프에서 동일한 이름으로 변수를 여러 번 선언할 수 있어 의도치 않은 재할당 가능, 2) 함수 레벨 스코프만 지원: 블록 레벨 스코프를 지원하지 않아 예상과 다른 동작, 3) 변수 호이스팅: 선언 이전에 undefined로 참조 가능하여 예측하기 어려운 코드

**Q: let과 var의 차이점은 무엇인가요?**<br />
A: let은 중복 선언이 불가능하고, 블록 레벨 스코프를 지원하며, 호이스팅이 발생하지만 TDZ(Temporal Dead Zone)로 인해 초기화 이전에 참조할 수 없습니다. var는 중복 선언이 가능하고, 함수 레벨 스코프만 지원하며, 호이스팅 시 undefined로 초기화됩니다.

**Q: Temporal Dead Zone(TDZ)이란 무엇인가요?**<br />
A: let, const, class로 선언된 변수가 호이스팅은 되지만, 초기화 이전에 참조할 수 없는 구간을 말합니다. 이 구간에서 변수에 접근하면 ReferenceError가 발생합니다. 이는 var의 undefined 초기화와는 다른 동작입니다.

**Q: 다음 코드의 실행 결과를 설명해주세요.**<br />

```javascript
console.log(x); // ?
console.log(foo); // ?
let x = 3;
var foo;
```
<br />
A: 첫 번째는 ReferenceError가 발생하고, 두 번째는 undefined가 출력됩니다. let으로 선언된 x는 TDZ에 있어 초기화 이전에 참조할 수 없지만, var로 선언된 foo는 호이스팅되어 undefined로 초기화됩니다.

**Q: const의 특징은 무엇인가요?**<br />
A: 1) 선언과 동시에 초기화 필수, 2) 재할당 금지, 3) 블록 레벨 스코프, 4) 호이스팅되지만 TDZ로 초기화 이전 참조 불가, 5) 객체의 프로퍼티는 변경 가능 (객체 자체는 재할당 불가)

**Q: const로 선언된 객체의 프로퍼티를 변경할 수 있나요?**<br />
A: 네, 가능합니다. const는 변수 자체의 재할당을 금지하는 것이지, 객체의 프로퍼티 변경을 금지하는 것은 아닙니다. 객체의 프로퍼티 추가, 삭제, 변경은 모두 가능합니다.

**Q: 전역 객체와 let/const의 관계는 어떻게 되나요?**<br />
A: var로 선언된 전역 변수는 window 객체의 프로퍼티가 되지만, let/const로 선언된 전역 변수는 전역 객체의 프로퍼티가 되지 않습니다. let/const는 보이지 않는 개념적인 블록 내에 존재합니다.

**Q: ES6에서 변수 선언 시 권장사항은 무엇인가요?**<br />
A: 1) var 키워드는 사용하지 않기, 2) 재할당이 필요 없는 경우 const 사용, 3) 재할당이 필요한 경우 let 사용, 4) 스코프는 최대한 좁게 만들기

**Q: 다음 코드에서 왜 ReferenceError가 발생하나요?**<br />
```javascript
let foo = 1;
{
    console.log(foo); // ReferenceError
    let foo = 2;
}
```
<br />
A: 블록 내부의 let foo 선언이 호이스팅되어 TDZ를 만들기 때문입니다. 만약 호이스팅이 일어나지 않았다면 외부 스코프의 foo(1)를 참조했겠지만, 호이스팅으로 인해 블록 내부의 foo가 선언되어 TDZ에 빠져 ReferenceError가 발생합니다.

**Q: TypeScript의 'as const'는 무엇인가요?**<br />
A: TypeScript에서 객체나 배열을 리터럴 타입으로 변환하는 구문입니다. 모든 프로퍼티가 읽기 전용(readonly)이 되고, 값 자체가 변경될 수 없도록 만듭니다. 코드의 안정성을 높이고 의도치 않은 변경을 방지하는 데 도움이 됩니다.
