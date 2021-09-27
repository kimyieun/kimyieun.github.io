---
title: "Ch5. Strategy Pattern"

categories:
  - designpattern

tags:
  - designpattern
---

### Strategy Pattern
- 구체적인 행동을 수행하는 algorithms 를 encapsulation 해서 swapping 하기 쉽도록 돕는다.
- use when
  - 여러 classes 간의 차이가 behavior 만 있을 때
  - algorithm 의 여러 버전이 필요할 때
  - class 의 행동이 runtime 에 정해질 때
  - conditional statements 가 복잡하고 유지보수가 어려울 때

### inheritance
- 변종이 등장할 때마다 각 class 에서 override 해서 구현해야 한다. 
- 너무 귀찮고 많다..


### Interface
- interface 는 implementation 이 없기 때문에 copy & paste 가 많이 필요하다.
- maintain 이 어렵다.

### encapsulate what varies
- 변하는 것과 변하지 않는 것을 분리한다.
- implementation 이 아닌 interface 에 프로그래밍 해야 한다.
  - polymorphism
  - object composition + delegation

### setting behavior dynamically
- set method 를 통해서 runtime 에도 behavior 변경 가능하다.


### Favor composition over inheritance
- polymorphic behavior 와 code reuse 가능하다.
- delegation - method call 을 composed object 에 전달한다.
- class inheritance
  - subclass 의 implementation 이 부모 implementation 에 의해 정의된다.
  - 부모 class implementation 이 subclass 에 보인다.
  - white-box reuse
  - compile time 에 되고, 사용하기 쉽지만, subclass 가 parent 에 depend 하게 되고 상속된 implementation 은 runtime 에 변경되지 않는다.
- object composition
  - object 가 복잡한 기능을 수행할 수 있다. polymorphism 에 의해 runtime 에 행동을 변경할 수 있다. black-box reuse.
  - dependency 가 적지만 이해하기 어렵다.


### summary
- encapsulate what varies
- program to interface, not implementation
- favor composition over inheritance
- 