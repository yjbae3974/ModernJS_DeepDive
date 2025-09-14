## generator
- 이터레이터를 반환하는 함수
- 탄생 배경: 비동기 처리를 위한 콜백 패턴의 단점 보완
- 또한, 이는 async/await의 탄생 배경이 되었다.

## async/await
- async 함수는 generator 함수를 포함한다.
- await 키워드는 generator 함수의 next 메소드를 호출하는 표현식이다.
- await 키워드를 사용하면, async 함수는 비동기 처리를 동기적으로 작성할 수 있다.
- async 함수는 promise 객체를 반환한다.
```js
function* gen() {
    yield 1;
    yield 2;
    yield 3;
}
const g = gen();
console.log(g.next()); // { value: 1, done: false }
console.log(g.next()); // { value: 2, done: false }
console.log(g.next()); // { value: 3, done: false }
console.log(g.next()); // { value: undefined, done: true }
```

## async/await 예시
```js
async function fetchData() {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    return data;
}

fetchData().then((data) => { // 반환값은 promise형태
    console.log(data);
});

//이런 단순한 함수도 promise임.
async function foo(n){return n;}
foo(1).then(console.log); // 1
```

### await 키워드
- await는 promise가 settled가 될 때까지 대기하고, settled된 값을 반환한다.
- 이후 resolve된 값을 반환한다.
## 에러 처리
- try/catch 문을 사용하여 에러를 처리할 수 있다.
```js
async function fetchData() {
    try {
        const response = await fetch('https://api.example.com/data');
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error(error);
    }
}
fetchData()
```