---
title: "Client-Dispatcher-Server Design Pattern"

categories:
  - architecturestyle

tags:
  - architecturestyle
---

### Client-Dispatcher-Server Design Pattern
- 아키텍처 패턴보다는 디자인 패턴으로 소개
- 디스패처 중간 레이어 도입
  - 네임 서비스를 통해 위치 투명성을 제공하고 클라이언트 서버 간의 통신을 위해 세부 구현을 숨김
- Context
  - 분산 서버들이 로컬 or 네트워크 상에 분산되어 실행된다.
  - 분산된 서버들을 통합해야 한다.
- Problem
  - 분산된 서버를 사용할 때 서버들간의 통신할 방법을 제공해야 한다.
  - 클라이언트는 서버 위치를 알 필요가 없고, 변경되어도 상관없다.
- Solution
  - 서비스 제공자의 위치를 상관없이 서비스 사용 가능
  - 디스패처 도입 
    - 네임 서비스는 물리적 위치가 아닌 이름으로 서버를 참조하는 방식, 위치 투명성 제공
    - 클라이언트와 서버 간의 통신 채널 설정 책임
- Client 
  - 디스패처에게 서버와의 연결 요청
  - 서버의 서비스 호출
- Server
  - 클라이언트에게 서비스 제공한다.
  - 디스패처를 통해 자신을 등록한다. (이름과 주소 사용)
- Dispatcher
  - 클라이언트와 서버간의 통신 채널 설정
  - 서버를 찾고, 등록하거나 등록해제 한다. (서버 이름과 서버의 물리적 위치 매핑)


### 동작 방식
1. 디스패처는 서버를 등록한다.
2. 클라이언트는 디스패처에게 getChannel 호출한다. 
3. 디스패처는 서버위치를 확인하고 서버 연결이 되어 있는지 통신한 후에 클라이언트에게 준다.
4. 그 이후로는 클라이언트가 직접 서버에게 요청을 보낸다.

### 구현 순서
1. 애플리케이션을 서버와 클라이언트로 분리한다.
2. c-d, s-d, c-s 간의 상호작용을 위한 통신 기능을 선택한다.
3. 컴포넌트 간의 상호작용 프로토콜을 정의한다.
4. 서버 이름 결정한다.
5. 디스패처 설계 및 구현한다.
6. 디스패처 인터페이스에 맞게 client, server 구현한다.

```java
Service s1 = new PrintService("printsvc", "srv1");
Service s2 = new PrintService("printsvc", "srv2");
Client client = new Client();
client.doTask();

// doTask
class Client{
  public void doTask(){
    Service s;
    try{
      s = CDS.disp.locate("printsvc"); // dispatcher locate 메소드를 사용해 server 위치 파악
      s.service(); // 서버의 위치를 받아와서 직접 요청
    }
  }
}
```

### Discussion
- 장점
  - 서버 교환 가능성, 위치 투명성 보장
  - 재구성 가능, 장애 허용성 보장(서버 바꾸면 된다)
- 단점
  - 명시적이며 우회적 연결 설정하므로 낮은 효율성
  - 디스패처에 의존하게 되므로 디스패처의 변경에 민감
