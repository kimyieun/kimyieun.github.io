---
title: "Singleton Pattern"

categories:
  - softwareengineering

tags:
  - design pattern
---

### Singleton Pattern
- purpose
  - system 내에 특정 class 의 instance 가 단 하나임을 보장한다.
- use when
  - class instance 가 하나만 필요할 때
  - single object 에 대한 접근이 통제되어야 할 때


### One of a Kind Objects
- One and Only One
  - window manager, printer spooler, thred pool manager, caches, logging, factory
- issues
  - global variable 로 정의하는게 더 쉽지 않나?
  - multi-threading 환경
- implementation
  - singleton pattern 은 언어에 따라 접근 방법이 달라서, 코딩 패턴으로 보기도 한다.


### Usage of Singleton

- clients 는 **new operator 를 사용할 수 없다.**
  
```java
public class AnyClientProgram{
    Singleton s = Singleton.getInstance();
}
```

### The Skeleton of Singleton (Single-thread)

```java
public class Singleton{
    private static Singleton uniqueInstance;
    // private - 외부에서 마음대로 조작하지 못하도록 하는 역할. 혹시나 외부에서 이것을 null 로 변경하면 instance 가 하나 더 생성되기 때문이다. static - getInstance 가 호출될 때 instance 가 없기 때문에. class 에 단 하나만 있고 내부 어디서든 공유할 수 있다.

    private Singleton(){}
    // private - constructor 인데, 보통은 무조건 public. new 사용을 막아야 하니까 사용한다. 아예 선언을 안하면 자동으로 생성되므로 private 으로 만들어줘야 한다.

    public static singleton getInstance(){
        if(uniqueInstance == null){
            uniqueInstance = new Singleton(); // class 내부(자기자신)이기 때문에 constructor 호출 가능
        }
        return uniqueInstance;
    }
    // static method - static 이 아니면 instance 를 사용해야지만 호출 가능하다. 객체가 없어서 만들기 위한 method 이므로 static method 로 사용한다. 
}
```

### Three Design Points
1. Make the constructor be private
2. Provide a getInstance() method
   1. public static Singleton getInstance()
3. Remember the instance once you have created it
   1. private static Singleton uniqueInstance


### Using Singleton on Multi-threads
- thread 간 context-switching 을 하면서 object 가 2개 생성되는 문제가 있다.

### Option 1

```java
public class Singleton{
    private static Singleton uniqueInstance;
    private Singleton(){}
    public static synchronized singleton getInstance(){
        if(uniqueInstance == null){
            uniqueInstance = new Singleton(); 
        }
        return uniqueInstance;
    }
}
```

- 여러 개의 thread 가 getInstance를 동시에 호출하여 문제가 되었으니, 하나만 부를 수 있도록 하자. 
- correct, but low performance. isn't it too much locking?
- locking overhead 발생
  - 예를 들어, A thread 가 먼저 instance 를 생성하였고, B,C,D 는 대기 중이다. B,C,D 는 이미 생성된 instance 그냥 사용하면 되는데, 차례대로 다 기다려야 한다. A->B->C->D 순서로..


### Option 2

```java
public class Singleton{
    private static Singleton uniqueInstance = new Singleton();
    private Singleton(){}
    public static singleton getInstance(){
        return uniqueInstance;
    }
}
```

- class가 memory 에 로드됨과 동시에 생성하면?
- correct and high performance. but, overhead of global variables.
  - 요청이 없는대도 무조건 생성한다는 것이 문제다. 덩치가 큰 애들은 메모리 효율이 떨어질 수 있음. 그리고 그럴거면 global variable 사용하지 왜?
    - 실제로 쉬워서 현업에서는 그렇게 사용 많이 함.


### Option 3. Double Checking

- 다시 처음으로 돌아가서, new 부분만 locking 을 하면 되지 않나?
  
```java
public class Singleton{
    private static Singleton uniqueInstance;
    private Singleton(){}
    public static singleton getInstance(){
        if(uniqueInstance == null){
            Synchronized(Singleton.class){
                uniqueInstance = new Singleton();
            } 
        }
        return uniqueInstance;
    }
}
```

- 위 코드에서 A가 instance 를 만들고 나왔는데, B는 이미 if문 안에 있다. 그러면 진입해서 instance 또 생성하게 된다. (not correct)
- locking 을 하지 않고 일단 uniqueInstance 변수가 초기화되었는지 확인한다. 되어있으면 그것을 반환한다. 아니면, lock 을 하고 변수가 초기화되었는지 다시 확인한다. 만약 다른 thread 가 이미 lock 을 했었다면, uniqueInstance 변수가 초기화 되어있을 것이므로 그것을 반환한다. 그게 아니면 초기화를 진행한다.

```java
public class Singleton{
    private static Singleton uniqueInstance;
    private Singleton(){}
    public static singleton getInstance(){
        if(uniqueInstance == null){
            Synchronized(Singleton.class){
                if(uniqueInstance == null){
                    uniqueInstance = new Singleton();
                }
            } 
        }
        return uniqueInstance;
    }
}
```

- option 1에서 발생한 문제를 처음 instance 를 생성하고자 하는 threads 끼리만 경쟁을 시키고, 이후에 들어오는 threads는 바로 read 가 가능하도록 해결한다.


### Double-Checked Locking
- 구현상의 문제가 있다. 
- thread A 가 lock 을 얻고, instancec 를 생성하려고 한다.
  - singleton 객체를 메모리 내 만들고, uniqueInstancec 변수에 assign 해야 하는데, 규모가 큰 객체라 생성에 시간이 좀 소요된다.
- 그 순간에 B가 첫번째 if 문에 있고, 변수가 생성되는 것처럼 보이는 것을 알아채고 그 값을 반환한다. B는 값이 이미 초기화되었다고 생각하고, LOCK 을 얻지 않았다.
- 그런데, A가 초기화를 끝마치기 전에 B에서 접근해서 runtime error 가 발생할 수 있다.
- 위 문제는 심지어 재현도 어렵다. 


### Correct Double-Checked Locking 

```java
public class Singleton{
    private volatile static Singleton uniqueInstance;
    private Singleton(){}
    public static singleton getInstance(){
        if(uniqueInstance == null){
            Synchronized(Singleton.class){
                if(uniqueInstance == null){
                    uniqueInstance = new Singleton();
                }
            } 
        }
        return uniqueInstance;
    }
}
```

- performance, memory, correct 모두 만족
- volatile keyword
  - shared variable 이 정확히 sync 되도록 보장한다.
  - java version 에 따라 의미가 변형됨.


### Reviewing the Options
- Option 1
  - 직관적. correct. 잦은 locking 으로 인한 runtime performance 이슈.
- Option 2
  - eager instantiation. 
- Option 3
  - perfect solution w.r.t performance. 최소 java 5 이상 보장 필요. 성능 문제가 없는 경우 과도한 double-checked locking 이 문제가 될 수 있음.

### Related Patterns and Summary
- Abstract Factory, Builder, Prototype 에서 구현할 때 Singleton 사용한다.
- Facade Object, State Object 도 Singleton 이다.

