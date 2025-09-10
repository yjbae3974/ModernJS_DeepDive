## strict mode
- ES5부터 추가된, 더 엄격한 문법과 오류 처리를 제공하는 모드
- 단, ESlint와 같은 도구를 사용해도 유사한 효과를 얻을 수 있음.

## strict mode 활성화, 바람직한 사용
- 함수나 스크립트 최상단에 `"use strict";`를 선언하여 활성화
- 전역 스코프에서 활성화하는 것보다, 함수 단위로 활성화하는 것이 바람직
- 따라서, 스크립트를 즉시 함수로 묶어서 사용하여, 전역 오염을 방지하는 것이 좋음
```js
(function() {
    "use strict";
    // ...
}());
```
- ES6 모듈은 기본적으로 strict mode가 적용되어 있음

## 에러 사례
- 암묵적 전역 변수 생성 금지
```js
function foo() {
    "use strict";
    x = 10; // ReferenceError: x is not defined
}
foo();
```
- 읽기 전용 프로퍼티에 값을 할당할 수 없음
```js
const obj = {};
Object.defineProperty(obj, "x", { value: 0, writable: false });
obj.x = 10; // TypeError: Cannot assign to read only property 'x' of object '#<Object>'
```
