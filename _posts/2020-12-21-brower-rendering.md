---
title: "Browser Rendering(Chapter 38)"

categories:
  - javascript

tags:
  - javascript
---

\*\* 모던 자바스크립트 Deep Dive 책을 읽고 정리한 포스팅이다.

### Parsing

- 프로그래밍 언어로 작성된 텍스트 파일을 읽어 들여 실행하기 위해 텍스트 문서의 문자열을 토큰으로 분해하고, 토큰에 문법적 의미와 구조를 반영하여 트리 구조의 자료구조인 parse tree/syntax tree를 생성하는 과정

### Rendering

- html, css, javascript 로 작성된 문서를 파싱하여 브라우저에 시각적으로 출력하는 것

## 브라우저의 렌더링 과정

1. 브라우저는 html, css, javascript, image, font file 등 렌더링에 필요한 리소스를 요청하고 서버로부터 요청을 받는다.
2. 브라우저의 렌더링 엔진은 html, css를 파싱해 DOM, CSSOM을 생성하고 이들을 결합하여 render tree를 생성한다.
3. 브라우저의 자바스크립트 엔진은 자바스크립트를 파싱하여 AST(Abstract Syntax Tree)를 생성하고 바이트코드로 변환하여 실행한다. 이때 자바스크립트는 DOM API를 통해 DOM, CSSOM을 변경 가능하다. 변경된 DOM, CSSOM은 다시 render tree에 결합된다.
4. render tree를 기반으로 html 요소의 레이아웃(위치, 크기)을 계산하고 브라우저 화면에 html요소를 페인팅한다.

## 요청과 응답

