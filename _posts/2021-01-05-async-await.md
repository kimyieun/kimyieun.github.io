# async/await

- generator를 사용하여 비동기 처리를 동기 처리처럼 동작하도록 구현했지만 코드가 무척 장황하고 가독성이 떨어진다.
- ES8에서는 비동기 처리를 동기 처리처럼 동작하도록 구현하는 async/await를 도입하였다.
  - async/await 는 promise 를 기반으로 동작한다. 프로미스의 후속 처리 메소드에 콜백 함수를 전달해 비동기 처리 결과를 후속 처리할 필요 없이 마치 동기 처리처럼 프로미스를 사용할 수 있다.
  
```javascript
const fetch = require('node-fetch');
async function fetchTodo(){
    const url = 'https://jsonplaceholder.typicode.com/todos/1';
    const response = await fetch(url);
    const todo = await response.json();
    console.log(todo);
}
fetchTodo();
```
  
  
### async 함수
- await 키워드는 반드시 async 함수 내부에서 사용해야 한다. async 함수는 언제나 프로미스를 반환한다. 
- 명시적으로 프로미스를 반환하지 않더라고 암묵적으로 반환값을 resolve하는 프로미스를 반환한다.
  ```javascript
  //async 함수 선언문
  async function foo(n){return n};
  foo(1).then(v => console.log(v)); // 1

  //async 함수 표현식
  const bar = async function(n){return n};
  bar(2).then(v => console.log(v)); // 2

  //async 화살표 함수
  const baz = async n => n;
  baz(3).then(v => console.log(v)); // 3

  //async 메소드
  const obj = {
      async foo(n){return n;}
  };
  obj.foo(4).then(v => console.log(v)); // 4

  //async class method
  class MyClass{
      async bar(n){return n;}
  }
  const myClass = new MyClass();
  myClass.bar(5).then(v => console.log(v)); // 5
  ```
  - 클래스의 constructor 메소드는 async 메소드가 될 수 없다. constructor 메소드는 instance를 반환해야 하는데, async 함수는 언제나 프로미스를 반환하기 때문이다.

### await 키워드
- await 키워드는 프로미스가 settled 상태(비동기 처리가 수행된 상태)가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve 한 결과를 반환한다. 
- await 키워드는 반드시 프로미스 앞에서 사용해야 한다.
  ```javascript
  const fetch = require('node-fetch');
  const getGithubUserName = async id => {
      const res = await fetch(`https://api.github.com/users/${id}`); // 1
      const {name} = await res.json(); // 2
      console.log(name); //YIEUN
  };
  getGithubUserName('Yieun');
  ```

  - 1에서 fetch 함수가 수행한 HTTP 요청에 대한 서버의 응답이 도착해서 FETCH 함수가 반환한 프로미스가 settled 상태가 될 때까지 1은 대기한다. 이후, **프로미스가 settled 상태가 되면 프로미스가 resolve 한 처리 결과가 res 변수에 할당된다.**
- await 키워드는 다음 실행을 일시 중지시켰다가 프로미스가 settled 상태가 되면 다시 재개한다.
```javascript
async function foo(){
    const res1 = await new Promise(resolve => setTimeout(() => resolve(1), 3000));
    const res2 = await new Promise(resolve => setTimeout(()=> resolve(2), 2000));
    const res3 = await new Promise(resolve => setTimeout(()=> resolve(3), 1000));

    console.log([res1, res2, res3]); // [1,2,3]
}
foo(); // 약 6초가 소요된다.
```

   - 그러나, 위의 프로미스들은 각각 독립적인 역할을 수행한다. 순차적으로 처리할 필요가 없다.

```javascript
async function foo(){
    const res = await new Promise.all([
        new Promise(resolve => setTimeout(() => resolve(1), 3000)),
        new Promise(resolve => setTimeout(()=> resolve(2), 2000)),
        new Promise(resolve => setTimeout(()=> resolve(3), 1000))
    ])
    console.log(res); // [1,2,3]
}
foo(); // 약 3초가 소요된다.
```


```javascript
async function foo(n){
    const res1 = await new Promise(resolve => setTimeout(() => resolve(n), 3000));
    const res2 = await new Promise(resolve => setTimeout(()=> resolve(res1 + 1), 2000));
    const res3 = await new Promise(resolve => setTimeout(()=> resolve(res2 + 1), 1000));

    console.log([res1, res2, res3]); // [1,2,3]
}
foo(1); // 약 6초가 소요된다.
```

### 에러 처리
- 비동기 함수의 콜백 함수를 호출하는 것은 비동기 함수가 아니기 때문에 try ...catch 문을 사용해 에러를 캐치할 수 없다.
- async/await 에서 에러 처리는 try ... catch 문을 사용할 수 있다. 콜백 함수를 인수로 전달받는 비동기 함수와 달리 프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기 때문에 caller 가 명확하다.

```javascript
const fetch = require('node-fetch');
const foo = async () => {
    try{
        const wrongURL = 'https://wrong.url';
        const response = await fetch(wrongURL);
        const data = await response.json();
        console.log(data);
    }catch(err){
        console.error(err); //TypeError : Failed to fetch
    }
};

foo();
```

- foo 함수의 catch 문은 HTTP 통신 에러 뿐만 아니라 try 코드 블록 내의 모든 문에서 발생하는 일반적인 에러도 캐치할 수 있다.
- async 함수 내에서 catch 문을 사용하지 않으면, async 함수는 발생한 에러를 reject하는 프로미스를 반환한다.

```javascript
const fetch = require('node-fetch');
const foo = async () => {
    const wrongURL = 'https://wrong.url';
    const response = await fetch(wrongURL);
    const data = await response.json();
    console.log(data);
};

foo().then(console.log).catch(console.error); //TypeError : Failed to fetch
```