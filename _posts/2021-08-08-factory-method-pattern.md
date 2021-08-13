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
  - object creation 을 factory object 에 **delegation** 한다.


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

public class PizzaStore{ // 새로운 피자 타입이 생겨도 변경되지 않는다.
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

### Simple Factory (not an official pattern)

![Validation](/assets/images/simplefactory.jpg){:width="400px" height="400px"}{: .center}

- Client 인 PizzaStore 는 더이상 new keyword 를 사용하지 않는다. 그러므로, concrete products(cheesePizza, veggiePizza, ...)에 depending 하지 않는다.


### How about Franchising the pizza store?
- 여러 스타일이 생긴다. NY, Chicago, California, etc
- 각 스타일마다 피자 factories 가 생긴다. NYPizzaFactory, ChicagoPizzaFactory, CaliforniaPizzaFactory
- store와 pizza 공급하는 factory 가 각자 논다...


### Factory Method Pattern
- Purpose
  - object 생성하는 method 를 노출하고, subclass 로부터 실제 생성 과정을 컨트롤하도록 한다.
- Use When
  - class 가 생성해야 하는 class 가 무엇인지 결정하지 못했을 때
  - **subclasses 가 무슨 object 가 생성되어야 하는지 구체화할 때**
  - parent classes 가 그들의 subclasses 에게 생성을 defer 하고 싶을 때


### Requiremenet Change
- store 와 pizza creation 을 연결시켜주는 framework 를 만들어보자.
- 해당 framework class 를 상속하는 subclass 가 customize 한다.

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
  - client : NYPizzaStore, ChicagoPizzaStore ...
- 공통적으로 그 product를 사용하는 방법 등은 framework 에서 제공해준다. 어떤 product 를 만들지는 이 framework 를 사용하는 사용자가 결정해야 한다.

```java
public class NYPizzaStore extends Pizza{
    public Pizza createPizza(String type){
      Pizza pizza;
      if(type.equals("cheese")){
        pizza = new NYStyleCheesePizza();
      } else if(type.equals("veggie")){
        pizza = new NYStyleVeggiePizza();
      }
      return pizza;
    }
}

public abstract class Pizza{
  String name;
  String dough;
  String sauce;
  ArrayList toppings = new ArrayList();

  void prepare(){
    ...
  }
  void bake(){
    ...
  }
  ...
}

public class NYStyleCheesePizza extends Pizza{
  public NYStyleCheesePizza(){
    name = "NY Style sauce and cheese Pizza";
    ...
  }
}
```

### Ordering a pizza using the Factory Method
1. PizzaStore instance 생성
2. orderPizza() 호출
3. orderPizza() 가 createPizza() 호출
4. orderPizza() 가 prepare, back, cut and box the pizza


### Test Drive

```java
public class PizzaTestDrive{
  public static void main(String[] args){
    PizzaStore nyStore = new NYPizzaStore();
    Pizza pizza = nyStore.orderPizza("cheese");
  }
}
```

### Bad Design
```java
public class DependentPizzaStore{
  public Pizza createPizza(String style, String type){
    Pizza pizza = null;
    if(style.equals("NY")){
      if(type.equals("cheese")){
        pizza = new NYStyleCheesePizza();
      }
      ....
    }
    pizza.prepare();
    pizza.bake();
    ...
    return pizza;
  }
}
```


### Factory Method Pattern

![Validation](/assets/images/factorymethod.png){:width="400px" height="400px"}{: .center}

- subclass 가 어떤 object 를 생성할지 결정하게 함으로써, object creation encapsulation. 
  - Creator class 는 어떤 concrete product class 가 생성될지 모른다. 
  - concrete product class 는 concrete creator subclass에 의해 생성되고, 사용된다.
  - **subclass 가 어떤 concreteproduct class 를 생성할지 runtime 에 결정한다는 의미가 아니다. compile time 에 결정된다.**
- FactoryMethod 에는 어떤 object 생성할지 모른다.

### Dependency Inversion Principle
- abstraction 에 의존하고, concrete class 에 의존하지 마라.
- high-level components 는 low-level components 에 의존하면 안된다. 둘 다, abstraction에 의존해야 한다.
- factory method 가 DIP 를 따른다.

### Abstract Factory Pattern
- Purpose
  - 생성 호출을 concrete classes 에 위임하는 interface 를 생성한다. 
- Use When
  - object 생성이 system 과 독립적으로 되어야 할 때
  - 시스템이 multiple families of objects을 사용해야 할 때?
  - families of objects (같은 주제를 갖는 여러 개의 objects)가 함께 사용될 때
  - library 가 내부 implementation details 를 노출하지 않고 공개되어야 할 때
  - concrete class 가 clients 와 분리되어야 할 때

### Factory Patterns
- Abstract factory pattern 은 factory method pattern 보다 한 단계 더 abstraction 된 형태이다.
- 메커니즘
  - abstract factory pattern - composition, delegation 사용 (object scope)
  - factory pattern - inheritance (class scope)
- Abstract factory pattern example
  - user interface toolkit - 다양한 look & feel 지원
    - Windows XP, MAC OS X, X Window ,...
  - subclasses 의 appearance, behavior 가 다를 때
    - UI element, scroll bars, windows, buttons, ...

### Abstract Factory 

![Validation](/assets/images/abstractfactory.png){:width="800px" height="600px"}{: .center}

- client 입장에서는 general, abstract 수준의 factory, product 만 안다.
  - **interface 만 사용**한다. 
- abstractFactory
  - abstract product objects 생성을 위한 operations interface 선언
- concreteFactory
  - concrete product objects 생성을 위한 operation implementation
- abstractProduct
  - product object 를 위한 interface 선언
- concreteProduct
  - 해당하는 concrete factory 로부터 생성되는 product object 선언
  - AbstractProduct interface implementation


```java
// not flexible
Button b = new MofiButton();
// Using abstract factory pattern
Button b = guiFactory.createButton();
```

### Making facotires for Ingredients (Abstract Factory + Factory Method)

```java
public interface PizzaIngredientFactory{
  // family of objects
  public Dough createDough();
  public Sauce createSauce();
  public Cheese createCheese(); 
  ...
}

public class NYPizzaIngredientFactory implements PizzaIngredientFactory{
  public Dough createDough(){
    return new ThunCrustDough();
  }
  public Sauce createSauce(){
    return new MarinaraSauce();
  }
  ...
}

public class NYPizzaStore extends PizzaStore{
  protected Pizza createPizza(String item){
    Pizza pizza = null;
    PizzaIngredientFactory ingredientFactory = new NYPizzaIngredientFactory();
    if(item.equals("cheese")){
      pizza = new CheesePizza(ingredientFactory);
    }
    else if(item.equals("veggie")){
      pizza = new VeggiePizza(ingredientFactory);
    }
    ...
    return pizza;
  }
}

public class CheesePizza extends Pizza{
  PizzaIngredientFactory ingredientFactory;
  public CheesePizza(PizzaIngredientFactory ingredientFactory){
    this.ingredientFactory = ingredientFactory;
  }
  void prepare(){
    dough = ingredientFactory.createDough();
    sauce = ingredientFactory.createSauce();
    cheese = ingredientFactory.createCheese();
  }
}
```

### What has changed?
1. PizzaStore instance 생성
2. orderPizza() 호출
3. orderPizza() 가 createPizza() 호출
4. 여기부터 Abstract Factory 사용 
   1. createPizza() 가 ingredientFactory 를 필요로 한다.
      1. Pizza pizza = new CheesePizza(nyIngredientFactory);
   2. prepare() 는 ingredient factory 를 사용해서 ingredient 생성한다.
   3. 끝
5. orderPizza() 가 back, cut, box 를 호출한다.

### When to Use Abstract Factory Pattern?
- creational patterns
  - system 이 products 가 생성,구성,표현되는 방식과 독립적이어야 한다.
  - class 는 그것이 생성할 objects의 class가 무엇인지 알지 못한다.
- specific
  - families of products 를 사용해야 한다.
  - 관련 있는 family of product objects 가 함께 사용되어야 하고, constraint 가 있을 때 사용해야 한다.
- 장점
  - isolate concrete classes
    - factory 가 parts 생성 과정과 책임을 encapsulation 한다.
    - client 를 implementation classes 로부터 분리한다.
  - exchanging product families easy
    - concrete factory 는 그것이 초기화될 때만 나타난다
  - products 간의 consistency 유지 가능
- 단점
  - 새로운 product 추가가 어렵다.
    - not impossible, but costly.
    - interface 가 바뀌므로 concrete factory 도 수정이 필요하다.
    - 반면, factory 추가는 쉽다.


### Related Patterns
- abstract factory 
  - classes 는 보통 factory method 로 implementation 된다.
  - prototype 사용해서 implementation 하기도 한다. (concrete factory 를 여러 개 만들지 않아도 된다)
- easy start => factory method
  - factory method 가 상대적으로 덜 복잡하고, customizable, subclasses proliferate 하다.
  - 추후에 abstract factory, prototype, builder 로 확장된다.
- Summary
  - DIP - abstraction 에 의존하라.
  - Factory method
    - object 생성하는 interface 선언하고, subclasses 가 생성할 class 를 선택하도록 한다.
    - class-scope pattern (inheritance)
    - client : 주어진 framework 상속
  - Abstract Factory 
    - client : 제3자 입장에서 product 생성해준다.
    - 관련 있는 families of objects 를 생성하기 위한 interface 를 제공한다.
    - object-scope pattern (object composition & delegation)


