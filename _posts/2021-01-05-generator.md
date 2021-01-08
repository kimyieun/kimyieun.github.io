# Generator

- ES6에서 도입된 generator는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수이다.
- 일반 함수와의 차이점

  - 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.
  - 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.
    제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.

  ### 제너레이터 함수의 정의

  - function\* 키워드로 선언하고, 하나 이상의 yield 표현식을 포함한다.

  ```javascript
  function* getDecFunc() {
    yield 1;
  }

  const getExpFunc = function* () {
    yield 1;
  };

  const obj = {
    *genObjMethod() {
      yield 1;
    },
  };

  class Myclass {
    *getClsMethod() {
      yield 1;
    }
  }
  ```

  - asterisk(\*)의 위치는 function 키워드와 함수 사이이면 상관없다.
  - **화살표 함수로 정의할 수 없다. new 연산자와 함께 생성자 함수로 호출할 수 없다.**

### 제너레이터 객체

- generator 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성하여 반환한다.
- 제너레이터 객체는 iterable이면서 동시에 iterator이다.
