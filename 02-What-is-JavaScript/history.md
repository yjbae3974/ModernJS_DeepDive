# 자바스크립트 역사

## 자바스크립트 탄생

-   1995년 넷스케이프에서 브렌던 아이크가 개발
-   `Mocha` -> `LiveScript` -> `JavaScript`
-   1996년 마이크로소프트에서 `JScript`라는 이름으로 자바스크립트 호환 언어 개발
-   1997년 ECMA 국제 표준으로 제정(ECMAScript)

## ECMAScript 버전별 특징

-   **ES1(1997)**: 최초 표준
-   **ES2(1998)**: ISO/IEC 16262 국제 표준 제정
-   **ES3(1999)**: 정규표현식, 예외처리(`try` ... `catch`)등 추가
    -   예시: 정규표현식 `/[a-z]+/`, 예외처리 `try { ... } catch(e) { ... }`
-   **ES4**: 개발 중단
-   **ES5(2009)**: 엄격모드(`'use strict';`), JSON 지원, 배열 메서드 추가
    -   예시: 엄격모드 `'use strict';`, JSON `JSON.parse()`, 배열 메서드 `Array.prototype.forEach()`
-   **ES6(2015)**: let/const, 화살표 함수, 클래스, 템플릿 리터럴, 디스트럭처링, 모듈 등 추가
    -   예시: 변수 선언 `let`, 화살표 함수 `() => {}`, 클래스 `class MyClass {}`, 템플릿 리터럴 `` `Hello, ${name}!` ``, 디스트럭처링 `const {a, b} = obj;`, 모듈 `import { myFunc } from './myModule.js';`
-   **ES7(2016)**: 지수 연산자(`**`), 배열 메서드 `Array.prototype.includes()` 추가
-   **ES8(2017)**: async/await, Object.values(), Object.entries() 추가
-   **ES9(2018)**: Rest/Spread 프로퍼티, Promise.finally() 추가
    -   예시: Rest/Spread 프로퍼티 `const { a, ...rest } = obj;`, Promise.finally() `promise.finally(() => { ... });`
-   **ES10(2019)**: Array.prototype.flat(), Object.fromEntries() 추가
    -   예시: 배열 평탄화 `array.flat()`, 객체 생성 `Object.fromEntries()`
-   **ES11(2020)**: Nullish Coalescing Operator(`??`), Optional Chaining(`?.`) 추가
    -   예시: Nullish Coalescing `const value = a ?? b;`, Optional Chaining `const value = obj?.prop;`
-   **ES12(2021)**: Logical Assignment Operators(`&&=`, `||=`, `??=`), String.prototype.replaceAll() 추가
    -   예시: 논리 할당 연산자 `a &&= b;`, 문자열 치환 `str.replaceAll('old', 'new');`

## 몰랐던 것 📝

-   자바스크립트는 원래 Mocha라는 이름으로 시작했고, LiveScript를 거쳐 JavaScript가 되었다
-   ES4는 개발 중단되었고, ES5에서 많은 기능이 추가되었다
-   ES6(ES2015)가 가장 큰 변화를 가져온 버전으로, 현대적인 자바스크립트의 기반이 되었다

## 기술 질문 대비 🤔

**Q: 자바스크립트는 언제, 누가 만들었나요?**
A: 1995년 넷스케이프의 브렌던 아이크가 개발했습니다. 원래는 Mocha라는 이름으로 시작했고, LiveScript를 거쳐 JavaScript가 되었습니다.

**Q: ECMAScript와 JavaScript의 차이는 무엇인가요?**
A: ECMAScript는 자바스크립트의 표준 사양이고, JavaScript는 ECMAScript를 구현한 언어입니다. 1997년 ECMA 국제 표준으로 제정되었습니다.

**Q: ES6의 주요 특징은 무엇인가요?**
A: let/const, 화살표 함수, 클래스, 템플릿 리터럴, 디스트럭처링, 모듈 등이 추가되어 현대적인 자바스크립트의 기반이 되었습니다.
