---
title: "Ch2. Object-Oriented Paradigm and Solid Principles"

categories:
  - designpattern

tags:
  - design pattern
---

### CHAP2 - Object-Oriented Paradigm Review

### Abstract data type (ADT)
- **encapsulation** of data and related operations into a **single syntactic unit**
- single syntactic unit - 하나의 문법적 요소로 프로그램을 관리하고 수정하기 쉽다.
- encapsulation - 높은 reliability
- class = ADT + inheritance + polymorphism

### Inheritance
- when calss Y inherits from class X
  - **Y is a X**
  - Y의 모든 instance 는 X의 instance 이다.
  - **X type instance 가 요청되는 모든 곳에서 Y type instance 를 사용할 수 있다.**

### Polymorphism
- 동일한 이름의 operation 이 여러 class 마다 다르게 동작하는 특성
- if-else, switch 코드를 많이 줄일 수 있다.
- 2가지 타입
  - runtime polymorphism (dynamic polymorphism) - method overriding
    - 자식 class 가 부모 class 에서 선언된 것과 동일한 method 를 갖는다. runtime 전에는 부모의 method 인지 자식의 method 인지 알 수 없다.
  - compile time polymorphism (static polymorphism) - method overloading
    - 하나의 class 내에서 동일한 이름이지만 다른 signature 를 갖는다.
- Run-time polymorphism
  - class 내에 정의될 수 있는 polymorphic variable 는 그 class의 object 뿐만 아니라, 그 class 의 자식 class의 objects 도 가리킬 수 있다.

```java
// method overriding
public class X{
  public void methodA(){
    ...
  }
}

public class Y extends X{
  public void methodA(){
    ...
  }
}

public class Z{
  public static void main(String args[]){
    X obj1 = new X();
    X obj2 = new Y(); // 상위 type 으로 하위 type instance 를 가리킬 수 있다.
    obj1.methodA(); // X 클래스의 methodA 가 불림
    obj2.methodA(); // Y 클래스의 methodA 가 불림
  }
}
```

### Abstarct class and abstract method (in java)
- abstract method - definition 을 포함하지 않는다. (body 구현 X, protocol 만 정의한다.)
- abstract class
  - 적어도 하나의 abstract method 를 포함하는 class 는 abstract class 다. abstract class 가 적어도 하나의 abstract method 를 포함해야 하는 것은 아니다.
  - *Italic*체로 작성한다.
- abstract class 는 instance 를 생성할 수 없다. (new 이후에 나올 수 없음)
- abstact class 타입의 변수는 자식 objects 를 가리키는데 사용할 수 있다. stable 하고 변하지 않으므로 선호한다.


#### Polymorphism + abstract class
```java
abstract class Animal{ // abstract method 를 하나 이상 가지고 있으므로, abstract class 가 된다. 
  protected String name;
  abstract public void say();
}

class Cat extends Animal{
  private void meow() {...}
  public void say(){meow();}
}

abstract class Canine extends Animal{
  protected bool likeBones;
  // Animal 의 say signature 도 상속받았는데, override 하지 않았으니, abstract method 를 가지게 되고, abstract class 로 선언해야 한다. 
}

class Dog extends Canine{
  private void bark() {...}
  public void say() {bark();}
}


Animal c1 = new Animal(); // compile error - abstract class 는 instance 생성하지 못한다.
Canine c2 = new Cat(); // compile error
Animal c3 = new Dog(); // abstract class 는 type 으로는 사용 가능하다.
```


### 그러면 어떤 method 를 실행할지 어떻게 정하지?
- 현재 class 에 concrete method 가 있으면 그것을 실행한다. 없으면 계속 부모까지 올라가면서 있는지 본다.
- 없으면 에러 발생한다. 근데, 그럴 수는 없다. 없는 method 면 compile error 가 났을거고, 있는 method 인데 body 가 비어있으면 모든 class 가 다 abstract class 라는 의미이다.
- 그러면 아무도 instance 를 만들 수 없기 때문에 가정이 틀리게 된다.

