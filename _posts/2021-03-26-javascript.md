---
title: "Javascript 2020년 이후의 동향"

categories:
  - javascript

tags:
  - javascript
---

- 이 포스팅은 https://d2.naver.com/helloworld/4268738 에 올라온 '2020년과 이후 JavaScript의 동향 - JavaScript(ECMAScript)'를 읽고 정리한 글이다.

## 브라우저 엔진

- Chromium
  - Chrome, MS Edge, Opera
  - rendering engine : Blink, Javascript engine : V8
- Webkit
  - Safari
- gecko
  - Firefox

## ESM 지원이 확장되면 번들러는?

- 모던 브라우저만 타겟하면 ESM 은 거의 대다수의 브라우저에서 네이티브하게 지원한다. 그러면 번들러는 필요없냐?
- NO. import/export 구문은 정적으로 분석이 가능하기 때문에, 번들러는 코드를 분석해 사용하지 않는 export 를 제거해 최적화할 수 있다.

## ECMAScript 2020

- 2020/6/18일 공식적으로 릴리스되었다.
- 동적 import() 구문

  - Promise 기반의 import() 구문은 javascript 모듈을 동적으로 로딩한다.
  - 1. 동적 매개변수 사용. 2) 런타임이나 조건부로 모듈 불러옴

  ```javascript
  import("/modules/my-module.js").then((module) => {
    //do something
  });

  let module = await import("/modules/my-module.js");
  ```

- Optional chaining
- Nullish coalescing operator
- export \* as ns from 'mod'

  - 모듈 import 한 후 새로운 이름으로 export 할 수 있게 한 문법이다.

  ```javascript
  export * as ns from "mod";
  ```

## Transpiler(Babel)은 계속 필요할까?

- 과거 Node.js 의 모던 ECMAScript 문법들의 미지원으로 인해 Babel 을 사용했으나, 오늘날 Node.js 는 대부분의 ECMAScript 문법을 지원하므로 별도의 transpile 과정이 필요 없어졌다.
- Babel 이 등장함으로써 크로스브라우저 이슈를 해결할 수 있었으나, 오늘날 프로젝트에서는 Babel 을 사용하지 않는 경우가 대부분이다.
  - 그러나 Babel 은 단지 최신 명세를 변환시키는 것 뿐만 아니라, 모던 프레임워크들에서 사용되는 새로운 문법(JSX와 같은)처리 등의 필요성이 여전히 있다.
