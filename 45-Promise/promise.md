## Promise

-   Promise는 비동기 처리를 위한 객체
-   비동기 처리를 위한 객체는 대표적으로 콜백 함수가 있었음
-   비동기 함수가 비동기 처리 결과를 가지고 또 다시 비동기 함수 호출.. 이런 식으로 계속 중첩되는 것을 콜백 지옥이라고 함
-   콜백 함수는 콜백 헬을 발생시키는 주요 원인이 됨
-   이를 해결하기 위해 Promise가 도입됨

## 콜백 지옥(Callback Hell) 예시

### 1. 기본적인 콜백 지옥

```js
// 사용자 정보를 가져오고, 그 다음에 포스트를 가져오고, 그 다음에 댓글을 가져오는 예시
getUser(1, function (user) {
    console.log("User:", user);

    getPosts(user.id, function (posts) {
        console.log("Posts:", posts);

        getComments(posts[0].id, function (comments) {
            console.log("Comments:", comments);

            // 더 깊어질 수 있음...
            getReplies(comments[0].id, function (replies) {
                console.log("Replies:", replies);
            });
        });
    });
});
```

### 2. 실제 API 호출 예시

```js
// 순차적으로 데이터를 가져오는 콜백 지옥
function fetchUserData(userId) {
    // 1단계: 사용자 정보 가져오기
    fetch(
        `/api/users/${userId}`,
        function (user) {
            console.log("User loaded:", user);

            // 2단계: 사용자의 주문 내역 가져오기
            fetch(
                `/api/users/${user.id}/orders`,
                function (orders) {
                    console.log("Orders loaded:", orders);

                    // 3단계: 첫 번째 주문의 상세 정보 가져오기
                    fetch(
                        `/api/orders/${orders[0].id}/details`,
                        function (orderDetails) {
                            console.log("Order details loaded:", orderDetails);

                            // 4단계: 상품 정보 가져오기
                            fetch(
                                `/api/products/${orderDetails.productId}`,
                                function (product) {
                                    console.log("Product loaded:", product);

                                    // 5단계: 리뷰 정보 가져오기
                                    fetch(
                                        `/api/products/${product.id}/reviews`,
                                        function (reviews) {
                                            console.log(
                                                "Reviews loaded:",
                                                reviews
                                            );

                                            // 에러 처리가 각 단계마다 필요
                                            if (reviews.length === 0) {
                                                console.log("No reviews found");
                                            }
                                        },
                                        function (error) {
                                            console.error(
                                                "Error loading reviews:",
                                                error
                                            );
                                        }
                                    );
                                },
                                function (error) {
                                    console.error(
                                        "Error loading product:",
                                        error
                                    );
                                }
                            );
                        },
                        function (error) {
                            console.error(
                                "Error loading order details:",
                                error
                            );
                        }
                    );
                },
                function (error) {
                    console.error("Error loading orders:", error);
                }
            );
        },
        function (error) {
            console.error("Error loading user:", error);
        }
    );
}
```

### 3. 타이머를 사용한 콜백 지옥

```js
// 여러 단계의 비동기 작업을 순차적으로 실행
function complexAsyncWork() {
    setTimeout(function () {
        console.log("Step 1 completed");

        setTimeout(function () {
            console.log("Step 2 completed");

            setTimeout(function () {
                console.log("Step 3 completed");

                setTimeout(function () {
                    console.log("Step 4 completed");

                    setTimeout(function () {
                        console.log("Step 5 completed");
                        console.log("All steps completed!");
                    }, 1000);
                }, 1000);
            }, 1000);
        }, 1000);
    }, 1000);
}
```

### 4. 파일 시스템 작업 예시

```js
// Node.js 파일 시스템 작업의 콜백 지옥
const fs = require("fs");

function processFiles() {
    // 1단계: 디렉토리 읽기
    fs.readdir("./data", function (err, files) {
        if (err) {
            console.error("Error reading directory:", err);
            return;
        }

        // 2단계: 첫 번째 파일 읽기
        fs.readFile(`./data/${files[0]}`, "utf8", function (err, data) {
            if (err) {
                console.error("Error reading file:", err);
                return;
            }

            // 3단계: 데이터 처리 후 새 파일 생성
            const processedData = data.toUpperCase();
            fs.writeFile(
                "./output/processed.txt",
                processedData,
                function (err) {
                    if (err) {
                        console.error("Error writing file:", err);
                        return;
                    }

                    // 4단계: 백업 파일 생성
                    fs.copyFile(
                        "./output/processed.txt",
                        "./backup/processed.txt",
                        function (err) {
                            if (err) {
                                console.error("Error creating backup:", err);
                                return;
                            }

                            console.log("All file operations completed!");
                        }
                    );
                }
            );
        });
    });
}
```

### 콜백 지옥의 문제점

