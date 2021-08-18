---
title: "Ch7. Sequence Diagram"

categories:
  - softwareengineering

tags:
  - uml
---

### Interaction Diagrams
- object 가 어떻게 메시지를 사용해서 인터랙션하는지 보여준다.
- dynamic object modeling
- types
  - sequence diagram
  - communication diagram
    - class diagram 의 구조를 그대로 차용한다. size 가 작다는 장점이 있지만, 시간 순서를 번호로 찾아야 한다.
  - interaction overview diagram
    - activity diagram(flowchart). 여러 sequence diagram, communication diagram 으로 구성된다.
  - timing diagram

### Sequence Diagram and Communication Diagram
- Sequence Diagram
  - time sequence 에 기반하여 object collaboration 을 모델링 한다.
  - operation body 를 채울 수 있다.
  - OOAD 에서 dynamic model 에 해당된다. cf. class diagram - static model (class 계층구조, operation, attribute 의 declaration 만 알고 있다.)
- Communication Diagram
  - time sequence 보다는 object collaboration 을 보여주는 것에 더 포커싱한다.
  - 번호가 없는 operation 은 system operation 이다.


### Communication Diagram Notations
- Link and Message
  - 모든 메시지는 link 를 따라 흐른다. 


### Sequence Diagram Notations
- lifeline boxes and lifelines
  - interaction 에서 참여자 (roles) 를 표현한다.
  - objects, class, sybsystem, component, etc.
  - <\<metaclass>> - abstract class 를 표현한다.
  - sales:ArrayList\<Sale> - 여러 개의 object 표현, sales[i]:Sale - collections 중 하나의 instance 표현.  
- messages
- execution specification bar 
  - operation body 의 내용과 기간을 나타낸다.
  - message 가 execution 되는데 걸리는 시간을 나타낸다.


### Order of Messages
- 하나의 lifeline 에서는 순서가 있다.
- 다른 lifeline 에서는 dependency 가 없기 때문에 순서가 없다. 높이에 따라 정해지는게 아니다. 

### 3 Types of Messages
- synchronous message
  - response message 를 받을 때 까지 sender 는 대기한다. 실선 채운 화살표
- asynchronous message
  - 실선 화살표. response message 를 기다리지 않는다. 
  - thread 만 제외하고 대부분은 sync!
- response message
  - synchronous message 와 세트! 생략되기도 한다.
  

### Message Syntax
- return = message (params : paramsType) : returnType
- e.g., initialize, d = getProduct(id), ...

![Validation](/assets/images/message.png){:width="300px" height="300px"}{: .center}
- d1 = aData

### Other Types of Messages
- Found message
  - 밖에서 들어오는 message. system operation.
  - message sender 를 잘 모르거나, 관련이 없다.
- Lost message
  - message receiver 를 잘 모르거나, 관련이 없다.
  - 밖으로 나가는 message. system action.
- Time-consuming message
  - software 에서 잘 사용하지 않는다.
  - message 전송에 시간이 소요된다. 사선으로 선을 그린다. 
  - 수평이면? message 전송에 시간이 소요되지 않는다. (instance time)


### Singleton Objects
- 잘 사용하지 않는다. object 오른쪽 위에 숫자 1 표시해준다.

### Instance Creation
![Validation](/assets/images/instancecreation.png){:width="300px" height="300px"}{: .center}
- class 의 instance 생성하고 싶을 때
- 점선 사용해야 하며, message name 에 "create" 포함해야 한다.
- create 당하는 애가 create 하는 애의 높이보다 낮게 그려야 하고, create 화살표가 lifeline box 의 정중앙에 들어가야 한다.

### Object Destruction
![Validation](/assets/images/objectdestruction.png){:width="300px" height="300px"}{: .center}
- <\<destroy>> stereotyped message 를 사용하고, 삭제되는 지점에서 x 표시를 해준다. 

### Combined Fragments and Operators
- 12 predefined types of operators
  - frames
    - diagram 의 fragments 나 regions 의미한다. guard와 operation 을 갖고 있다.
  - frames 는 nested 된다.
  - 여러 시나리오를 한 장에 압축할 때 유용하다.
  - UML 에서 사용하는 유용한 operator - alt, opt, loop, break 4가지


### alt fragment
- switch 문, alternative sequences
- guards 는 하나의 path 만 선택하기 위해서 사용된다. 
  - square bracket 으로 표현된다.
  - default : true
  - predefined : \[else]

### opt framgent
- optional sequence. else 가 없는 if 문
- 하나의 operand 만 있다.

### loop fragment
- repeatedly-executed sequences, for loop
- (min,max) = (min..max)
- default : (\*) - guard 가 반드시 필요하다.
- loop(8,8) = loop(8), loop = loop(\*) = loop(0..\*)

### break fragment
- exception handling
- guard 가 true 이면, break 문을 수행하고, **break 블록을 감싸는 fragment 실행을 하지 않는다.**
- break block 을 감싸는 block 이 없으면 시나리오를 종료한다.

### seq fragment
- events의 default order
- weak sequencing
  - 다른 lifeline 의 event 는 순서가 상관없다. dependency 가 없으므로.
  - 동일한 ifeline 에 있는 event 는 순서가 정해져야 한다. 
- seq 표시가 있으면, 가능한 traces 가 여러 개 있을 수 있다는 의미이다.

### Interaction Reference
- 한 장에 그릴 때, 하나의 sequence diagram 에 다른 sequence diagram 을 외부 library 처럼 포함할 수 있다. 
- sd AuthenticateUser 이런식으로 만들어놓고, 사용할 때는 ref AuthenticateUser 로 사용한다. 


### Iteration Over a Collection
- collection 의 모든 member 에 대해서 동일한 메시지를 전송한다.


### Waht is the Relationship between Interaction and Class Diagrmas?
- class diagram 은 interaction diagrams 으로부터 interative 하게 생성된다.
- interaction diagrams 그리면, 여러 개의 classes, methods 가 나타난다.
- interaction diagram 용도
  - operation body 채울 때
  - OOD 에서도 사용
    - USE CASE 에서 sequence diagram 그리고, 그걸 가지고 class diagram 을 채운다. (attr, operations, parameters 등)
    - 이걸 한번에 다 해버리는게 waterfall, 지속적으로 조금씩 하는게 iterative, evolutionary, 이걸 archi risky? 하게 하는게 UP 기반의 iterative 하게 진행하는 것이다. 
- dynamic, static view 가 서로 지속적으로 concurrently, iteratively 보완하면서 그려진다.
- 