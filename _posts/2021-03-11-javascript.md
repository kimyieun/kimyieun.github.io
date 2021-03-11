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

