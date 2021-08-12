---
title: "Composite Pattern"

categories:
  - software engineering

tags:
  - design pattern
---

### Composite Pattern
- Purpose
  - a set of nested objects 나 each object 를 동일한 interface 로 취급하는 것을 목표로 한다.
  - object hierarchy 생성을 용이하게 한다.
- Use When
  - object 의 계층 구조 표현이 필요할 때
  - object, compositions of objects 를 동일하게 다루고 싶을 때


### Problems

```java
if(current instanceOf(menuItem)){
// Menu Item (leafnode) 를 다룬다.
}else if(o instanceOf(Menu)){
// Menu (internal node) 를 다룬다.
}
```

### Composite Pattern Diagram

### Component Class

```java
public class MenuComponent{ // Concrete Class - why??
  public void add(MenuComponent menuComponent){
    throw new UnsupportedOperationException();
  }
  ...
  // 모든 operation 에 대해서 exception 발생 (동일한 interface 제공을 위한 class 이므로 MenuComponent 를 만드는건 의미가 없다)
}
```

