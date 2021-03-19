---
title : "Javascript"

categories :
    - javascript

tags :
    - javascript, ECMAScript, Node.js, ES6, Ajax, jquery

---
- 모던 자바스크립트 Deep Dive 책을 읽고 정리한 포스팅이다.

## Javascript, ECMAScript 의 탄생
- Javascript 가 탄생한 후에 파생 버전인 JScript 가 출시되었다. 
- MS 는 JScript 를 Explorer 에 탑재하였고, 넷스케이프는 Javascript 를 탐재하였다. 이로 인해, 각 브라우저들이 경쟁적으로 기능을 추가하기 시작했고, 브라우저에 따라 웹페이지가 정상 동작하지 않는 **cross browsing issue**가 발생했다.
- 표준화에 대한 필요성이 대두되었고, 표준화된 ECMAScript 가 완성된다.
  -  ES6 (2015) 에서 중요한 기능들이 대거 도입되었다.   
  (let/const, arrow function, class, module 등)
  -  매년 비교적 작은 기능을 추가하는 수준으로 버전업이 될 것으로 예고되었고, 2020 년 기준 ES11까지 나왔다.
  -  현재 각 브라우저 제조사는 ECMAScript 사양을 준수해 브라우저에 내장되는 javascript 엔진을 구현한다.


## Ajax
- javascript 를 이용해 서버와 브라우저가 asynchronous 방식으로 데이터를 교환할 수 있는 통신 기능. XMLHttpRequest 라는 이름으로 등장하였다.
- Ajax 등장 이전에는 서버로부터 완전한 HTML 코드를 전송받아 웹페이지 전체를 렌더링하는 방식으로 동작하였다. 이로 인해 화면이 전환되면 순간적으로 깜박이는 현상이 발생하였다. (웹페이지의 어쩔 수 없는 한계로 받아들였음)
- Ajax 등장으로, 변경이 필요없는 부분은 다시 렌더링하지 않아도 되서 성능의 개선이 이루어졌다. 

## jQuery
- 2006년 등장하여, DOM 을 쉽게 제어하기 위하여 사용되었다. javascript 를 사용하지 않고, 더 쉽고 직관적인 jQuery 를 더 선호하는 개발자들이 많았다.

## Node.js
- javascript 를 브라우저가 아닌 환경에서도 동작할 수 있도록 javascript 엔진을 브라우저에서 독립시킨 javascript 실행(runtime) 환경이다.
- 주로 Server-side Application 개발에 사용된다. javascript 는 Node.js 의 등장으로 인해 프론트엔드 영역뿐만 아니라 백엔드 영역까지 아우르게 되었다.


## Javascript
- 웹 브라우저에서 동작하는 유일한 프로그래밍 언어이며, **프로토타입 기반의 객체지향 언어**이다. 
- 런타임에 컴파일되고 실행 파일이 생성되지 않으며 interpreter 도움 없이 실행할 수 없으므로 compile 언어가 아닌 interpreter 언어이다.
- 실행 환경 (둘 다 ECMAScript 실행 가능)
  - 브라우저 환경
    - HTML, css, javascript 를 실행해 웹페이지를 브라우저 화면에 렌더링하는 것이 주된 목적, DOM API 제공, File system 제공 X
  - Node.js 환경
    - 브라우저 외부에서 자바스크립트 실행 환경을 제공하는 것이 주된 목적, DOM API 제공 X, File system 제공
    - Node.js REPL(Read Eval Print Loop) 



## variable hoisting
- 소스코드 평가 과정(변수/함수 선언문 실행)이 끝나면, 소스코드를 한 줄씩 순차적으로 실행하는 런타임이 실행된다.
- 소스코드 평가 과정에서 변수 선언문은 코드의 선두로 끌어 올려진 것처럼 동작한다.

```javascript
console.log(score); //undefined
var score; 
```
- 변수 뿐만 아니라, var, let, const, function, class 키워드를 사용해 선언하는 모든 식별자는 호이스팅된다. 모든 선언문은 런타임 이전 단계에서 실행되기 때문이다.

```javascript
console.log(score);
score = 80;
var score;
console.log(score); // ?
```

## Template literal
- ES6 부터 도입되었다. 편리한 문자열 처리 기능을 제공하고, 런타임에 일반 문자열로 변환되어 처리된다.
- Backtick (``) 을 사용해 표현한다.


1. multi-line string
- 일반 문자열 내에서는 줄바꿈 허용되지 않는다. 이스케이프 시퀀스를 사용해야한다.


```javascript
var template = `<ul>
    <li>a</li>
    <li>b</li>
</ul>`;
console.log(template);
/*
<ul>
    <li>a</li>
    <li>b</li>
</ul>
*/
```

2. 표현식 삽입

```javascript
console.log(`1+2=${1+2}`);
``` 

## undefined / null type
- undefined 는 개발자가 의도적으로 할당한 값이 아니라 js 엔진이 변수를 초기화할 때 사용하는 값이다.
- 반면에 null 은 변수에 값이 없다는 것을 의도적으로 명시할 때 사용된다. (intentional absence)

## Dynamic Typing
- static / strong type language
  - 변수 선언 시, 변수에 할당 가능한 값의 종류와 타입을 명시해야 한다. **explicit type declaration**
  - 컴파일 시점에 타입 체크를 수행하여 에러를 발생시킴으로써, 런타임에 발생하는 에러를 줄인다.
- dynamic / weak type language
  - js, python 등의 언어는 값을 할당하는 시점에 변수의 타입이 동적으로 결정되고 변수의 타입을 언제든 변경 가능하다.

## 쉼표 연산자 / 지수 연산자
```javascript
var x, y, z;
x = 1, y = 2, z = 3; // 3(제일 마지막 피연산자 평가 결과 반환)
```

```javascript
2 ** 2; // 4
Math.pow(2, 2); // 4 (ES7 지수 연산자 도입 전 사용)
```

## block
```javascript
function sum(a, b) {
    return a + b;
}
// block 문 마지막에는 ; 을 붙이지 않는다. 
```

## this
- this는 자기 참조 변수로, 자신이 속한 객체나 자신이 생성할 인스턴스를 가리킨다.
- 이 때, this 가 가리키는 값은 함수 호출 방식에 의해 동적으로 결정된다.
- 일반 함수 호출 - 전역 객체
  - strict mode 가 적용된 일반 함수에서는 undefined 바인딩된다.
- 메소드 호출 - 메소드를 호출한 객체
  - 메소드 내 정의된 중첩 함수/콜백 함수, 메소드 함수도 일반 함수로 호출되면 전역 객체 바인딩된다. 
    - 이를 해결하기 위해 ES6 이전에는 that 변수에 재할당 후 사용하는 방식을 택함.
    - ES6부터는 화살표 함수를 사용한다. 화살표 함수는 함수 자체의 this binding이 없으므로 상위 스코프의 this 를 사용하기 때문에.
- 생성자 함수 호출 - 생성자 함수가 생성할 인스턴스
- Function.prototype.apply/call/bind 메소드에 의한 간접 호출 - 앞의 메소드들의 첫 번째 인수로 전달된 객체
  - arguments 객체와 같은 유사 배열 객체나 이터러블에 배열 메소드를 사용하기 위해서 사용된다. 
  - apply는 this와 인자를 list 에 넣어서 parameter 로 사용한다.
  - call은 this 와 인자를 쉼표로 구별하여 parameter 로 사용한다.
  - bind는 this 를 parameter 로 사용하고, 함수를 실행하지 않는다.
- 화살표 함수는?
  - 화살표 함수는 apply, call, bind 메소드 사용해도 this 변경할 수 없다.
