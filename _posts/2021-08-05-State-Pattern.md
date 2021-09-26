---
title: "Ch09. State Pattern"

categories:
  - designpattern

tags:
  - design pattern
---

### State Pattern
- Purpose
  - object circumstances과 object 행동을 연결하여, object 의 내부 상태에 따라 object 가 행동하도록 하기 위해 사용한다.
- Use When
  - object behavior 가 그 object의 상태로부터 결정될 때
  - 복잡한 조건으로 인해 object behavior 와 state 가 연결되면
  - state 의 변화가 명확해야 할 때

![Validation](/assets/images/statepattern.png){:width="500px" height="300px"}{: .center}



### Gumball Machine Example

```java
public class GumballMachine {
  final static int SOLD_OUT = 0;
  final static int NO_QUATER = 1;
  final static int HAS_QUATER = 2;
  final static int SOLD = 3;

  int state = SOLD_OUT; // initial state
  int count = 0; // gumball count

  public void insertQuarter(){
    if(state == HAS_QUATER){
      ...
    }
    else if(state == SOLD_OUT){
      ...
    }
    else if(){...}
  }
}
```

- 아래는 위 GumballMachine class 테스트 코드이다.

```java
public class GumballMachineTestDrive{
  public static void main(String[] args){
    GumballMachine gumballMachine = new GumballMachine(5);

    gumballMachine.insertQuarter();
    gumballMachine.turnCrank(); 

    ...
  }
}
```

- 이렇게 하면 된다! 근데 문제가 발생했다. 요구사항이 변경되었다.
- 1/10의 확률로 gumball 을 2개 주도록 변경해주세요, ....
- 이렇게 계속 요구사항이 변화하면?
  - 새로운 state 를 추가해야 한다.
  - 코드 상의 변경이 많아진다.

### New Design
- **Encapsulate what varies**
  - 각 state 에 대해 각자의 class 를 만들고, 각 state 가 actions 을 구현하도록 변경해라.
  - gumball machine 은 state object 에 delegation 하는 역할만 수행해라.
- **Favor composition over inheritance** 
- 위와 같이 if,else 조건문은 지우고, state object 에 delegation 만 해주자.

```java
public interface State{
  public void insertQuarter();
  public void ejectQuarter();
  public void turnCrank();
  public void dispense();
}

public class NoQuarterState implements State{
  GumballMachine gumballMachine;

  public NoQuarterState(GumballMachine gumballMachine){
    this.gumballMachine = gumballMachine;
  }

  public void insertQuarter(){
    System.out.println("you inserted a quarter");
    gumballMachine.setState(gumballMachine.getHasQuarterState()); 
    // noquarterstate -> hasquarterstate 로 변경
  }

  public void ejectQuarter(){
    System.out.println("you haven't inserted a quarter");
  } 
}
```

- 위와 같이 concreate state object 에서 적합한 action 을 취하고 gumballmachine state 를 세팅한다. 그러면 GumballMachine class 도 변경해보자.

```java
public class GumballMachine{
  State soldOutState;
  State noQuarterState;
  State hasQuarterState;
  State soldState;

  State state = soldOutState; // initial state
  int count = 0;

  public GumballMachine(int numberGumballs){
    soldOutState = new SoldOutState(this);
    noQuarterState = new NoQuarterState(this);
    hasQuarterState = new HasQuarterState(this);
    soldState = new SoldState(this);

    this.count = numberGumballs;
    if(numberGumballs > 0) state = noQuarterState;
  }

  public void insertQuarter(){
    state.insertQuarter(); // 현재 state 에게 action 을 위임한다.
  }

  void setState(State state){
    this.state = state; // state 변경할 수 있도록 method 로 제공
  }
}
```


- 변경 전 코드와 구조적으로는 많이 달라졌지만, 동일하게 동작한다.
- 무엇이 달라졌나?
  - 각 state 의 행동을 본인의 class 에 localization 
  - conditional statements 제거해서 유지보수가 용이하도록 지원
  - 각 state 에서 close modification, open to extension (새로운 state class 추가는 쉬워지고 기존 state 는 변경이 필요없다. 각 state 별로 encapsulation)
  - 읽고 이해하기 쉬워짐


### State Pattern
- 객체의 behavior 가 그 객체 내부의 state 변화에 따라 runtime 에 함께 변화해야할 때 사용하는 패턴
- operation 이 굉장히 많고, object state 에 따라 조건문이 많이 필요할 때, state pattern 사용하면 각 조건문의 branch 별로 별개의 class 를 생성한다.
- 장점?
  - state 와 관련된 behavior 가 하나의 object 내에 모두 포함된다 (high cohesion)
  - inconsistent state 를 피할 수 있다.
- 단점?
  - object 개수가 state 개수만큼 존재

### State vs. Strategy
- 패턴의 차이는 결과물이 아닌 해결하고자 하는 문제(목적)으로부터 온다.
- 두 패턴은 굉장히 유사하다. 결과물만 보면 거의 동일하다. 차이는 intent!
- State Pattern
  - **Encapsulate state-based behavior**
    - context의 행동은 시간이 지나면서 바뀐다.
    - context 에 많은 conditionals 를 두는 것을 대체하기 위해 사용한다.
  - 외부의 자극에 따라 state 가 유동적으로 변경된다.
- Strategy Pattern
  - algorithm 전체를 encapsulation 하고자 한다.
    - **Encapsulate interchangeable behavior**
    - context 에 적합한 하나의 전략 객체가 있다. 
    - subclasses(inheritance) 를 대체하기 위해 사용한다.
  - algorithm 선택이 꽤 stable 하다. runtime 에 변경 가능하지만, 보통 하나의 전략을 쭉 사용한다.
- 둘 다, **composition with delegation** 을 사용한다.

### Implementation Issues
- state transitions 을 누가 정의하나?
  - Option1. Context class
    - 간단한 상황에서는 사용 가능
  - Option2. ConcreteState classes
    - 우리 예제. 일반적으로 더 flexible, 그러나 ConcreteState classes 간의 implementation dependency 가 생긴다. getter method 를 사용하는 것을 권장한다.

- ConcreteState object 는 언제 만드나?
  - Option1. 필요할 때 만든다.
  - Option2. 한번에 모두 만들고, Context object 가 그들에 대한 reference 를 유지한다.

### WinnerState 를 추가하면?
-  WinnerState class 를 새로 생성한다.
-  기존 state 중 WinnerState 생성으로 인해 영향을 받는 애만 처리해주고 나머지 코드는 closed to the modification.

### Related Patterns
- Strategy Pattern
- Template Method
  - algorithm 을 어떤 단계로 implementation 할지 결정하기 위해 subclasses 사용한다.