1. **가독성 저하**: 코드가 오른쪽으로 계속 들여쓰기됨
2. **에러 처리 복잡**: 각 단계마다 에러 처리 필요
3. **유지보수 어려움**: 중간에 로직 추가/수정이 어려움
4. **디버깅 어려움**: 어느 단계에서 문제가 발생했는지 파악 어려움
5. **코드 재사용성 저하**: 특정 단계만 재사용하기 어려움

```js
const promise = new Promise((resolve, reject) => {
    if (success) {
        resolve(result);
    } else {
        reject(error);
    }
});
```

-   Promise 객체의 내부
    -   `[[PromiseState]]` : pending, fulfilled, rejected
    -   `[[PromiseResult]]` : 비동기 처리 결과. resolve 또는 reject 호출 시 결과 값이 저장됨

## Promise 상태

1. pending: 비동기 처리 중
2. fulfilled: 비동기 처리 성공. resolve 호출 시 fulfilled 상태가 됨
3. rejected: 비동기 처리 실패. reject 호출 시 rejected 상태가 됨

## Promise 후속 처리 메서드

-   then, catch, finally
-   then: 2개의 콜백 함수를 인자로 받는다.
    -   첫 번째 콜백 함수: 비동기 처리 성공 시 호출
    -   두 번째 콜백 함수: 비동기 처리 실패 시 호출
-   catch: 콜백 함수를 인자로 받는다.
    -   비동기 처리 실패 시 호출(then의 두 번째 콜백과 동일)
-   finally: 콜백 함수를 인자로 받는다.
    -   프로미스 상태의 성공, 실패에 관계 없이 호출.

## Promise 체이닝

-   Promise 객체의 then 메서드를 호출하면, 새로운 Promise 객체를 반환한다.

## Promise.all()

-   여러 개의 Promise 객체를 인자로 받아, 모든 Promise 객체가 성공 시 하나의 Promise 객체를 반환한다.
-   모든 인자가 Promise 객체.
-   이때 반환되는 객체는 배열 형태로 모든 인자가 성공 시 반환된 값들을 배열 형태로 반환한다.

```js
// 두 개의 비동기 작업(Promise)을 정의합니다.
const fetchUser = fetch("https://api.example.com/user/1").then((res) =>
    res.json()
);
const fetchPosts = fetch("https://api.example.com/posts").then((res) =>
    res.json()
);

// Promise.all()을 이용해 두 Promise를 동시에 실행합니다.
Promise.all([fetchUser, fetchPosts])
    .then(([userData, postsData]) => {
        // 두 요청이 모두 성공적으로 완료되면 실행됩니다.
        console.log("사용자 데이터:", userData);
        console.log("게시물 데이터:", postsData);
    })
    .catch((error) => {
        // 두 요청 중 하나라도 실패하면 실행됩니다.
        console.error("요청 중 하나가 실패했습니다:", error);
    });
```

## Promise.race()

-   여러 개의 Promise 객체를 인자로 받아, 가장 먼저 완료된 Promise 객체의 결과를 반환한다.

## Promise.allSettled()

-   여러 개의 Promise 객체를 인자로 받아, 모든 Promise 객체가 완료된 후 하나의 Promise 객체를 반환한다.
-   모든 인자가 Promise 객체.
-   이때 반환되는 객체는 배열 형태로 모든 인자가 완료된 후 반환된 값들을 배열 형태로 반환한다.

## Promise.any()

-   여러 개의 Promise 객체를 인자로 받아, 가장 먼저 완료된 Promise 객체의 결과를 반환한다.
-   모든 인자가 Promise 객체.
-   이때 반환되는 객체는 배열 형태로 가장 먼저 완료된 인자의 결과를 반환한다.

## 핵심 개념 요약

### Promise란?

-   **비동기 처리를 위한 객체**
-   **콜백 지옥을 해결하기 위해 도입된 패턴**
-   **3가지 상태**: pending(대기), fulfilled(성공), rejected(실패)

### Promise의 핵심 특징

1. **상태 불변성**: 한 번 settled(fulfilled/rejected)되면 상태 변경 불가
2. **체이닝**: then 메서드가 새로운 Promise 반환하여 연속 호출 가능
3. **에러 전파**: catch로 에러를 한 곳에서 처리 가능
4. **비동기 제어**: 비동기 작업의 순서와 결과를 명확하게 관리

### Promise vs 콜백

-   **콜백**: 중첩 구조, 에러 처리 복잡, 가독성 저하
-   **Promise**: 체이닝 구조, 에러 처리 단순, 가독성 향상

## 면접 예상 질문

### 1. Promise 기본 개념

**Q: Promise란 무엇인가요?**

-   비동기 처리를 위한 객체
-   콜백 지옥을 해결하기 위해 도입
-   3가지 상태(pending, fulfilled, rejected)를 가짐

**Q: Promise의 3가지 상태를 설명해주세요.**