### Interface
- 모든 method 가 abstract 이다. (java 8 이후로는 default method 가 있다고 한다.)
- instance variable 을 포함할 수 없으나, public static final viarables 는 가질 수 있다.
- abstract class 보다 더 abstract 하다. implements 키워드를 사용한다.
- **interfaces 도 type 이다.**
  - abstract class 처럼 자식 class의 objects 를 가리키는 type 으로 사용할 수 있다.
- vs abstract classes
  - 아무 제한이 없으면 interface 를 더 선호해라. 더 abstract 하고, 더 많은 기회를 남기니까?
  - abstract class 선호하는 경우
    - implementation 을 적절하게 제공할 수 있다.
    - 명확하게 subclass-superclass 관계가 is a 일 때 사용해야 한다. 
  - method 의 implementation 을 하나도 할 수 없거나, 다른 class 를 상속하고 싶거나(java 에서는 multi inheritance 가 불가능하므로), interface 가 class 의 모든 부분을 대표하는 것이 아니라, interface 의 method 가 class 의 작은 부분에만 해당될 때 interface 를 사용한다.

### Class Relationships
- A -> B
- source -> target : target 이 변경되면 source 에 영향을 준다. target 은 source 의 존재 및 변경사항을 알지 못한다.

#### Polymorphism with interface

```java
abstract class Animal{
  protected String name;
  abstract public void say();
}

class Cat extends Animal implements Sayable{
  private void meow() {...}
  public void say(){meow();}
}

abstract class Canine extends Animal{
    protected bool likeBones;
}

class Dog extends Canine implements Sayable{
  private void bark() {...}
  public void say() {bark();}
}

class Robot implements Sayable{
  private void printOut() {...}
  public void say() {printOut();}
}

interface Sayable{
  public void say();
}

Animal aref = new Dog();
Sayable sref = new Cat();
Canine cref = new Dog();
```

### Strategy Pattern

```java
public abstract class Duck{
  QuackBehavior quackBehavior;
  FlyBehavior flyBehavior;

  public Duck{
  }

  public abstract void display();

  public void performFly(){
    flyBehavior.fly();
  }

  public void performQuack(){
    quackBehavior.quack();
  }

  public void swim(){
    System.out.println('all ducks float');
  }

  public void setFlyBehavior(flyBehavior fb){
    flyBehavior = fb;
  }

  public void setQuackBehavior(quackBehavior qb){
    quackBehavior = qb;
  }
}
```


```java
public interface FlyBehavior{
  public void fly();
}

public class FlyWithWings implements FlyBehavior{
  public void fly(){
    System.out.println('i am flying');
  }
}

public class FlyNoWay implements FlyBehavior{
  public void fly(){
    System.out.println('i can not fly');
  }
}

public class FlyRocketPowered implements FlyBehavior{
  public void fly(){
    System.out.println('i am flying with a rocket');
  }
}

/////////////////////////////////////////////////////

public class MallarDuck extends Duck{
  public MallarDuck(){
    quackBehavior = new Quack();
    flyBehavior = new FlyWithWings();
  }

  public void display(){
    System.out.println('i am a real mallard duck');
  }
}

public class ModelDuck extends Duck{
  public ModelDuck(){
    flyBehavior = new FlyNoWay();
    quackBehavior = new Quack();
  }
}
//test
public class MiniDuckSimulator{
  public static void main(String[] args){
    Duck mallard = new MallarDuck();
    mallard.performFly(); // i am flying 
    
    Duck model = new ModelDuck();
    model.performFly(); // i can not fly 
    model.setFlyBehavior(new FlyRocketPowered());
    model.performFly(); // i am flying with a rocket
  }
}
```

#### sorting example

```java
void methodOfClient(ArrayList al){
  SortedList sl = new SortedList(al);
  sl.setSortAlgo(new QuickSort());
  sl.sort();
}
```

### Observer Pattern


