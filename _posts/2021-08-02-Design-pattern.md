---
title: "Ch02. Object-Oriented Paradigm and Solid Principles"

categories:
  - designpattern

tags:
  - design pattern
---

### CHAP2 - Object-Oriented Paradigm Review

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