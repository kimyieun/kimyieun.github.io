---
title: "Ch10. Mediator Pattern"

categories:
  - designpattern

tags:
  - design pattern
---

### Mediator Pattern
- Purpose
  - 서로 다른 objects 간의 interaction/communication 하는 방식을 encapsulation 함으로써 loose coupling 을 지원하기 위해 사용한다.
- Use When
  - objects 간의 communication 이 잘 정의되어 있으나, 복잡한 경우
  - relationship 이 많거나, communication/control 의 공통 point 가 필요할 때

### Battling Clss Complexity
- 관제탑 - control tower
- 비행기들 - colleague
- 비행기들은 서로 모르고 직접적으로 communication 하지 않는다. 정보는 control tower 만 가지고 있다. 
- direct communication? => tightly coupled

![Validation](/assets/images/mediatorpattern1.png){:width="500px" height="300px"}{: .center}
- colleagues 간의 interaction 은 전혀 없다.


### Mediator Pattern

![Validation](/assets/images/mediatorpattern2.jpeg){:width="500px" height="300px"}{: .center}
- concrete mediator 는 concrete colleagues 과 모두 직접 연결되어 있다. 각자 자신의 상태를 돌려주는 API 가 다르기 때문이다. 
  - 각 위젯을 생성하고, control, 상태 변화 통보 등 역할을 수행한다.


- Mediator 에 objects 간의 interconnect encapsulation.
  - communication hub 
  - colleage 간 interaction 을 컨트롤하고 조정하는 책임을 진다.
  - n-1 개의 path (n^2 X)
- class 간의 loose coupling 지원
- colleague 가 많아져도 communication flow 를 이해하기 쉽다. 
- 단점은 mediator 는 재활용이 어렵다. application 에 특화될 수 밖에 없다.
- 장단점이 observer pattern 과 반대이다.
  - observer
    - 참여하는 publisher 가 여러개일 수 있다. flow 가 이해하기 어렵지만, code reuse 가 쉽다.
    - 하나의 topic 에 대해서 notification 하는 것이 publisher 하나이지만, mediator pattern 에서는 notification 하는 것이 다수의 colleagues 이다.


### Example : FontDialog
- dialog 위젯들간의 dependencies 가 있다. 예를 들어, entry field 에 따라서 listbox 의 내용이 달라질 수 있다.
- dialog 도 다양할 수 있는데, dialog boxes 마다 위젯이 같을 수 있어도 dependency 가 다를 수 있기 때문에, widget class 를 재사용하기는 어렵다.
- **widget 은 dialog-specific dependencies 를 반영하기 위해 커스터마이징되어 있다.**
- 그들을 subclassing 화하여 커스터마이징하는 것은 연관된 classes 가 많기 때문에 어려울 수 있다.
- 이 문제를 해결하기 위해서는 collective behavior 를 독립된 mediator object 에 encapsulation 한다.

![Validation](/assets/images/mediatorpattern3.png){:width="500px" height="300px"}{: .center}

### Sequence Diagram of FontDialog

![Validation](/assets/images/mediatorpattern4.png){:width="500px" height="300px"}{: .center}
1. showDialog()
2. 사용자가 listbox 를 수정해서 windgetChanged() 
3. getSelection() 통해 변한 상태값을 쿼리한다.
4. setText() 를 호출해 필요한 조취를 취한다.