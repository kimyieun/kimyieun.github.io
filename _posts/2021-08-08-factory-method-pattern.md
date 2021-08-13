---
title: "Factory Method Pattern"

categories:
  - softwareengineering

tags:
  - design pattern
---

## Factory Method Pattern / Abstract Factory Pattern

### Creating Objects
- concrete class 가 아닌 interface 에 코딩해라.

```java
Duck duck;
if(a) duck = new MallardDuck();
else if(b) duck = new DecoyDuck();
else duck = new RubberDuck();
```
- condition 에 따라 여러 종류의 duck 이 필요하다면?
  - new operator 는 항상 concrete class 가 필요하다.
  - 그러면 client code 가 concrete class 에 의존하게 되므로 좋은 디자인이 아니다.
  - object 생성 패턴으로 바꿔보자.


### Factory Pattern
- Creational patterns
  - new operator 를 사용하지 않고 새로운 object 생성 가능
  - 그러므로, client code 를 변경하지 않고도 여러 종류의 object instantiation 가능
  - examples : factory method, abstract factory, singleton, builder, prototype
- Factory pattern
  - instantiation 되는 object 를 결정하기 위해 **상속**을 사용한다.
- Abstract Factory
  - object creation 을 factory object 에 delegation 한다.


### code

```java
pizza orderPizza(String type){
    Pizza pizza;
    //// #1
    if(type.equals("cheese")) pizza = new CheesePizza();
    else if(type.equals("greek")) pizza = new GreekPizza();
    else if(type.equals("pepperoni")) pizza = new PepperoniPizza();

    //// #2
    pizza.prepare();
    pizza.bake();
    pizza.cut();
    pizza.box();
    return pizza;
}
```

- 위 코드에서 pizza type 이 변경되었다면?
- #2는 피자 타입이 변경되어도, 공통인 작업이다.
- 우선, 변경되는 부분과 변경되지 않는 부분은 구분하고 분리시켜야 한다.


```java
public class SimplePizzaFactory{
    public Pizza createPizza(String type){
        Pizza pizza;
        if(type.equals("cheese")) pizza = new CheesePizza();
        else if(type.equals("greek")) pizza = new GreekPizza();
        else if(type.equals("pepperoni")) pizza = new PepperoniPizza();
        return pizza;
    }
}

public class PizzaStore{
    SimplePizzaFactory factory;
    public PizzaStore(SimplePizzaFactory factory){
        this.factory = factory;
    }
    public Pizza orderPizza(String type){
        Pizza pizza;
        pizza = factory.createPizza(type);
        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();
        return pizza;
    }
}
```

### How about Franchising the pizza store?
- 여러 스타일이 생긴다. NY, Chicago, California, etc
- 여러 피자 factories 가 생긴다. NYPizzaFactory, ChicagoPizzaFactory, CaliforniaPizzaFactory
- store와 pizza 공급하는 factory 가 각자 논다...


### Factory Method Pattern
- purpose
  - object 생성하는 method 를 노출하고, 각 subclass 로부터 실제 생성 과정을 컨트롤하도록 한다.
- use when
  - class 가 생성해야 하는 class 가 무엇인지 결정하지 못했을 때
  - subclasses 가 무슨 object 가 생성되어야 하는지 구체화할 때
  - parent classes 가 그들의 subclasses 에게 생성을 defer 하고 싶을 때


### Requiremenet Change
- store 와 pizza creation 을 연결시켜주는 framework 를 만들어보자.

```java
public abstact class PizzaStore{
    public Pizza orderPizza(String type){ // 공통 작업
        Pizza pizza;
        pizza = createPizza(type); 
        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();
        return pizza;
    }
    protected abstract Pizza createPizza(String type);
    //  subclasses 에서 override 해서 사용하게 함
}
```

### Factory Method
- 위 코드에서 createPizza() factory method 를 호출하여, pizza 를 만든다.
- client - framework 를 subclassing 한다. 즉, 주어진 class 의 subclasses 를 생성하여 특정 product 를 생성하는 method 를 override 한다.
- 공통적으로 그 product를 사용하는 방법 등은 framework 에서 제공해준다. 어떤 product 를 만들지는 이 framework 를 사용하는 사용자가 결정해야 한다.

```java
public class NYPizzaStore extends Pizza{
    public Pizza createPizza()
}
```
