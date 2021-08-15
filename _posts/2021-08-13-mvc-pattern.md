---
title: "Ch18. MVC Pattern"

categories:
  - softwareengineering

tags:
  - design pattern
---

### MVC Pattern
- Compound pattern
  - 둘 이상의 pattern 이 통합되어 있는 패턴이다.
  - Strategy pattern, Observer pattern, Composite pattern, ...
- Model - the application object
- View - UI
- Controller = UI가 사용자 input 에 어떻게 반응해야 할지 정의


### MVC Structure
![Validation](/assets/images/mvc.png){:width="500px" height="300px"}{: .center}
- view(ui layer)
  - model rendering
  - 사용자 제스쳐를 controller 에게 보내는 역할
  - controller가 view 를 선택하도록 하는 역할
- controller(ui layer)
  - ui layer 의 brain. 사용자 action 발생하면 모델 업데이트
  - 각 기능별로 여러 개의 controllers 두기도 한다.
- model
  - state encapsulation
  - notifies views of changes
- view 는 사용자 인터랙션이 생기면 controller 알리고 controller 는 view 에게 selection 을 보여준다. 그리고 model 의 state 를 변경한다.
- model 은 view 에게 change notification 을 하고, view 가 state query 를 해온다. 
- example scenario
  - 사용자는 view 와 interaction 한다.
  - controller 는 사용자 input 을 handling 한다. callback function.
  - controller 가 model 을 update 한다. state 변경.
  - model 이 view 에게 알려주고, view 는 state query 를 통해 변경된 값을 가져온다.
  - view 는 다시 사용자 interaction 을 기다린다.
  

### Motivations
- 동일한 데이터가 여러 다른 view 에서 표현될 수 있다.
- 동일한 data 가 여러 다른 interaction 에 의해 update 될 수 있다.
- 다양한 view, interaction 을 지원하는 것은 core functionality 를 제공하는 component 에는 영향을 주지 않는다.


### Solution
- responsibilities 를 분리한다.
  - core business model 기능과 presentation, control logic 을 분리한다.
  - 동일한 data를 공유하는 multiple views를 지원한다.
  - 여러 clients 가 implement, test, maintain 할 수 있도록 지원한다.
- model
  - core functionality. data, business rules
- view
  - render the content of a model, model 통해서 data 접근
- controller
  - 사용자 interaction 을 model 이 수행해야 하는 action 으로 변환한다.

### Patterns in MVC
- views를 model 로부터 분리한다.
  - observer pattern.
  - model 은 views 에 대해 아는 것이 없고, 단지 변화가 생겼을 때 noti 만 해준다.
- nested view widgets
  - composite pattern
  - UI component 는 계층적 구성. 일관적 인터페이스 제공한다.
- view 는 gesture 해석을 controller 에게 위임한다.
  - 전략 패턴. 
  - view : context, controller = 특정 concrete strategy 객체들
- 이외에도 Factory, Decorator pattern 사용한다.

### Example : Bouncing Ball Applet
- model
  - ball motion, window size 조정
  - observable (publisher) 


```java
class Model extends Observable{
    // view, controller 관련 코드가 있으면 안된다.
    final int BALL_SIZE = 20;
    void makeOneStep(){ //controller 가 이 메소드 호출한다.
        ...
        setChanged(); // state 변화
        notifyObservers(); // observer notification
    }
}
```

- view
  - ball's state 에 접근 가능해야 한다.
  - ball 그리기.
  - observer. model state display 한다. 
    - save 누르면 그게 save 되었다고 view 가 아는 것이 아니라, model 이 알려주는 것을 받는다. 본인이 생각하는 것이 아님.

```java
class View extends Canvas implements Observer{
    Controller controller;
    Model model;
    int stepNumber = 0;

    public void update(Observable obs, Object args){
      // observable 로부터 noti 받으면 실행되는 부분
        repaint();
    }
}
```

- controller
  - model 이 할 일을 알려준다. view 에게 display refresh 할 타이밍을 알려준다. (여러 view 중 select)
  - ui framework 마다, view, controller 구별하는 것이 많이 다르다. 콜백 method 이후에 controller 이다 등.. 별로 중요하지 않고, view, controller 는 서로 많이 알아도 상관없다.

```java
public class Controller extends JApplet{
    Model model = new Model();
    View view = new View();

    public void init(){
        button.addActionListener(
            ...
            model.makeOneStep(); // 모델 state 변경한다.
        )
        model.addObserver(view); // view subscriber 로 등록한다.
        view.controller = this;
    }
}
```


### Notes
- MVC 패턴 잘 적용되었는지 확인
  - litmus test : UI를 변경한다.
    - MODEL 을 변경하지 않고도 다른 UI에서 잘 동작하나?
- model action 이 빨라야 한다.
  - model 이 실행되는 동안 GUI 는 freezing 상태.
  - multi-threading 사용




