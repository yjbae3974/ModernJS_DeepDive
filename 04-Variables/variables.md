### 메모리

-   데이터를 저장할 수 있는 메모리 셀의 집합체.
    -   메모리셀: 데이터를 저장하는 공간.
    -   메모리셀 하나의 크기: 1바이트(8비트).
    -   각 셀은 고유의 메모리 주소를 가진다.
    -   메모리에 저장되는 데이터는 그 종류에 상관없이 2진수로 저장된다.
-   자바스크립트는 개발자의 직접적인 메모리 관리를 허용하지 않는다.
    -   대신, 변수를 통해 메모리를 간접적으로 관리한다.
-   변수: 메모리 셀에 저장된 데이터를 가리키는 이름.
-   변수에 값을 저장: `할당`(assignment).
-   변수에 저장된 값을 읽어오는 것: `참조`(reference).
-   변수 이름은 `식별자`라고 한다.
    -   식별자 규칙:
        -   문자, 숫자, 언더스코어(\_), 달러 기호($)를 사용할 수 있다.
        -   숫자로 시작할 수 없다.
        -   공백을 포함할 수 없다.
        -   대소문자를 구분한다.
        -   **자바스크립트 예약어(키워드)는 사용할 수 없다.**

### undefined?

-   변수를 선언만 하고 값을 할당하지 않으면, 해당 변수의 값은 `undefined`가 된다.
-   메모리는 확보된 상태

### 실행 컨텍스트

-   자바스크립트 엔진이 평가하고 실행하기 위해 필요한 환경 제공, 코드의 실행 결과 저장.
-   이를 통해 식별자와 스코프 관리.

    -   스코프란?
        -   식별자가 유효한 범위.
        -   전역 스코프: 어디서든 접근 가능.
        -   지역 스코프: 특정 블록 내에서만 접근 가능.
    -   스코프 vs closure

        -   **스코프**: 식별자가 유효한 범위.
            -   자바스크립트 엔진이 식별자를 검색할 때 사용하는 규칙
            -   스코프 체인을 통해 상위 스코프로 올라가며 식별자를 검색
            -   렉시컬 스코프(정적 스코프): 함수가 정의된 위치에 따라 스코프가 결정됨
        -   **클로저**: 함수와 그 함수가 선언된 렉시컬 환경의 조합.
            -   외부 함수가 반환된 후에도 내부 함수가 외부 함수의 변수에 접근할 수 있는 현상
            -   데이터 은닉과 캡슐화를 구현할 수 있음
            -   메모리 누수 주의 필요
        -   **구체적인 예시**:
        -   ```javascript
            // 1. 기본적인 클로저 예시
            function outer() {
                let outerVar = "I am outside!";

                function inner() {
                    console.log(outerVar); // 클로저를 통해 outerVar에 접근 가능
                }

                return inner; // 함수를 반환
            }

            const myFunc = outer();
            myFunc(); // "I am outside!" 출력

            // 2. 클로저를 이용한 카운터 구현
            function createCounter() {
                let count = 0; // private 변수

                return {
                    increment: function () {
                        count++;
                        return count;
                    },
                    decrement: function () {
                        count--;
                        return count;
                    },
                    getCount: function () {
                        return count;
                    },
                };
            }

            const counter = createCounter();
            console.log(counter.increment()); // 1
            console.log(counter.increment()); // 2
            console.log(counter.getCount()); // 2
            // console.log(counter.count);    // undefined (접근 불가)

            // 3. 스코프 체인 예시
            const globalVar = "global";

            function outerFunction() {
                const outerVar = "outer";

                function innerFunction() {
                    const innerVar = "inner";
                    console.log(innerVar); // 'inner' - 현재 스코프
                    console.log(outerVar); // 'outer' - 상위 스코프
                    console.log(globalVar); // 'global' - 전역 스코프
                }

                innerFunction();
            }

            outerFunction();
            ```

### 호이스팅

-   변수 선언과 함수 선언이 해당 스코프의 최상단으로 끌어올려지는 현상.
-   자바스크립트는 인터프리터에 의해 한 줄씩 실행되지만, 변수 선언은 실행 전에 먼저 처리된다.
-   예시
-   ```javascript
    console.log(x); // undefined (선언은 호이스팅되었지만, 할당은 아직 안됨)
    var x = 5;
    console.log(x); // 5

    // 함수 선언의 호이스팅
    greet(); // "Hello!"
    function greet() {
        console.log("Hello!");
    }
    ```

