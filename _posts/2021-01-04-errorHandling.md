### 에러 처리의 필요성

- 발생한 에러에 대해 대처하지 않고 방치하면 프로그램이 강제 종료된다.
- 직접적으로 에러를 발생시키지 않는 exceptional 한 상황이 발생할 수도 있다.

```javascript
const $button = document.querySelector("button"); //에러를 발생시키지 않고, null을 반환한다.
$button.classList.add("disabled"); //TypeError : Cannot read property 'classList' of null
$button?.classList.add("disabled"); //optional chaining 연산자 사용
```

### try ... catch ... finally 문

에러 처리 방법

1. exceptional 한 상황이 발생하면 반환하는 값(null 또는 -1)을 if 문이나 단축 평가 또는 옵셔널 체이닝 연산자를 통해 확인하는 방법
2. 에러 처리 코드를 미리 등록해 두고 에러가 발생하면 에러 처리 코드로 점프하도록 하는 방법
   - try ... catch ... finally 문 (일반적으로 error handling이라고 한다.)

### Error 객체

- message property와 stack property를 갖는다.
  - message property 값 : Error 생성자 함수에 인수로 전달한 에러 메시지
  - stack property 값 : Error 를 발생시킨 콜스택의 호출 정보를 나타내는 문자열. 디버깅용.
- 7가지의 Error 생성자 함수를 제공한다.
  - Error : 일반적인 객체 에러 객체
  - SyntaxError : 자바스크립트 문법에 맞지 않는 문을 해석할 때 발생하는 에러 객체
  - TypeError : 피연산자 혹은 인수의 데이터 타입이 유효하지 않을 때 발생하는 에러 객체
  - ReferenceError : 참조할 수 없는 식별자를 참조했을 때 발생하는 에러 객체
  - RangeError : 숫자값의 허용 범위를 벗어났을 때
  - URIError : encodeURI, decodeURI 함수에 부적절한 인수를 전달했을 때
  - EvalError : eval 함수에서 발생하는 에러 객체

### throw 문

- 에러 객체를 생성한다고 에러가 발생하는 것은 아니다. 즉, **에러 객체 생성과 에러 발생은 의미가 다르다.**

```javascript
try {
  new Error("something wrong");
} catch (error) {
  console.log(error);
}
```

```javascript
try {
  throw new Error("something wrong");
} catch (error) {
  console.log(error);
}
```

- throw 문의 표현식은 일반적으로 에러 객체를 지정한다. 에러를 던지면 catch문의 에러 변수가 생성되고 던져진 에러 객체가 할당된다. 그리고 catch 코드 블록이 실행된다.

### 에러의 전파

```javascript
const foo = () => {
  throw Error("foo에서 발생한 에러");
};

const bar = () => {
  foo();
};

const baz = () => {
  bar();
};

try {
  baz();
} catch (err) {
  console.error(err);
}
```

- foo 함수가 throw한 에러는 caller에게 전파되어 전역에서 캐치된다.
- **비동기 함수인 setTimeout이나 프로미스 후속 처리 메소드의 콜백 함수는 caller가 없다.**  
  따라서 에러를 전파할 caller가 존재하지 않는다.