-   pending: 비동기 처리 중
-   fulfilled: 비동기 처리 성공 (resolve 호출 시)
-   rejected: 비동기 처리 실패 (reject 호출 시)

### 2. Promise 패턴 활용 예시와 동작원리

**Q: Promise 패턴을 활용한 예시를 들어보고 동작원리를 설명해보세요.**

```js
// 실제 사용 예시: 사용자 데이터와 포스트를 순차적으로 가져오기
function fetchUserAndPosts(userId) {
    // 1단계: 사용자 정보 가져오기
    return fetch(`/api/users/${userId}`)
        .then((response) => {
            if (!response.ok) {
                throw new Error("사용자 정보를 가져올 수 없습니다");
            }
            return response.json();
        })
        .then((user) => {
            console.log("사용자 정보:", user);

            // 2단계: 사용자의 포스트 가져오기
            return fetch(`/api/users/${user.id}/posts`);
        })
        .then((response) => {
            if (!response.ok) {
                throw new Error("포스트를 가져올 수 없습니다");
            }
            return response.json();
        })
        .then((posts) => {
            console.log("포스트 목록:", posts);
            return { user: user, posts: posts };
        })
        .catch((error) => {
            console.error("에러 발생:", error);
            throw error;
        });
}

// 사용법
fetchUserAndPosts(1)
    .then((data) => {
        console.log("최종 결과:", data);
    })
    .catch((error) => {
        console.error("처리 실패:", error);
    });
```

**동작원리:**

1. **Promise 생성**: fetch()가 Promise 객체 반환
2. **체이닝**: 각 then()이 새로운 Promise 반환
3. **상태 전파**: 이전 Promise의 결과가 다음 then()으로 전달
4. **에러 처리**: catch()에서 모든 에러를 한 번에 처리
5. **비동기 제어**: 순차적 실행 보장

### 3. Promise 메서드 활용

**Q: Promise.all()과 Promise.race()의 차이점은?**

-   **Promise.all()**: 모든 Promise가 성공해야 성공, 하나라도 실패하면 실패
-   **Promise.race()**: 가장 먼저 완료된 Promise의 결과 반환

**Q: 다음 코드의 실행 결과는?**

```js
const promise1 = new Promise((resolve) =>
    setTimeout(() => resolve("첫 번째"), 1000)
);
const promise2 = new Promise((resolve) =>
    setTimeout(() => resolve("두 번째"), 500)
);
const promise3 = new Promise((resolve) =>
    setTimeout(() => resolve("세 번째"), 1500)
);

Promise.race([promise1, promise2, promise3]).then((result) =>
    console.log(result)
); // ?
```

### 4. Promise 에러 처리

**Q: Promise에서 에러를 처리하는 방법은?**

-   **then()의 두 번째 인자**: 해당 then()에서만 에러 처리
-   **catch()**: 체이닝된 모든 Promise의 에러 처리
-   **finally()**: 성공/실패 관계없이 항상 실행

**Q: 다음 코드에서 에러가 어떻게 전파되나요?**

```js
Promise.resolve()
    .then(() => {
        throw new Error("첫 번째 에러");
    })
    .then(() => {
        console.log("실행되지 않음");
    })
    .catch((error) => {
        console.log("에러 캐치:", error.message);
    });
```

### 5. Promise vs async/await

**Q: Promise와 async/await의 차이점은?**

-   **Promise**: 체이닝 방식, 함수형 프로그래밍 스타일
-   **async/await**: 동기 코드처럼 작성, 더 직관적

**Q: Promise를 async/await로 변환해보세요.**

```js
// Promise 방식
function fetchData() {
    return fetch("/api/data")
        .then((response) => response.json())
        .then((data) => {
            console.log(data);
            return data;
        })
        .catch((error) => {
            console.error(error);
            throw error;
        });
}

// async/await 방식
async function fetchData() {
    try {
        const response = await fetch("/api/data");
        const data = await response.json();
        console.log(data);
        return data;
    } catch (error) {
        console.error(error);
        throw error;
    }
}
```

### 6. Promise 실무 활용

**Q: 실제 프로젝트에서 Promise를 어떻게 활용하나요?**

-   **API 호출**: fetch()와 함께 사용
-   **파일 처리**: Node.js의 fs.promises
-   **데이터베이스**: ORM의 Promise 기반 메서드
-   **이미지 로딩**: Image 객체의 load 이벤트

**Q: Promise.all()을 사용한 병렬 처리 예시를 보여주세요.**

```js
async function loadUserDashboard(userId) {
    try {
        const [user, posts, comments] = await Promise.all([
            fetch(`/api/users/${userId}`).then((res) => res.json()),
            fetch(`/api/users/${userId}/posts`).then((res) => res.json()),
            fetch(`/api/users/${userId}/comments`).then((res) => res.json()),
        ]);

        return { user, posts, comments };
    } catch (error) {
        console.error("대시보드 로딩 실패:", error);
        throw error;
    }
}
```
