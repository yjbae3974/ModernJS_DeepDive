# ECMAScript와 Web API

## ECMAScript란?

-   JS의 표준 사양.
-   프로그래밍 언어의 값, 타입, 객체와 프로퍼티 함수, 표준 빌트인 객체 등 핵심 문법 규정.
    -   `표준 빌트인 객체`: Array, Date, Math, String 등.
-   각 브라우저 제조사는 이를 준수하여 자바스크립트 엔진 구현

## JS: ECMAScript + Web API

-   ECMAScript: 자바스크립트의 핵심 문법과 기능을 정의
-   Web API: 브라우저 환경(클라이언트 사이드)에서 제공하는 다양한 기능과 인터페이스
    -   `DOM`, `BOM`, `XMLHttpRequest`, `Fetch API`, `WebWorker` 등.
    -   `BOM`: Browser Object Model (브라우저 창과 관련된 객체 모델)
    -   `XMLHttpRequest`: 서버와 비동기 통신을 가능하게 하는 API
    -   `Fetch API`: 네트워크 요청을 처리하기 위한 현대적인 인터페이스
    -   `WebWorker`: 백그라운드 스레드에서 스크립트를 실행할 수 있게 하는 API
-   Web API는 `W3C`(World Wide Web Consortium)와 WHATWG(Web Hypertext Application Technology Working Group)에서 표준화 작업 진행

## ES6 브라우저 지원 현황

-   대부분의 최신 브라우저에서 ES6 기능 지원
-   구형 브라우저(예: IE11)에서는 일부 기능 미지원
-   Babel과 같은 트랜스파일러를 사용하여 ES6 코드를 구형 브라우저에서도 호환 가능하게 변환

## 몰랐던 것 📝

-   자바스크립트는 ECMAScript(핵심 문법) + Web API(브라우저 기능)로 구성된다
-   Web API는 W3C와 WHATWG에서 표준화 작업을 진행한다
-   Babel 같은 트랜스파일러를 통해 구형 브라우저 호환성을 확보할 수 있다

## 기술 질문 대비 🤔

**Q: ECMAScript와 JavaScript의 차이는 무엇인가요?**
A: ECMAScript는 자바스크립트의 표준 사양으로, 핵심 문법과 기능을 정의합니다. JavaScript는 ECMAScript + Web API로 구성되며, Web API는 브라우저에서 제공하는 추가 기능들입니다.

**Q: Web API에는 어떤 것들이 있나요?**
A: DOM, BOM, XMLHttpRequest, Fetch API, WebWorker 등이 있습니다. 이들은 브라우저 환경에서만 제공되며, W3C와 WHATWG에서 표준화 작업을 진행합니다.

**Q: ES6 브라우저 호환성 문제는 어떻게 해결하나요?**
A: Babel과 같은 트랜스파일러를 사용하여 ES6 코드를 구형 브라우저에서도 호환 가능한 코드로 변환할 수 있습니다.
