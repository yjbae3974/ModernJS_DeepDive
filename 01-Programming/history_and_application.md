## 컴퓨팅 사고(Computational Thinking)
- 사람의 일반적인 사고 방식은 매우 포괄적이며 실생활에서 경험하는 사항에 대해 당연시한다.
- 컴퓨터 관점에서 문제를 사고
  - 논리적, 수학적.
  - 해결 과제를 작은 단위로 분해하고 패턴화, 추출
  - ex) 걷다 라는 기능을 디자인
    - 판단해야하는 상태는 무엇인지.
    - 그 상태를 판단하는 시기는 언제인지
    - 판단 기준은 무엇인지.
### 프로그래밍의 목적은 문제 해결이다.
- 문법(syntax)이 중요한 것이 아니라, 문제를 해결하는 방법, 그리고 의미(semantic)이 중요하다.
- 프로그래밍 언어는 문제 해결을 위한 도구일 뿐이다.

### 자바스크립트
- 1995년 넷스케이프에서 브렌던 아이크가 개발
- `Mocha` -> `LiveScript` -> `JavaScript`
- 1996년 마이크로소프트에서 `JScript`라는 이름으로 자바스크립트 호환 언어 개발
- 1997년 ECMA 국제 표준으로 제정(ECMAScript)

### ECMAScript 버전별 특징
- ES1(1997) : 최초 표준
- ES2(1998) : ISO/IEC 16262 국제 표준 제정
- ES3(1999) : 정규표현식, 예외처리(`try` ... `catch`)등 추가
  - 예시: 정규표현식 `/[a-z]+/`, 예외처리 `try { ... } catch(e) { ... }`
- ES4 : 개발 중단
- ES5(2009) : 엄격모드(`'use strict';`), JSON 지원, 배열 메서드 추가
  - 예시: 엄격모드 `'use strict';`, JSON `JSON.parse()`, 배열 메서드 `Array.prototype.forEach()`
- ES6(2015) : let/const, 화살표 함수, 클래스, 템플릿 리터럴, 디스트럭처링, 모듈 등 추가
  - 예시: 변수 선언 `let`, 화살표 함수 `() => {}`, 클래스 `class MyClass {}`, 템플릿 리터럴 `` `Hello, ${name}!` ``, 디스트럭처링 `const {a, b} = obj;`, 모듈 `import { myFunc } from './myModule.js';`
- ES7(2016) : 지수 연산자(`**`), 배열 메서드 `Array.prototype.includes()` 추가
- ES8(2017) : async/await, Object.values(), Object.entries() 추가
- ES9(2018) : Rest/Spread 프로퍼티, Promise.finally() 추가
  - 예시: Rest/Spread 프로퍼티 `const { a, ...rest } = obj;`, Promise.finally() `promise.finally(() => { ... });`
- ES10(2019) : Array.prototype.flat(), Object.fromEntries() 추가
  - 예시: 배열 평탄화 `array.flat()`, 객체 생성 `Object.fromEntries()`
- ES11(2020) : Nullish Coalescing Operator(`??`), Optional Chaining(`?.`) 추가
  - 예시: Nullish Coalescing `const value = a ?? b;`, Optional Chaining `const value = obj?.prop;`
- ES12(2021) : Logical Assignment Operators(`&&=`, `||=`, `??=`), String.prototype.replaceAll() 추가
  - 예시: 논리 할당 연산자 `a &&= b;`, 문자열 치환 `str.replaceAll('old', 'new');`

### Ajax
- Asynchronous JavaScript and XML
- 자바스크립트를 이용하여 `비동기적`으로 서버와 통신하는 기술
- 이전에는, 서버와 통신하려면 서버로부터 새로운 HTML을 전송받아 페이지 전체를 새로고침해야 했음
  - 기존에는 웹페이지의 어쩔 수 없는 한계였다.
- Ajax를 이용하면, 페이지의 일부만 갱신할 수 있어 사용자 경험이 향상됨

### jQuery
- 2006년 존 레식이 개발한 자바스크립트 라이브러리
- DOM 조작, 이벤트 처리, Ajax 등을 쉽게 할 수 있도록 도와줌

### V8 엔진
- 구글에서 개발한 자바스크립트 엔진
- 이로 인해 데스크톱 애플리케이션과 유사한 경험 제공 가능.
- 과거 웹서버에서 주로 수행되던 로직이 대거 클라이언트로 이동.
- 프론트엔드 영역이 주목받는 계기
- `크롬 브라우저`와 `Node.js`에서 사용

### Node.js
- 2009년 라이언 달이 개발한 자바스크립트 런타임
- V8 엔진을 사용하여 브라우저 이외의 환경에서 자바스크립트를 실행할 수 있도록 함
- 서버 사이드 애플리케이션 개발에 주로 사용.
- 비동기 I/O 지원.
- 단일 스레드 이벤트 루프 기반으로 동작
  - 요청 처리 성능이 좋음.

### SPA
- Single Page Application
- 모던 웹 애플리케이션이 데스크톱 애플리케이션과 비교해도 손색없는 성능을 보여주는 것이 디폴트가 됨.
- 이전 개발방식으로는 복잡해진 개발 과정을 수행하기 어려웠음.
- `CBD`(Component Based Development) 방식이 대세가 됨.
- `React`, `Vue`, `Angular` 등 프레임워크/라이브러리가 주목받음.