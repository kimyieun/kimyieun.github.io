---
title: "Ch12. Inception"

categories:
  - ooad

tags:
  - uml
---

### Inception
- 대부분의 프로젝트는 아래 질문에 대한 답을 준비하기 위한 initial step (1주) 이 필요하다.
  - vision, business case
  - feasible
  - buy and/or build
  - cost
  - proceed or stop
- inception 은 짧아야 한다. 
  - one week. 대부분의 requirement analysis 는 inception 이 아닌, elaboration 단계에서 일어난다 -> casual format 의미한다. 
  - inception 은 대부분의 requirements 를 찾지만, brief format 정도로 찾는다.

### Artifacts start in inception
- use-case model 
  - functional requirements
- supplementary specification
  - non-functional requirements 정리
- glossary - terminology
- prototypes and proof-of-concepts
- development case 
  - 이 프로젝트에 customize 된 up 단계와 필요한 artifacts 를 정리한 문서

### How much UML during inception?
- inception 의 목적은 최대한 정보를 많이 모으는 것!
  - common vision 확립
  - feasibility test, elaboration phase 로 진입할지 결정
- 대부분의 UML 다이어그램이 필요 없다.
  - inception 은 basic scope, 10% of the requirements 를 이해하는데 대부분 사용한다.
  - 실제로 대부분의 UML 다이어그램은 elaboration phase 에 등장한다.


### Requirements
- 시스템이 갖춰야 할 capabilities, conditions 
- requirement analysis
  - client 와 team members 에 명확한 형태로 적힌 필요한 requirements 를 찾고, communicate, organize 한다.
  - requirement engineering - requirement identification - requirement analysis - requirement validation
- UP 에서는 requirements analysis 가 iterative, skillfull 하게 수행된다.
  - skillful elicitation (finding)
    - client 와 함께 use case 를 작성한다.
    - 모두가 함께 requirement workshop에 참여한다.
    - 매 iteration 마다, client 에게 일종의 데모, 프로토타입을 보여주고 피드백을 받는다.
  - waterfall 에서는 이 단계에서 requirement analysis 를 픽스한다.

### Types and Categories of requirements
- UP 에서 requirement 는 FURPS+ model 에 따라 카테고리화 된다.
  - Functional
  - Usability
  - Reliability
  - Performance
  - Supportability
- reference model 로서 checklist 로만 활용해라.

### Quality Attributes/Requirements
- usability, reliability, performance, supportability 
- non-functional requirements 
- quality attributes 도 system 의 architecture 에 큰 영향을 준다.


### How requirements are organized
- UP는 여러 requirements artifacts 를 제공한다(단, optional).
  - use case model
    - inception 에서 시작해서 elaboration 까지 계속 refine.
    - elaboration 의 r 은, requirements 가 evolutionary 하게 변경되는 것을 의미한다. 
      - brief->casual->fully 로 내려간다는 의미가 아니다. 
    - functional requirements
  - supplementary specification
    - all non-functional requirements (performance, licensing,...)
    - 특별한 diagram 을 사용하진 않고, format 은 자유로운 문서이다.
    - quality requirments, reports, hw constraints, dev constraints, internationalization, packaging, documentation, standards, domain rules, ...
  - glossary
  - vision
  - business rules
- 위의 내용들을 다 적은 문서가 software requirements specification (SRS)가 된다. 
- Waterfall 모델에서는 inception -> requirements analysis -> SRS -> design -> SDS -> coding 단계로 수행된다.
- UP 에서는 Elaboration 단계 이후에 SRS 가 나온다. 
  
