## Ajax란?

-   Asynchronous JavaScript and XML의 줄임말로, 자바스크립트를 이용해 비동기적으로 서버와 통신하는 기술
-   이전 웹페이지는, 서버로부터 완전한 HTML문서를 전송받아 웹페이지 전체를 처음부터 다시 렌더링하는 방식으로 동작. 이는 변경할 필요가 없는 부분까지 처음부터 다시 렌더링하는 문제가 있었다.

### Ajax등장 이후

-   변경할 부분을 갱신하는데 필요한 데이터만 서버로부터 받음.
-   서버랑 클라 통신이 비동기적으로 동작. 서버에게 요청을 보낸 이후 블로킹이 발생하지 않는다.

## JSON

-   클라와 서버 간 HTTP 통신을 위한 텍스트 데이터 포맷
-   JS에 종속되지 않는 포맷으로 대부분의 프로그래밍 언어에서 사용할 수 있다.

### JSON.stringify()

-   객체, 혹은 배열을 JSON 문자열로 변환
-   직렬화: 클라에서 서버로 객체를 전송하기 전 객체를 문자열로 변환하는 작업

### JSON.parse()

-   JSON 문자열을 객체로 변환
-   역직렬화: 서버에서 클라로 전송된 문자열을 객체로 변환하는 작업

## XMLHttpRequest

-   Ajax를 구현하기 위한 메서드
-   비동기적으로 서버와 통신하기 위한 메서드
-   HTTP 응답을 처리하는 경우 다음 순서를 따른다.
    1. `readystatechange` 이벤트 핸들러에서 `readyState` 프로퍼티 확인
    2. `readyState` 프로퍼티 값이 4(DONE)인 경우 `status` 프로퍼티 확인
    3. `status` 프로퍼티 값이 200(OK)인 경우 `responseText` 프로퍼티 확인
    4. `responseText` 프로퍼티 값을 JSON.parse 메소드로 파싱
    5. 파싱된 객체를 사용

## Ajax와 Axios의 관계

### Ajax란?

-   **기술의 개념**: 비동기적으로 서버와 통신하는 기술 자체
-   **구현 방법**: XMLHttpRequest, fetch API, Axios 등 다양한 방법으로 구현 가능

### Axios란?

-   **라이브러리**: Ajax 통신을 더 쉽게 하기 위한 JavaScript 라이브러리
-   **기반**: 내부적으로 XMLHttpRequest를 사용하여 구현
-   **Promise 기반**: Promise를 반환하여 비동기 처리를 더 편리하게 함

### 관계도

```
Ajax (기술 개념)
├── XMLHttpRequest (네이티브 API)
├── fetch API (네이티브 API)
└── Axios (라이브러리) ← XMLHttpRequest 기반
```

### 비교 예시

#### XMLHttpRequest (네이티브)

```js
const xhr = new XMLHttpRequest();
xhr.open("GET", "/api/users");
xhr.onreadystatechange = function () {
    if (xhr.readyState === 4 && xhr.status === 200) {
        const data = JSON.parse(xhr.responseText);
        console.log(data);
    }
};
xhr.send();
```

#### Axios (라이브러리)

```js
axios
    .get("/api/users")
    .then((response) => {
        console.log(response.data);
    })
    .catch((error) => {
        console.error(error);
    });
```

### Axios의 장점

1. **간단한 문법**: XMLHttpRequest보다 훨씬 간단
2. **Promise 기반**: async/await와 함께 사용 가능
3. **자동 JSON 파싱**: response.data로 바로 접근
4. **요청/응답 인터셉터**: 전역 설정 가능
5. **에러 처리**: 자동 에러 처리
6. **요청 취소**: AbortController 지원

## fetch API

### fetch란?

-   **네이티브 API**: 브라우저에 내장된 비동기 통신 API
-   **Promise 기반**: Promise를 반환하여 비동기 처리
-   **모든 HTTP 메서드 지원**: GET, POST, PUT, DELETE, PATCH 등

### fetch의 HTTP 메서드들

#### GET 요청

```js
fetch("/api/users")
    .then((response) => response.json())
    .then((data) => console.log(data));
```

#### POST 요청

```js
fetch("/api/users", {
    method: "POST",
    headers: {
        "Content-Type": "application/json",
    },
    body: JSON.stringify({
        name: "John",
        email: "john@example.com",
    }),
})
    .then((response) => response.json())
    .then((data) => console.log(data));
```

#### PUT 요청

```js
fetch("/api/users/1", {
    method: "PUT",
    headers: {
        "Content-Type": "application/json",
    },
    body: JSON.stringify({
        name: "John Updated",
        email: "john.updated@example.com",
    }),
})
    .then((response) => response.json())
    .then((data) => console.log(data));
```

#### DELETE 요청

```js
fetch("/api/users/1", {
    method: "DELETE",
}).then((response) => {
    if (response.ok) {
        console.log("삭제 완료");
    }
});
```

#### PATCH 요청

```js
fetch("/api/users/1", {
    method: "PATCH",
    headers: {
        "Content-Type": "application/json",
    },
    body: JSON.stringify({
        name: "John Patched",
    }),
})
    .then((response) => response.json())
    .then((data) => console.log(data));
```

### fetch vs XMLHttpRequest vs Axios

| 특징                   | fetch                | XMLHttpRequest   | Axios            |
| ---------------------- | -------------------- | ---------------- | ---------------- |
| **HTTP 메서드**        | 모든 메서드 지원     | 모든 메서드 지원 | 모든 메서드 지원 |
| **Promise 기반**       | ✅                   | ❌               | ✅               |
| **JSON 자동 파싱**     | ❌ (수동 필요)       | ❌ (수동 필요)   | ✅               |
| **요청/응답 인터셉터** | ❌                   | ❌               | ✅               |
| **요청 취소**          | ✅ (AbortController) | ✅               | ✅               |
| **브라우저 지원**      | 모던 브라우저        | 모든 브라우저    | 모든 브라우저    |

### fetch의 장단점

#### 장점

-   **네이티브 API**: 별도 라이브러리 불필요
-   **Promise 기반**: async/await와 함께 사용 가능
-   **모든 HTTP 메서드 지원**: RESTful API 완벽 지원
-   **요청 취소 지원**: AbortController 사용

#### 단점

-   **JSON 수동 파싱**: response.json() 호출 필요
-   **에러 처리**: 404, 500 등도 성공으로 처리
-   **요청/응답 인터셉터 없음**: 전역 설정 어려움
-   **자동 재시도 없음**: 수동으로 구현해야 함

### fetch 에러 처리

```js
fetch("/api/users")
    .then((response) => {
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        return response.json();
    })
    .then((data) => console.log(data))
    .catch((error) => console.error("Error:", error));
```

### 결론

-   **Ajax**: 비동기 통신 기술의 개념
-   **XMLHttpRequest**: Ajax를 구현하는 네이티브 API (콜백 기반)
-   **fetch**: Ajax를 구현하는 네이티브 API (Promise 기반)
-   **Axios**: Ajax를 구현하는 라이브러리 (Promise 기반, 편의 기능 제공)
-   **관계**: 모두 Ajax를 구현하는 다양한 방법들