- 브라우저의 주소창에 입력한 URL의 호스트 이름이 DNS를 통해 IP 주소로 변환되고 이 IP 주소를 갖는 서버에게 요청을 전달한다.
- 루트 요청
  - /, 스킴과 호스트만으로 구성된 URL 에 의한 요청 (예시 : https://www.naver.com)
  - 암묵적으로 서버는 루트 호출에 대해 index.html 을 응답한다.

## HTTP 1.1 and HTTP 2.0

- HyperText Transfer Protocol. 웹에서 브라우저와 서버가 통신하기 위한 프로토콜(규약).
- HTTP/1.1
  - 커넥션당 하나의 요청과 응답만 처리한다. 여러 개의 요청과 응답을 한번에 전송할 수 없다.
  - 요청할 리소스 개수에 비례해 응답 시간이 증가한다는 문제점.
- HTTP/2.0
  - 여러 리소스의 동시 전송 가능. 페이지 로드 속도가 약 50% 더 빠르다.

## HTML 파싱과 DOM 생성

- 브라우저가 받은 html 문서는 문자열로 이루어진 순수한 텍스트.
- 렌더링 엔진은 이를 파싱하여 **DOM(Document Object Model)** 을 생성한다.

## CSS 파싱과 CSSOM 생성

- 렌더링 엔진은 html을 처음부터 한 줄씩 순차적으로 파싱하여 DOM 생성한다.
- 이때 css를 로드하는 link나 style 태그를 만나면 DOM 생성을 일시 중단한다. 그리고 CSSOM(CSS Object Model) 을 생성한다.
- CSSOM 은 css의 상속을 반영하여 생성된다.

## 렌더 트리 생성

- 렌더링 엔진은 html, css 를 파싱해 각각 DOM, CSSOM 을 생성한다. 이들은 렌더 트리로 결합된다.
- 렌더 트리 ? 렌더링을 위한 트리 구조의 자료 구조이다. 따라서 브라우저 화면에 렌더링되지 않는 노드(meta, script tag) 와 css에 의해 비표시되는 노드(display : none) 는 포함하지 않는다. 오직 **화면에 렌더링되는 노드만으로 구성**된다.
- 완성된 렌더 트리는 각 html 요소의 레이아웃을 계산하는 데 사용되며 픽셀을 렌더링하는 페인팅 처리에 입력된다.

브라우저 렌더링이 반복되어 실행되는 경우

- 자바스크립트에 의한 노드 추가/삭제
- 브라우저 창의 리사이징에 의한 뷰포트의 크기 변경
- html 요소의 레이아웃에 변경을 발생시키는 width, height, margin, padding, border, display, position, top/right/left/bottom 등의 스타일 변경

## 자바스크립트 파싱과 실행

- DOM은 html 문서의 구조와 정보뿐만 아니라, html 요소와 스타일을 변경할 수 있는 프로그래밍 인터페이스로서 DOM API 를 제공한다.
- 자바스크립트 코드에서 DOM API 를 사용하면 이미 생성된 DOM 을 동적으로 조작할 수 있다.
- css 파싱 과정과 같이 렌더링 엔진은 DOM 생성하다가 **자바스크립트 코드를 로드하는 script 태그나 자바스크립트 코드를 콘텐츠로 담은 script 태그**를 만나면 DOM 생성을 일시 중단한다.
- 그리고 자바스크립트 엔진에게 제어권을 넘긴다. 자바스크립트 엔진은 코드를 파싱하여 CPU 가 이해할 수 있는 low-level language 로 변환하고 실행한다. 크롬, node.js v8 등 여러 종류가 있다.
- 최종적으로는 AST를 생성하고, 이를 기반으로 인터프리터가 실행할 수 있는 중간 코드인 바이트코드를 생성하여 실행한다.

## Reflow and Repaint

- 자바스크립트 코드에 DOM, CSSOM 을 변경하는 DOM API 가 사용된 경우 이는 변경되고, 다시 렌더 트리로 결합되고 레이아웃과 페인트 과정을 거쳐 브라우저 화면에 다시 렌더링한다.
- Reflow?
  - 레이아웃 재계산. 노드 추가/삭제, 요소의 크기/위치 변경, 윈도우 리사이징 등 레이아웃에 영향을 주는 변경이 발생한 경우에 한하여 실행.
- Repaint?
  - 재결합된 렌더 트리를 기반으로 다시 Paint 하는 것.
- reflow, repaint 는 순차적으로 발생하지 않는다. 레이아웃에 영향이 없는 경우 repaint 만 발생한다.

## 자바스크립트 파싱에 의한 html 파싱 중단

- 지금까지 본 바와 같이 **렌더링 엔진과 자바스크립트 엔진은 병렬적으로 파싱을 실행하지 않고 직렬적으로 수행**한다.
- 브라우저는 **synchronous**하다. 따라서 script 태그 위치가 중요하다.
- DOM 이 생성되기 전에 자바스크립트 코드에서 DOM API 를 사용하는 경우 문제가 발생한다.
  - 해결하기 위해 body 요소 가장 아래에 자바스크립트를 위치시킨다.
    - DOM이 완성되지 않은 상태에서 자바스크립트가 DOM을 조작하면 에러가 발생한다.
    - 자바스크립트 로딩/파싱/실행으로 인해 html 요소들의 렌더링이 지장받지 않아 페이지 로딩 시간이 단축된다.

## script 태그의 async/defer attribute

- 위의 문제를 근본적으로 해결하기 위해 HTML5부터 async, defer attribute 추가되었다.
- src attribute 를 통해 외부 자바스크립트 파일을 로드하는 경우에만 사용 가능하다.
- html 파싱과 외부 자바스크립트 파일의 로드가 **asynchronous**으로 동시에 진행된다.
- async attribute
  - html 파싱과 외부 자바스크립트 파일의 로드가 비동기적으로 동시에 진행된다. 단, 자바스크립트의 파싱과 실행은 자바스크립트 파일의 로드가 완료된 직후 진행되고, 이때 html 파싱이 중단된다.
  - 여러 개의 script 의 경우에는 순서와 관계없이 로드가 완료된 것부터 실행하므로 순서가 보장되지 않음.
- defer attribute

  - 자바스크립트 파싱과 실행은 html 파싱이 완료된 직후, 즉 DOM 생성이 완료된 직후에 진행된다. **DOM 생성이 완료된 이후에 실행되어야 할 자바스크립트에 유용**하다.

  ## 정리

  - 그래서 브라우저 렌더링 과정은 어떻게 되는데?

    - 주소창에 url 을 입력하면, 서버에 요청이 보내진다. 이 페이지에 필요한 text,image,js,css,html 등 정적인 파일들을 응답받는다.
    - 브라우저의 렌더링 엔진이 html,css 를 파싱하기 시작하는데, 우선 html 먼저 파싱하면서 DOM Tree를 구축한다.
    - 파싱하는 중에, css link 를 만나게 되면 html 파싱을 중단하고 css를 파싱하여 CSSOM 을 생성한다.
    - 이 두 Tree 를 결합시켜 render tree 를 생성한다. 이 render tree 는 렌더링에 포함되는 요소들로만 구성된 상태이다.
      - 예를 들어, display : none 은 어떤 공간도 차지하지 않으므로 포함되지 않지만, visibility : invisible 속성은 공간은 차지하므로 포함된다.
    - 이제 position 등 layout 을 잡고 paint 를 위하여 render tree 가 사용된다.
    - 이와 별개로, html 파싱 중에 js script 태그를 만나면 html 파싱을 중단하고, 자바스크립트 엔진으로 제어권을 넘긴다.
    - 자바스크립트 엔진은, 코드를 파싱해 CPU 가 이해할 수 있는 Low-level language 로 변환하여 실행한다. AST 를 최종적으로 생성한다.

  - 빈 화면에서 렌더링 완료까지 너무 오래걸리면 어떻게 해결하나?

    - script 태그를 body tag 가장 아래로 내리거나, async / defer 속성을 사용한다.

  - CORS 문제 ? 어떻게 해결?
    - 다른 도메인에서 리소스를 요청할 때 cross-origin HTTP 에 의해 요청하는데, 대부분의 브라우저는 보안상의 이유로 이를 제한한다.
    - Same origin policy
      - 프론트 서버와 백엔드 서버의 포트가 다른 경우 발생한다.
    -
