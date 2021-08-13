---
title: "Builder Pattern"

categories:
  - softwareengineering

tags:
  - design pattern
---

### Builder Pattern
- purpose
  - 쉽게 변경 가능한 알고리즘을 기반으로 dynamically object 생성 지원 
- use when
  - creation 과정에서 runtime control 이 필요할 때
  - 다양한 형태의 creation 알고리즘이 필요할 때
  - object 생성 알고리즘이 system 과 decoupled 되어야 할 때
  - core code 변경하지 않고 새로운 creation 기능의 추가가 필요할 때 


### Builder - Structure
- responsibility 가 다르다 - director / concrete builder
- director
  - final product 를 위해 어떤 부분이 필요한지 알고 있다.
  - parts 를 조립하는 지시를 내리는 역할
- concrete builder
  - 각 part 를 어떻게 만들어내고, final product 에 어떻게 더하는지 알고 있다.
- final product 는 director 또는 abstract Builder 에 보관 가능하다.
- 전략 패턴과 유사하여, 전략 패턴을 object creation 에 적용한 패턴이라고도 한다.


### Builder - Collaborations
- client 
  - director 생성. product 생성 요청한다. 각 part 에 대해 아는 정보가 없고, product 만 알고 있다.
  - getProduct() 를 통해 최종 product 만 get 한다.
- director
  - product 를 위해 어떤 builder 가 필요한지 아니까 만들어달라고 지시한다. 
  - buildPartA(), buildPartB(), buildPartC()

### Builder - Participants
- client
  - product 생성에 필요한 director, concrete builder 를 선택한다. director 에게 어떤 concrete builder 와 일할지 알려준다.
  - concrete builder 에게 final product return 을 요구한다. director 한테 할 수도 있을 듯?
- director
  - product 생성을 위한 steps 를 알고 있다.
  - 어떻게 각 step 이 수행되어야 하는지는 모른다.
- builder
  - product object 의 parts 생성을 위한 abstract interface 역할
- concrete builder
  - builder interface 를 implementation 하여 parts 를 생성한다.
  - 생성된 representation 을 정의하고 계속 관리한다.
  - product 를 위한 interface 를 제공한다.
- product
  - complex object under construction

### Example : Building Airplanes
- director : aerospaceEngineer
- abstract builder : airplaneBuilder
- airplane : builder
- concrete builders : cropduster, flighterjet, glider

### Director

```java
public class AerospaceEngineer{
    private AirplaneBuilder airplaneBuilder;
    public void setAirplaneBuilder(AirplaneBuilder ab){
        airplaneBuilder = ab;
    }

    public Airplane getAirplane(){ // get final airplane product
        return airplaneBuilder.getAirplane();
    }

    public void constructAirplane(){
        airplaneBuilder.createNewAirplane();
        airplaneBuilder.buildWings();
        airplaneBuilder.buildPowerplant();
        airplaneBuilder.buildAvionics();
        airplaneBuilder.buildSeats();
    }
}
```

### Abstract Builder

```java
public abstract class AirplaneBuilder{
    protected Airplane airplane;
    protected String customer;
    protected String type;

    public Airplane getAirplane(){
        return airplane;
    }

    public void createNewAirplane(){
        airplane = new Airplane(customer, type);
    }

    public abstract void buildWings();
    public abstract void buildPowerplant();
    public abstract void buildAvionics();
    public abstract void buildSeats();
}
```

### Product

```java
public class Airplane{
    private String type;
    private float wingspan;
    private String powerplant;
    private int crewSeats;
    private int passengerSeat;
    private String avionics;
    private String customer;

    Airplane(String customer, String type){
        this.customer = customer;
        this.type = type;
    }

    public void setWingSpan(float w) this.wingspan = w;
    public void setPowerplant(String p) this.powerplant = p;
    public void setAvionics(String a) this.avionics = a;
    ...
    public void getCustomer(){ return customer; }
}
```

### Concrete Builder 1

```java
public class CropDuster extends AirplaneBuilder{
    CropDuster(String customer){
        super.customer = customer;
        super.type = "Crop duster";
    }
    //overide methods
    public void buildWings(){
        airplane.setWingSpan(9f);
    }
    public void buildPowerPlant(){
        airplane.setPowerplant("single piston");
    }
    ...
}
```

### Client Application

```java
public class BuilderExample{
    public static void main(String[] args){
        AerospaceEngineer aero = new AerospaceEngineer();
        AirplaneBuilder crop = new CropDuster("farmer joe");
        aero.setAirplaneBuilder(crop);
        aero.constrctAirplane();
        Airplane completedCropDuster = aero.getAirplane();
    }
}
```

### Real-life Example



### When a Builder Should't Be Used
- interface 가 stable 하지 않으면 이 패턴 사용의 장점이 없다.
  - interface 가 바뀌면 controller, abstract base class, 그들의 objects 에 영향을 준다.
  - base class 를 변경하기 위해 새로운 method 가 필요하고, concrete classes 는 새로운 method 를 overriding 해야 한다.
  - 구체적인 method interface 변화는 모든 concrete classes 가 오래된 method 도 지원하도록 요구한다.


### Related Patterns and Summary
- builder 가 만드는 것은 composite (complex object 의 representation)
- abstract factory
  - builder 는 object 를 step-by-step 만들고, 결과가 제일 마지막에 요청된다.
  - abstract factory 는 요청된 object 를 즉각 return하고, abstract builder 사용하지 않는다. application 이 factory method 를 직접적으로 호출한다.
  - builder 는 product 1개고, parts 여러 개인 반면에 abstract factory 는 product 가 여러 개이다.

