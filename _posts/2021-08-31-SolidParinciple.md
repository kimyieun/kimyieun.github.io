---
title: "Ch03. Solid Principle"

categories:
  - designpattern

tags:
  - uml
---

### Design Smells
- rigidity, 경직성
  - 시스템을 변경하기 어렵다. 하나를 수정하면, 연쇄적으로 다른 것들도 수정해야 한다.
- fragility, 취약성
  - 시스템의 한 부분을 변경하는 것이 완전히 관련 없는 다른 곳에도 영향을 준다.
- immobility, 부동성
  - 시스템의 부분을 다른 시스템에서 재활용할 수 있도록 분리할 수 없다.
- viscosity, 점착성
  - 디자인에 맞는 코드를 추가하기 보다 hack 을 추가하는 게 더 쉽다?
- needless complexity
  - 실제로 당장 필요하지 않고 향후에 도움이 될만한 복잡한 코드가 많다.
- needless repetition
  - 코드가 복붙 형태가 많다.
- opacity, 투명성

### dependency management
- design smells 는 잘못 관리된 dependencies 로 인해 발생한다.
  - 스파게티 코드. 복잡한 커플링이 많이 발생한다.
- OO Language 는 interface, polymorphism 등을 사용해 dependency 를 관리한다.

### SOLID principles
1. Single Responsibility Principle
2. Open-closed Principle
3. Liskov Substitution Principle
4. Dependency Inversion Principle
5. Interface Segregation Principle

### 1. Single Responsibility Principle
- 하나의 클래스는 하나의 변경될 이유 (responsibility)를 가져야 한다.
- responsibility - a contract or obligation of a class
- responsibility 에 따라 다른 classes 로 분리해라.
- cohesion measure
  - 하나의 module 안에 많은 responsibilities 가 얼마나 연관되어 있고 focusing 되어 있는지 측정하는 척도
- avoid needless complexity
  - 불필요하면 도입하지 않는 것이 좋다. 필요할 것 같아서 적용하면 복잡도만 높일 수 있다.


### 2. Open-closed Principle
- sw entities(classes, modules, functions, etc)는 확장에는 open, 변화에는 close 되어야 한다.
- 확장을 하되, 과도한 변경을 일으키면 안된다.
- rigidity, fragility 
  - 새로운 타입이 추가되면 코드 변경이 많이 필요하다. if/else, switch/case 문이 증가하고 발견하기 어렵고 이해하기 어려운 코드가 될 수 있다.
- immobile
  - 함수 재활용하고 싶은데, 그 함수내에 포함된 여러 class types 도 같이 필요해진다.
- abstraction 이 중요하다.
  - 고정되어 있지만, 가능한 behaviors 의 제한이 없다. 
  - concrete class(implementation)가 아닌, abstract class(interface)에 프로그래밍해라.
- 변하기 쉬운 것을 찾고, abstraction 을 사용하여 그 변화로부터 보호해야 한다.
- OCP 를 적용하는 것은 비용이 많이 든다(Polymorphism 을 사용하기 때문에 runtime cost 지불)
- 적절한 abstraction 을 생성하는데에도 시간과 노력이 많이 소요된다.
- abstracction 은 복잡도를 높일 수 있다.
- SRP 와 마찬가지로 모든 케이스에 적용하지 말고 필요하면 적용해라.
    - 처음에는 변하지 않을거라 가정하고 코드를 작성하되, 변화가 발생하면 abstraction 을 적용하는 것이 좋다.


### 3. Liskov Substitution Principle
- subtypes(derived class) 는 base type(base class) 로 대체 가능해야 한다. 
- 상속을 사용할지 말지 체크하기 위해서 사용된다.
- C가 P의 subtype 이면, P를 C로 대체해도 아무 문제가 없어야 한다.
- subtyping vs. implementation inheritance
  - subtyping 
    - is A 관계. interface inheritance. 상속 관계만 설정하고 implementation 을 가져오는 것은 아니다.
  - implementation inheritance
    - code inheritance. type 에는 영향이 없고 implementation 만 상속한다.
- 대부분의 OOP 언어는 상속을 extends 키워드를 사용해서 subtyping 과 implementation inheritance 를 동시에 한다.

### Inheritance
- 상속을 사용하기 전에는 신중해야 한다. 
- implementation 을 재사용하고 싶다면, 상속보다는 object composition 을 사용하는 것이 좋다.


```java
java.util.Date date = new java.util.Date();
int dateValue = date.getDate();

date = new java.sql.Time(10, 10, 10);
dateValue = date.getDate(); // throws
```
- java.util.Date 는 java.sql.Date 와 java.sql.Time 의 상위 type 이다.
  - 상위 타입으로 하위 타입의 instance을 가리킬 수 있으므로, date 의 타입은 base type 이다.
  - 그런데, java.sql.Time 에는 getDate 라는 method 가 정의되어있지 않다. 그래서 에러가 발생한다.
- LSP 위반은 OCP 위반도 불러온다.
  
### 4. Dependency Inversion Principle
- High level modules 은 low level modules 에 의존하면 안된다. abstraction 에 의존해야 한다.
- abstraction 은 detail 에 의존하면 안되고, detail 이 abstraction 에 의존해야 한다.
- inversion?   
  - DIP 는 기존의 structured analysis and design 접근에서의 의존성 역전을 시도한다.

### Inversion of ownership
- dependency 의 역전뿐만 아니라, onwership 의 역전이다.
- 기존에는 서버가 interface 를 소유했지만, DIP 에서는 client 가 interface 를 소유해야 한다.
- client 는 lower-level layer 가 아니라 interface 를 소유해야 한다.

### 5. Interface Segregation Principle
- client 는 사용하지 않는 method 에 의존하면 안된다.
- fat interface
  - 여러 clients 에 대한 기능을 하나의 interface 에 넣으면, 불필요하게 clients 간의 coupling 이 발생한다.
  - **break the interface into cohesive groups.**
