---
title: "Ch15. Adapter Pattern"

categories:
  - softwareengineering

tags:
  - design pattern
---

### Adapter Pattern
- Wrapper
- Purpose
  - communicate, interact 를 위한 공통 object 를 생성하여 다른 interface를 가진 classes 끼리 함께 일할 수 있도록 한다.
- Use When
  - class 가 interface requirements 를 만족하지 않을 때
- Mechanism
  - client 는 target interface 를 사용해서 adapter 에게 요청한다.
  - adapter 는 adaptee 에게 맞게 요청을 해석해서 adaptee interface 로 변환한다.
  - client 는 결과를 받고, 중간에 translation 과정이 있는지 알지 못한다.
- Motivation
  - toolkit, class library 가 호환되지 않는 interface 를 가지고 있을 때
  - toolkit, library 의 소스 코드에 접근하는 것이 불가능하다. 가능하다 해도, 변화를 최소화하고 싶다.

### Simplified Duck and Turkey

```java
public interface Duck{
    public void quack();
    public void fly();
}

public class MallardDuck implements Duck{
    public void quack(){
        System.out.println("quack");
    }
    public void fly(){
        System.out.println("fly");
    }
}

public interface Turkey{
    public void gobble();
    public void fly();
}

public class WildTurkey implements Turkey{
    public void gobble(){
        System.out.println("gobble");
    }
    public void fly(){
        System.out.println("fly");
    }
}

// Turkey Adapter (Duck - target interface)
public class TurkeyAdapter implements Duck{
    Turkey turkey;
    public TurkeyAdapter(Turkey turkey){
        this.turkey = turkey;
    }

    public void quack(){
        turkey.gobble();
    }

    public void fly(){
        for(int i=0;i<5;i++) turkey.fly();
    }
}
```

```java
public class DuckTestDrive{
    public static void main(String[] args){
        MallardDuck duck = new MallardDuck();
        WildTurkey turkey = new WildTurkey();
        Duck turkeyAdapter = new TurkeyAdapter(turkey);
        testDuck(duck);
        testDuck(turkeyAdapter);
    }

    static void testDuck(Duck duck){
        duck.quack();
        duck.fly();
    }
}
```

### Object Adapter vs. Class Adapter
![Validation](/assets/images/adapterpattern.png){:width="500px" height="300px"}{: .center}

- Object Adapter
  - composition 사용. adapter 객체는 Target interface 를 implements 하고, adaptee 객체의 reference 를 갖고 있다. object composition + delegation.

![Validation](/assets/images/adapterpattern2.jpeg){:width="500px" height="300px"}{: .center}
- Class Adapter
  - inheritance 사용. Adaptee class 상속하고, Target 은 interface 의 경우 implements/아니면 상속한다. 


### Implementation Issues
- How much adaptation? 
  - method 이름 변환, arguments 순서 변환 등 쉽고 단순한 interface 변경
  - 완전히 새로운 set of operations 구성
- two-way transparency
  - two-way adapter : target, adaptee interface 둘 다 지원한다. adapter가 adaptee로 보이거나, target object 로 보일 수도 있음.

### Enumerators vs. Iterator
- Enumeration
  - java's old collection (vector, stack, hashtable)
  - hasMoreElements(), nextElement()
- Iterator
  - hasNext(), next(), remove()


### Adapting Enumeration to Iterator 

![Validation](/assets/images/adapterpattern3.jpeg){:width="500px" height="300px"}{: .center}

```java
public class EnumerationIterator implements Iterator{
    Enumeration enum;
    public EnumerationIterator(Enumeration enum){
        this.enum = enum;
    }
    public boolean hasNext(){
        return enum.hasMoreElements();
    }
    public Object next(){
        return enum.nextElement();
    }
    public boolean remove(){ // 해당 기능은 지원하지 않음
        throw new UnsupportedOperationException();
    }
}
```

### Summary
- class의 interface 를 client 가 기대하는 다른 interface 로 변환한다.
- 호환이 불가능한 interfaces를 가진 classes 들을 함께 일할 수 있게 해준다.
- class adapter, object adapter