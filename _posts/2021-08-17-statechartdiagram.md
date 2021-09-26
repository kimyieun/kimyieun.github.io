---
title: "Ch8. Statechart Diagram"

categories:
  - ooad

tags:
  - uml
---

### Statechart Diagram
- 모든 objects는 서로 다른 유한 개의 state 를 갖는다.
- state machine (=statechart) diagram 사용 목적
  - 시스템이나 object 의 가능한 state 를 모델링하기 위해
  - 이벤트의 결과로 어떻게 state transition 이 일어나는지 보여주기 위해
  - 각 state 에서 시스템이나 object 가 취하는 행동을 보여주기 위해

### State
- state machine 의 node
- active state - object 가 해당 state 에 있는 중.
  - activity 는 여러 개의 actions 로 구성되어 있고, 해당 state 의 activities 를 모두 수행한다.
- state operations
  - entry/Activity(...)
    - object 가 state 에 들어오면 실행한다.
  - exit/Activity(...)
    - object 가 state 에 나가면 실행된다.
  - do/Activity(...)
    - object 가 state 에 머무르는 동안 실행된다. (횟수는 모른다.)
- initial state, terminate node : pseudo state
  - 실제 state 가 아니라서, real state 로 넘어가야 한다.
- final state : real state
  

### Transition
- source state -> target state
- transition : event [guard] / sequence of actions
  - event 가 발생하면
  - guard - condition, option
  - sequence of actions - 이것을 하면서 state transition 한다.

![Validation](/assets/images/transition.png){:width="500px" height="300px"}{: .center}
- 가정 : s1 이 active 상태이다. e 가 발생하면 x 값은 어떻게 될까?
- s1 가 active 되면, x = 4
- e 이벤트가 발생하고, guard 는 체크하고 evaluates to true 가 된다.
- s1 을 exit 하고, x = 5
- x = x*2 를 실행해서 x = 10
- s2 enter 하면서 x = 11 이 된다.


### Composite State
- complex state, nested state - or state
- state 계층 구조. statechart 안에 statechart...
- state 가 substate 를 갖는다. (composite state)
  - 특정 시점에서는 하나의 substate 만 active 된다.
  - arbitrary nesting depth of substates
- **composite, orthogonal, history state** 3가지 특징을 가지는 FSM 은 statechart formalism(diagram) 이라 한다. 


![Validation](/assets/images/compositestate.png){:width="500px" height="300px"}{: .center}
1. s3 에서 시작한다.
2. e2 or e1 발생
   1. e1 이벤트가 발생하면, a0->a1->a3->a7 순서로 실행되고, state 는 S1->S1.2 가 된다.
      1. e4 이벤트가 발생하면, a8->a5->a1 이 된다. 
   2. e2 이벤트가 발생하면, a0->a2->a3->a4 순서로 실행되고, state는 S1->S1.1 이 된다.
      1. e3 이벤트가 발생하면, a6->a5->a2->a1 순서로 실행되고, state는 S2 가 된다.
      2. e5 이벤트가 발생하면, a6->a5->a3->a1 순서로 실행되고, state는 S2 가 된다. 


### Orthogonal State 
- composite state 가 2개 이상의 부분으로 나뉜다.(and state, 점선 표현)
  - 특정 시점에 각 region 의 하나의 state 가 active 하다.
  - concurrent/parallel substates
- entry
  - 모든 regions 의 초기 상태가 activate 된다.
- exit
  - completion event 발생을 위해서는 모든 regions 에서 final state 에 도달해야 한다.
- 여러 개의 regions 을 갖는 state 로 들어가면, 어떤 위치로 들어가든 모든 regions 이 activate 되어야 한다. 
- parallelization node
  - 특정 state 로 바로 들어갈 수도 있다. 이 때, 모든 regions 과 다 연결되어야 한다.
- synchronization node
  - 해당 단계에서 기다렸다가 모든 state 가 도착하면 나간다. 만약에 실수로 하나의 연결을 빼먹으면 deadlock 발생한다.


### Submachine State(SMS)
- notation - state:submachineState
- state machine diagrams 의 부분을 다른 state machine diagrams 에서 재사용하기 위해 사용한다
- submachine state 가 activate 되면, submachine 의 행동이 실행된다.
- refinement symbol - 안경 표시
  - 내 state 안에 statechart diagram 이 잔뜩 있어요 라는 의미.


### History State (syntatic sugar)
- composite state 에서 가장 마지막으로 active 된 substate 를 기억하기 위해서 사용한다.
- shallow history state (H)
  - composite state의 동일한 level 의 state 로 복구
  - h 의 level과 튕겨져 나간 state 의 state 계층 구조에서 h 의 level 과 동일한 state 부터 다시 시작한다.
- deep history state (H*)
  - 전체 nesting depth 에서 마지막으로 active substate 복구
  - 튕겨져서 나간 그 state 부터 다시 시작한다.

![Validation](/assets/images/historystate.png){:width="500px" height="300px"}{: .center}
1. H
   - e2 이벤트가 발생해서 s1.2 에 있는 상태이다.
   - 이때, e10 이 발생해서 s5 로 다시 돌아간다.
   - 그리고 e9 가 발생하면 H 로 들어가는데, 튕겨져 나간 state 가 S1.2 (S1->S4 계층구조이다.). 이 때, H 와 동일한 level 은 S1 으로, S1 으로 다시 들어가고, S1.1 로 이동한다.

2. H*
   - e2 이벤트가 발생해서 s1.2 에 있는 상태이다.
   - 이때, e10 이 발생해서 s5 로 다시 돌아간다.
   - 그리고 e8 가 발생하면 H* 로 들어가고, 이전에 튕긴 state 인 S1.2 로 바로 들어간다.

만약에 튕긴 state 가 없다면?
1. H
   1. Internal transition
      1. entry, exit 하지 않고, initial state 로 바로 들어간다.
2. H*
   1. External transition
      1. entry, exit 도 함께 해준다. 외부에서 들어오거나 나가는 것.