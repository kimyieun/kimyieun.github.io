---
title: "Microkernel Architecture Pattern"

categories:
  - architecturestyle

tags:
  - architecturestyle
---

### Microkernel Architecture Pattern
- 기존의 Monolithic kernel based OS
  - 레이어별로 수직적으로 쭉 연결되어 있다.
- microkernel based OS
  - 핵심은 코어에 담고 덜 중요한 기능들은 플러그인 형태로 확장성 있게 구현하자
  - 모듈 단위 개발로 확장과 포팅이 용이하지만, 메시지 전달 방식으로 자원에 접근해서 태스크 스위칭에 오버헤드
- 시스템 요구사항에 변할 때 이를 쉽게 시스템에 반영하기 위한 패턴
- 최소 핵심 기능을 공통으로 두고 변경 가능한 확장 부분을 플러그인 방식으로 추가
- Context
  - 동일한 기능을 기반으로 비슷한 프로그래밍 인터페이스를 제공하는 시스템 개발
- Problem
  - 애플리케이션 플랫폼은 HW, SW 의 지속적인 발전/변화를 반영해야 한다.
  - 새롭게 생겨난 기능을 쉽게 통합하기 위한 이식성,확장성,적용성 보장 필요하다.
  - 기존 표준을 준수한 애플리케이션도 실행 가능해야 한다.
- Solution
  - microkernel 컴포넌트는 캡슐화하고, 다른 컴포넌트와 통신하는 기능을 가진다.
    - 파일이나 프로세스처럼 리소스 저장 역할. 다른 컴포넌트 접속하도록 인터페이스 제공
    - 여기에 필요없는 핵심 기능은 internal server 로 분리시켜 크기랑 복잡성을 줄인다.
  - external server 에는 external server 자체의 뷰를 구현하고 microkernel 인터페이스를 이용해 연동한다.
  - client(사용자가 아니라 요청을 하는 application) 는 microkernel 이용해 external server 와 통신한다.


### 각 컴포넌트의 역할
- external server
  - 자체 클라이언트를 위한 인터페이스 제공
- microkernel
  - 핵심 메커니즘 제공, 통신 기능 제공
  - 시스템 종속성 캡슐화, 리소스 관리
- adapter
  - 클라이언트와 통신하는 기능 등의 시스템 종속성 숨김 (proxy 같은 역할)
  - 클라이언트 대신해 외부 서버가 메소드 호출 
- internal server
  - 추가 서비스 구현
  - 특정 시스템에 맞춰진 구현 캡슐화
- client
  - 특정 외부 서버 하나랑 연결된 application (1:1 매핑)
    


### 동작 방식
1. 클라이언트가 외부 서버에 서비스를 호출할 때 동작
   1. client 는 adapter 의 callservice 호출한다. 
   2. adapter 는 microkernel 과 initCommunication 시작한다.
   3. microkernel 은 receiver 를 찾고 adapter 에게 돌려준다.
   4. adapter 는 external server 로 바로 요청을 보낸다.
   5. external server 는 서비스를 실행하고 그 결과를 adapter에게 주고 adapter 는 client 에게 돌려준다.
2. 외부 서버가 내부 서버에서 제공하는 서비스에 대한 요청을 할 때 하는 동작
   1. 외부 서버는 microkernel 의 메커니즘을 실행한다.
   2. microkernel 은 내부 서버를 호출하고, 요청을 internal server 로 보낸다.
   3. 내부 서버는 서비스를 실행해 결과를 microkernel 에게 주고, microkernel 은 external server 로 돌려준다.
 

### Discussion
- 장점
  - 이식성 보장
  - 유연성과 교환 가능성 - 외부 서버 추가 및 확장
  - 정책과 메커니즘 분리해 유지보수성과 가변성 향상(정책 - 외부 서버, 메커니즘 - 마이크로커널)
  - 확장성 확보, 신뢰성, 투명성 제공 (분산 형태의 microkernel 에서)
  - 
- 단점
  - 성능떨어짐
  - 시스템 설계 복잡 개발 어려움
