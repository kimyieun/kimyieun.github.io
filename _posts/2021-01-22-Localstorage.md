---
title: "Day15 : LocalStorage"

categories:
  - javascript

tags:
  - javascript, HTTP protocol, cookie, session, WebStorage, LocalStorage, SessionStorage
---

### HTTP Protocol

- Stateless - 통신이 끝나면 상태 정보를 유지하지 않는다. 페이지를 새로 켜거나 새로고침을 할 때마다 초기화되는 문제가 있다.  
  이를 해결하기 위해 쿠키, session, webstorage 가 사용된다.

### Cookie
- 서버가 사용자의 웹 브라우저에 전송하는 작은 데이터 조각이다. 브라우저가 서버에 요청을 보낼 때 쿠키를 함께 전송한다.
- 쿠키는 client 에 저장된다.
- **과거에 클라이언트 쪽에 정보를 저장할 때 쿠키를 사용하였지만, 지금은 webstorage 를 사용하는 것을 권장한다.**
  - 쿠키는 매번 서버로 전송된다. 웹사이트에서 쿠키를 설정하면 매 서버 요청마다 쿠키 정보를 포함하여 전송한다. 네트워크 트래픽 비용이 증가한다.
  - 쿠키 크기는 4KB 로 굉장히 제한이 있다.
  
- HTTP 요청을 수신할 때, 서버는 Response 와 함께 Set-Cookie 헤더를 전송할 수 있다.
  - session key, 만료 기간, 지속시간 등 명시한다.

- Document.cookie 를 사용해 자바스크립트에서 쿠키를 접근할 수 있다.
  - 변경도 가능하다. 쿠키 값을 변경해버리면 로그인이 풀리게 될 수도 있다.
  - 옛날에 웹서비스에서는 이벤트를 열 때, 이벤트 참여 기록을 쿠키에 남기기도 했다. 그러면, 특정 사용자들이 쿠키를 수정하여 이벤트에 여러 번 중복하여 참여하는 것이 가능했다..
  - "3일간 보이지 않기" 와 같은 기능은 cookie 세팅일 만료를 3일로 잡는 방식으로 구현되어있다. 

### Session
- cookie 를 기반하지만, 세션은 서버에서 관리한다.
- 사용자가 웹페이지에 접속하면, 서버가 cookie를 확인해 브라우저가 session id 를 가지고 있는지 확인한다.
- 없으면, 새로운 session id 를 부여하고, session id 를 서버에 저장한다. 
- 보안 측면에서는 서버에 저장되기 때문에 쿠키보다 안전하지만, 서버자원을 사용한다는 단점이 있고 속도도 느리다.

### WebStorage (HTML5)

- 서버에 저장할 필요가 없는 간단한 데이터를 저장할 곳이 필요할 때, 브라우저 상에 데이터를 저장할 수 있도록 하는 기술.
- localStorage and SessionStorage

### LocalStorage

- 브라우저를 종료해도 데이터가 영구적으로 유지되나, 도메인이 다른 경우 localStorage 에 접근이 불가하다.
- client 가 특정 domain 에서 passwd 를 입력하면 서버는 session key 를 넘겨준다. 그리고 localstorage 에 그 session key 를 저장한다. 
이후 요청에서는 local storage 를 읽어서 header 에 포함시켜서 보낸다. 

### SessonStorage

- 각 세션마다 데이터가 개별적으로 저장되며 세션이 종료(브라우저 탭 종료)되면 데이터가 제거된다. 같은 도메인이라도 세션이(브라우저가) 다르면 데이터에 접근할 수 없다.

### 정리
- cookie, sessionStorage, LocalStorage 의 공통점은 도메인마다 독립적이며, 클라이언트가 임의로 수정이 가능하므로 임의 변경에 대한 방어 코드가 필요하다.