```java
// preparing interfaces
public interface Subject{
  public void registerObserver(Observer o);
  public void removeObserver(Observer o);
  public void notifyObservers(); 
}

public interface Observer{
  public void update(float temp, float humidity, float pressure);
}

public interface DisplayElement{
  public void display();
}

// implementing weatherdata
public class WeatherData implements Subject{
  private ArrayList observers; // observers 관리하는 리스트
  private float temperature;
  private float humidity;
  private float pressure;

  public WeatherData(){
    observers = new ArrayList();
  }

  public void registerObserver(Observer o){
    observers.add(o);
  }

  public void removeObserver(Observer o){
    int i = observers.indexOf(o);
    if(i>=0) observers.remove(i);
  }

  public void notifyObservers(){
    for(int i=0;i<observers.size(); i++){
      Observer observer = (Observer)observers.get(i);
      observer.update(temperature, humidity, pressure);
    }
  }

  public void measurementsChanged(){
    notifyObservers();
  }

  public void setMeasurements(float temp, float humidity, float pressure){
    this.temperature = temp;
    this.humidiity = humidity;
    this.pressure = pressure;
    measurementsChanged();
  }
}

public class CurrentConditionDisplay implements Observer, DisplayElement{
 private float temperature;
 private float humidity;
 private Subject weatherData;

 public CurrentConditionDisplay(Subject weatherData){
   this.weatherData = weatherData;
   weatherData.registerObserver(this);
 }

 public void update(float temperature, float humidity, float pressure){
   this.temperature = temperature;
   this.humidity = humidity;
   display();
 }

 public void display(){
   System.out.println("...");
 }
}

public class WeatherStation{
  public static void main(String[] args){
    WeatherData weatherData = new WeatherData();
    CurrentConditionDisplay currentdisplay = new CurrentConditionDisplay(weatherData);
    weatherData.setMeasurements(80, 40, 30.4f);
  }
}
```


### Java's Official Observable Class

```java
setChanged(){
  changed = true;
}

notifyObservers(Object arg){
  if(changed){
    for every observer on the list{
      call update(this, arg);
    }
    changed = false;
  }
}

notifyObservers(){
  notifyObservers(null); 
}
```

### Observable, Observer 를 쓰면 아래와 같이 코드가 간단해진다.

```java
import java.util.Observable;
import java.util.Observer;

public class WeatherData extends Observable{
  // private ArrayList observers; // observers 관리하는 리스트
  private float temperature;
  private float humidity;
  private float pressure;

  public WeatherData(){
    // observers = new ArrayList();
  }

  // public void registerObserver(Observer o){
  //   observers.add(o);
  // }

  // public void removeObserver(Observer o){
  //   int i = observers.indexOf(o);
  //   if(i>=0) observers.remove(i);
  // }

  // public void notifyObservers(){
  //   for(int i=0;i<observers.size(); i++){
  //     Observer observer = (Observer)observers.get(i);
  //     observer.update(temperature, humidity, pressure);
  //   }
  // }

  public void measurementsChanged(){
    setChanged();
    notifyObservers();
  }

  public void setMeasurements(float temp, float humidity, float pressure){
    this.temperature = temp;
    this.humidiity = humidity;
    this.pressure = pressure;
    measurementsChanged();
  }

  public float getTemperature(){
    return temperature;
  }

  public float getHumidity(){
    return humidity;
  }

  public float getPressure(){
    return pressure;
  }
}


public class CurrentConditionDisplay implements Observer, DisplayElement{
 private float temperature;
 private float humidity;
 private Observable weatherData;

 public CurrentConditionDisplay(Observable weatherData){
   this.weatherData = weatherData;
   weatherData.addObserver(this);
 }

 public void update(Observable obs, Object arg){
   if(obs instanceof WeatherData){
    WeatherData weatherData = (WeatherData)obs;
    this.temperature = weatherData.getTemperature();
    this.humidity = weatherData.getHumidity();
    display();
   }
 }

 public void display(){
   System.out.println("...");
 }
}

```


### Template Method Pattern

```java
public abstract class caffeinBeverage{
  final void prepareRecipe(){
    boilwater();
    brew();
    pourInCup();
    addCondiments();
  }

  abstract void brew();
  abstract void addCondiments();

  public void boilwater(){
    System.out.println("boiling water");
  }

  public void pourInCup(){
    System.out.println("pouring into cup");
  }
}
```