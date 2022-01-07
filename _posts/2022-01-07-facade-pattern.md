---
title: "Facade Pattern"

categories:
  - designpattern

tags:
  - designpattern
---

### Facade Pattern
- Purpose
  - 시스템 내의 a set of interfaces 에 대해 a single interface 를 제공하기 위한 목적
- Use When
  - 복잡한 시스템에 접근하기 위해서 단순한 인터페이스가 필요할 때
  - 시스템 implementation 과 client 간의 dependency 가 많을 때-facade 를 사용해서 줄여보자!
  - 시스템과 서브시스템이 layered 되어야 할 때 (인터페이스 계층을 하나 더 둔다.)

### Motivation
- 기존의 OO Design 에서는 system 을 subsystem 으로 나누고, subsystem 을 class 로 나눈다.
- 문제점? - 작은 class 가 많아진다!
- 새롭게 진입한 사람이 어디서부터 시작할지 모른다.
- facade object 가 하나의 단순한 인터페이스를 제공함으로써, subsystem 을 쉽게 사용하도록 도움을 준다.

### Benefits
- client 에게 subsystem implementation 을 숨긴다.
- client 와 subsystem 간의 weak coupling 을 제공한다.
- 주의
  - 숙련된 사용자가 underlying class 에 접근하는 것을 막고자 하는 것이 아니다.
  - facade 는 새로운 기능을 추가하는 것이 아니라, 단지 인터페이스를 단순화하는 역할이다.

### Example : Home Sweet Home Theater
- 집에서 영화관처럼 영화를 보고 싶다!
- 영화보려면?
  1. 팝콘 기계를 킨다.
  2. 팝콘 popping 한다.
  3. 불을 어둡게 한다.
  4. 스크린을 내린다.
  5. 프로젝터를 켠다.
  6. ...

- 이 많은 과정을 하나의 클래스에 담으면, 너무 많은 클래스에 dependency 가 생긴다!

```java
popper.on();
popper.pop();
light.dim(10);
screen.down();
project.on();
//... 객체가 더 많을 수 있지만, 예시로 3-4개만 들었다.
```

### HomeTheaterFacade

```java
public class HomeTheaterFacade{
    PopcornPopper popper;
    Screen screen;
    TheaterLight light;
    //...

    public HomeTheaterFacade(PopcornPopper popper, Screen screen, TheaterLight light){
        this.popper = popper;
        this.screen = screen;
        this.light = light;
        //...
    }

    public void watchMovie(String movie){
        popper.on();
        popper.pop();
        light.dim(10);
        screen.down();
        project.on(); 
        //...
    }

    public void endMovie(){
        popper.off();
        light.on();
        screen.up();
        project.off(); 
        //...
    }
}

public class HomeTheaterTestDrive{
    public static void main(String[] args){
        HomeTheaterFacade homeTheater = new HomeTheaterFacade(amp, tuner, dvd, cd, projector, popper, screen, light);

        homeTheater.watchMovie("Lost Ark");
        homeTheater.endMovie();
    }
}
```

### Facade Pattern
- 한 단계 높은 인터페이스를 제공함으로써, subsystem 을 사용하기 쉽도록 한다.
- **Least Knowledge Principle**
  - 직접적인 친구하고만 말하는 것이 좋다. 나머지는 가능한한 서로 모르는게 좋다!
  - dependency 를 가지는 관계가 적어야 한다는 의미
  - immediate friends 와만 말해라
    - a method m of an object o
      - o
      - m's parameters
      - m 내에서 생성하는 objects
      - o's direct component objects
      - global variable
    - a.b.method() 와 같은 관계 권장하지 않는다. .은 한번만 사용해라. 
- Related Patterns
  - Mediator
    - mediator 의 colleagues 는 mediator 를 알고 있다.
    - 차이점은 mediator는 양방향의 interaction 을 지원해주지만, facade 에서는 단방향의 인터랙션을 지원한다. subsystem 은 facade 에 대한 지식이 없다.
    - 공통점은 둘다 기능을 추가하는 것은 아니라는 점이다.