---
title: "Ch5. Use Case Diagram"

categories:
  - softwareengineering

tags:
  - uml
---

### Use Cases
- text stories of some actors using a system to meet goals.
  - requirements 발견하고 분석하기 위한 메커니즘
  - not a diagram, but a text (3 formats)
    - brief -> casual -> fully dressed

### Use Case Diagram
- use cases, actors, 그들간의 관계 표현
- 용도
  - System context diagram (system boundary)
  - A summary of all use cases

![Validation](/assets/images/usecasediagram.webp){:width="500px" height="300px"}{: .center}

- Actor
  - 상황에 따라 actor role 이 바뀔 수 있다.
  - primary actor : system 직접 사용해서 목표를 충족시키는 actor (cashier)
  - supporting actor : system 에 service 를 제공하는 actor (payment authorization service) 
  - offstage actor : use case 의 행동에 관심은 있지만, not primary or supporting. (tax agency)


### 3 formats of use cases
- Brief
  - 간결한 one paragraph
  - main scenario, happy path 만 보통 작성한다.
  - 구현하기로 결정하면 아래 두가지의 포맷으로 넘어간다.
- Casual
  - informal multiple paragraph format
  - 3가지 시나리오를 구별해서 다룬다.
    - main success scenario
      - 가장 많이(typical) 발생하는 시나리오
    - alternate scenario (extensions)
      - 그 외 success 는 하지만 not typical 시나리오
    - exceptional scenario
      - success 하지 못하는 시나리오
- Fully dressed
  - 디자인 단계(OOD)에서 작성하는 가장 디테일한 단계이다.
  - all steps, variations, supporting sections 도 표현한다.
    - preconditions - use case 시작하기 전에 system 상태

### Guideline - write in an essential style
- essential writing style
  - 구체적인 action 은 제외하고 사용자 인터랙션과 시스템 책임을 표현한다.
    - UI free-style (flexible, implementation detail 은 가급적 포함하지 않는다.)
    - 초기 requirements 분석 단계에서는 구체적인 use case 는 피하는 것이 좋다.
- e.g., system 은 사용자 identity 를 인증한다.
  - 지문, 홍채인식, 비밀번호인지 구체화하지 않는다.

### Guideline - write black-box use cases
- system 의 내부 동작, components, design 은 묘사하지 않는다.
  - 대신, system 이 어떻게 동작하는지(design)보다, 무엇을 하는지를 정의해라(design).
- e.g., 이 시스템은 판매량을 기록한다.
  - 이 값을 DB 에 저장하고, SQL INSERT 문을 생성한다 와 같은 구체적인 내용은 포함하지 않는다.

### Are use cases funtional requirements?
- YES (in OOAD)
- not functional requirements 는 SRS 의 F(use case)/NF 에서 NF 로 표현된다.
- use cases는 requirements, 특히 functional requirements 다.
- 