---
title : "Babel과 Webpack을 이용한 ES6+/ES.NEXT 개발 환경 구축"

categories :
    - javascript

tags :
    - javascript

---
  ** 모던 자바스크립트 Deep Dive 책을 읽고 정리한 포스팅이다.

- ES.NEXT (제안 단계에 있는 ES 제안 사양)
- 대부분의 프로젝트가 모듈을 사용하므로 모듈 로더가 필요하다. ES6 모듈은 대부분의 모던 브라우저에서 가능하지만, 아래와 같은 이유로 별도의 모듈을 사용하는 것이 더 일반적이다.
  - IE를 포함한 구형 브라우저는 ESM 을 지원하지 않는다.
  - ESM (ES6 모듈) 을 사용하더라도 트랜스파일링이나 번들링은 필요하다.
  - ESM 에서 지원하지 않는 기능들이 아직 있다.

### Babel

- Javascript Transpiler (Compiler 의 일종)
  - ES6+/ES.NEXT 로 구현된 최신 사양의 소스코드를 구형 브라우저에서도 동작하는 ES5 사양의 소스코드로 Transpiling 한다.
- Babel 을 사용려면 @babel/preset-env 를 설치해야 한다.

  - 함께 사용되어야 하는 Babel 플러그인을 모아 둔 것으로 Babel 프리셋이라 부른다.
  - Official preset
    - @babel/preset-env
    - @babel/preset-flow
    - @babel/preset-react
    - @babel/preset-typescript
  - 필요한 플러그인들을 프로젝트 지원 환경에 맞춰 동적으로 결정해준다.

- Transpiling
  - Babel CLI 명령어를 사용하여 트랜스파일링할 수도 있으나, 매번 CLI 명령어를 입력하는 것은 번거로우므로, npm scripts 에 Babel CLI 명령어를 등록해 사용한다.

### Webpack

- 앞서 언급한 ESM의 불완전함을 이유로 Webpack 을 사용하는 것이 더 일반적이다.
- 의존 관계에 있는 js, css, image 등의 리소스를 하나(또는 여러 개)의 파일로 번들링하는 모듈 번들러이다.
- Webpack 말고도 Rollup & Parcel 번들러도 있다.
  - 상황에 따라 적절하게 선태해 사용하면 된다.
- 의존 모듈이 하나의 파일로 번들링되므로 별도의 모듈 로더가 필요 없다. 그리고 html 파일에서 여러 개의 script 태그를 사용할 필요도 없다.

### 1. webpack 설치

- npm install webpack webpack-cli

### 2. babel-loader 설치

- webpack이 모듈을 번들링할 때 Babel을 사용해 ES6+/ES.NEXT 사양의 소스코드를 ES5 사양의 소스코드로 트랜스파일링하도록 babel-loader 를 설치한다.
- npm scripts 를 변경하여 Babel 대신에 Webpack을 실행하도록 수정한다.

### 3. webpack.config.js 설정 파일 작성

- webpack.config.js 설정 파일에 필요한 부분들을 작성한다.

### 4. babel-polyfill 설치
