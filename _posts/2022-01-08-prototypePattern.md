---
title: "Prototype Pattern"

categories:
  - designpattern

tags:
  - designpattern
---

### Prototype Pattern
- 일반적인 인스턴스 생성은 클래스의 생성자를 호출한다.
- Purpose
  - 클래스로부터 새로운 인스턴스를 만드는 것이 아니라, 현재 존재하는 인스턴스를 복사해서 새로운 인스턴스를 만들고 싶을 때 사용한다.
- Prototype
  - 클래스 안에 자신을 복사하는 메소드를 두자. (모든 필드 값이 동일한 인스턴스 생성)


![Validation](/assets/images/prototypePattern.png){:width="500px" height="300px"}{: .center}

- Product interface
  - java.lang.Cloneable interface 상속한다.
  - clone() 메소드를 이용해서 복제를 만들어낸다.
- Manager class
  - Product interface 사용해서 instance 복제를 지시하는 일을 한다.
- Product interface, Manager class 는 구체적인 product 에 대한 정보는 전혀 알지 못한다.


```java
public interface Product extends Cloneable{
    public abstract void use(String s);
    public abstract Product createClone(); 
}


public class Manager{
    private Hashtable showcase = new Hashtable(); // product 의 이름과 instance 를 저장한다.
    public void register(String name, Product p){
        showcase.put(name, p);
    }
    public Product create(String protoname){ // protoname 을 가진 instance 가 생성되어 있다는 것을 가정하고 create clone 을 하는 것 같다.
        Product p = (Product)showcase.get(protoname);
        return p.createClone(); 
    }
}

public class MessageBox implements Product{
    public Product createClone(){
        Product p = null;
        try{
            p = (Product)clone(); // java.lang.Cloneable interface 를 구현한 클래스만 해당 메소드를 가진다. clone() 메소드는 자신의 클래스 및 하위 클래스에서만 호출 가능하므로 createClone 같은 메소드로 감싸줘야 한다.
        }catch(CloneNotSupportedException e){
            e.printStackTrace();
        }
        return p;
    }
}
```

### Discussion
- 관련 패턴
  - Composite 패턴 / Decorator 패턴을 사용하면 복잡한 구조의 인스턴스가 생성되는 경우가 많다. 이 때, prototype 패턴을 사용하면 편리하다.
- clone() method
  - **이 메소드는 자기 자신만 호출할 수 있다.** 그래서 Product 에서 구현해놓지 않고, createClone 으로 한번 더 감싸나보다.
  - 복사 대상이 되는 class 는 반드시 java.lang.Cloneable 인터페이스를 구현해야 한다.