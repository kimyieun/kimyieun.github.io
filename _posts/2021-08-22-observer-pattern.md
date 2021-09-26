---
title: "Ch06. Observer Pattern"

categories:
  - designpattern

tags:
  - designpattern
---

### Observer Pattern
- pub/sub model
- purpose
  - 여러 objects 가 다른 objects 에서의 상태 변화를 notification 받기 위해 사용한다.
- use when
  - communcation 을 위해 loose coupling 이 필요할 때
  - state 변화가 다른 objects 에 영향을 줄 때
  - broadcasting 해야할 때

### The Application Overview
- weather station 에는 humidity, temperature, pressure sensor 가 있다.
- 여기서 data 를 추출해서 display 해주는 시스템을 디자인한다.
- display 는 weather data 를 사용한다.
  - current conditions, statistics, forecast display 에서 사용한다.
  - 위 displays 는 weatherdata 가 새로 측정될 때마다 업데이트가 필요하다.
- 추가로 확장 가능한 시스템이 필요하다.
  - 다른 개발자가 새로운 custom display elements 를 추가할 수 있고, 사용자 입장에서는 필요한 display elements 만 선택해서 쓸 수 있도록 디자인 해야한다.

### The First Attempt

```java
public class WeatherData{
    public void measurementChanged(){
        currentConditionDisplay.update(temp, humidity, pressure);
        statisticsDisplay.update(temp, humidity, pressure);
        forecastDisplay.update(temp, humidity, pressure);
        // sensor 를 늘리거나, display 를 늘리면 코드가 계속 늘어난다.
    }
}
```

### Meet the Observer Pattern
- 신문 publisher가 있고, 특정 publisher 를 구독하는 subscriber 가 있다. 
- publisher = subject, subscriber = observer

### Observer Pattern
![Validation](/assets/images/observerpattern.png){:width="500px" height="300px"}{: .center}

- one-to-many dependency 를 생성하여, 한 object 의 상태가 변경되면 dependent objects 가 통보받고, 자동으로 업데이트한다.
- observer 로 통신에 참여하는게 굉장히 쉽다. concreteObserver 는 update method 를 implement 하기만 하면 된다.
- concrete subject 입장에서는 concrete observer 에 대한 지식이 없고, observer interface 로만 알고 있다.


### The power of loose coupling
- loose coupling 은 interaction 은 하지만, 서로 아는 것이 거의 없어야 한다. observer pattern 은 subject, observers 간의 loose coupling 을 지원한다.
  

```java
// interface
public interface Subject{
    public void registerObserver(Observer o);
    public void removeObserver(Observer o);
    public void notifyObservers(); // observer 전체에 노티
}

public interface Observer{
    public void update(float temp, float humidity, float pressure);
}

public interface DisplayElement{
    public void display();
}

```

### Using java's official Observable Class
- subject = Observable 로 사용 가능.
- addObserver, deleteObserver, notifyObservers, setChanged 메소드가 이미 존재한다.
- notification 을 보내기 위해서는 setChanged(), notifyObservers() or notifyObservers(Obejct arg) 호출한다.
- observer
  - Observer 가 되기
    - Implement java.util.Observer interface
  - Subscribe
    - addObserver() on any Observable object
  - Unsubscribe
    - deleteObserver()
  - Get updated
    - Implement update(Observable o, Object arg)

### setChanged() 함수
- flexibility 장점
  - notification 최적화
  - sensor 가 sensitive 한 경우, 매번 update 하지 않고 싶을 때, notification points 를 조절할 수 있다.
- clearChanged() - change 변수를 삭제
- hasChanged() - change 변수 가져옴


```java
// Observable side
import java.util.observable;
import java.util.observer;

public class WeatherData extends Observable{
    public void measurementsChanged(){
        setChanged();
        notifyObservers();
    }
}

public class CurrentConditionDispaly implements Observer, DisplayElement{
    Observable observable;
    public CurrentConditionDispaly(Observable observable){
        this.observable = observable;
        observable.addObserver(this);
    }

    public void update(Observable obs, Object arg){
        if(obs instanceof WeatherData){ // WeatherData 에서 온 Observable 인지 확인
            WeatherData weatherData = (WeatherData)obs;
            ...
        }
    }
}
```
