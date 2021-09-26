---
title: "Ch9. Activity Diagram"

categories:
  - ooad

tags:
  - uml
---

### Activity Diagram
- activity diagram 은 system 의 dynamics 를 표혀한다.
  - flow of actions in system
  - alternate paths
  - parallel, branched and concurrent flow
- 한 activity 에서 다른 activity 로의 message flow 는 보여주지 않는다. (data flow 는 제공 x)
- methods/functions 에서 operations flow 는 보여준다.

### Activity / Transition
- Activity
  - 상태가 아닌 일을 나타낸다. e.g., add a new client
- Transition
  - activities 간의 flow, sequence 를 나타낸다. statechart 와 다르게 조건이 없음.
- Start state - 검은 색 동그라미
- decision points - 다이아몬드
- Guard conditions - [campaign to add]
- Final State - 흰 동그라미 안 검은 동그라미

### Branching
- Guard conditions 는 mutually exclusive 를 권장하지만 무조건 그래야 하는 것은 아니다.
- Decision points, guard conditions

### Objects
- 자주 사용하지는 않는다. object flow 를 나타낸다.

### Swimlanes
- vertical column 사용한다. 
- person, organisation, department 도 함께 레이블링 해서 표현한다.
- 보통은 그냥 swimlanes 사용하지 않는다.

### Activity diagram vs. statechart diagram
- activity diagram 은 event(trigger) 메커니즘이 없이 functions flow 를 나타내는 것이고, statechart diagram 은 triggered state 로 이루어져 있다.
- statechart 는 events 에 의해 actions 을 수행하지만, activity diagram 은 node 간의 transition 이 activity 의 완료에 따라 자동으로 진행된다.