-   `var` 키워드로 선언된 변수는 호이스팅되지만, `let`과 `const`로 선언된 변수는 호이스팅되지 않는다.

### 값 할당

-   변수의 선언은 런타임 이전에 처리.
-   값의 할당은 런타임에 이루어짐.

```js
var a = 50; // 한 줄이지만, 두 단계로 나뉨:
// 1. 변수 선언: var a; (호이스팅)
// 2. 값 할당: a = 50; (런타임에 실행)
```

### 네이밍 컨벤션

-   변수나 함수: camelCase (예: myVariable, calculateSum)
-   생성자 함수: PascalCase (예: MyClass, UserModel)

    -   생성자 함수란?
    -   `new` 키워드와 함께 호출되어 객체를 생성하는 함수.
    -   예시
    -   ```javascript
        function Person(name, age) {
            this.name = name;
            this.age = age;
        }

        const john = new Person("John", 30);
        console.log(john.name); // "John"
        ```

-   상수: UPPER_SNAKE_CASE (예: MAX_VALUE, API_KEY)

## 몰랐던 것 📝

-   메모리 셀 하나의 크기가 1바이트(8비트)라는 것
-   자바스크립트는 직접적인 메모리 관리를 허용하지 않고 변수를 통해 간접적으로 관리한다
-   변수 선언과 값 할당이 런타임 이전과 런타임에 각각 처리된다
-   호이스팅은 `var`에만 적용되고 `let`, `const`에는 적용되지 않는다
-   클로저를 통해 private 변수를 구현할 수 있다

## 기술 질문 대비 🤔

**Q: 자바스크립트에서 메모리는 어떻게 관리되나요?**<br />
A: 자바스크립트는 개발자의 직접적인 메모리 관리를 허용하지 않습니다. 대신 변수를 통해 메모리를 간접적으로 관리하며, 가비지 컬렉터가 자동으로 메모리를 정리합니다.

**Q: 변수 선언과 값 할당의 차이점은 무엇인가요?**<br />
A: 변수 선언은 런타임 이전에 처리되고, 값 할당은 런타임에 이루어집니다. `var a = 50;`은 실제로는 `var a;`(선언)와 `a = 50;`(할당) 두 단계로 나뉩니다.

**Q: 호이스팅이란 무엇이고, 어떤 변수에 적용되나요?**<br />
A: 호이스팅은 변수 선언과 함수 선언이 해당 스코프의 최상단으로 끌어올려지는 현상입니다. `var` 키워드로 선언된 변수와 함수 선언에만 적용되며, `let`과 `const`로 선언된 변수는 호이스팅되지 않습니다.

**Q: 스코프와 클로저의 차이점은 무엇인가요?**<br />
A: 스코프는 식별자가 유효한 범위를 의미하며, 클로저는 함수와 그 함수가 선언된 렉시컬 환경의 조합입니다. 클로저를 통해 외부 함수가 반환된 후에도 내부 함수가 외부 함수의 변수에 접근할 수 있습니다.

**Q: 클로저의 실용적인 활용 예시를 들어주세요.**<br />
A: 클로저를 이용해 private 변수를 구현할 수 있습니다. 예를 들어, 카운터 함수에서 `count` 변수를 외부에서 직접 접근할 수 없게 하고, `increment()`, `decrement()`, `getCount()` 메서드를 통해서만 조작할 수 있게 할 수 있습니다.

**Q: 자바스크립트의 네이밍 컨벤션은 어떻게 되나요?**<br />
A: 변수나 함수는 camelCase, 생성자 함수는 PascalCase, 상수는 UPPER_SNAKE_CASE를 사용합니다. 예를 들어 `myVariable`, `MyClass`, `MAX_VALUE`와 같이 작성합니다.

**Q: undefined와 null의 차이점은 무엇인가요?**<br />
A: `undefined`는 변수를 선언만 하고 값을 할당하지 않았을 때의 값이며, `null`은 의도적으로 빈 값을 할당한 것입니다. `undefined`는 자동으로 할당되고, `null`은 개발자가 명시적으로 할당합니다.
