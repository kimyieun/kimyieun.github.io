---
title: "Proxy Pattern"

categories:
  - designpattern

tags:
  - designpattern
---

### Proxy Pattern
- 본인을 대신하는 객체를 생성하여 이 객체를 통해서 본인에게 접근하도록 한다.
- Use When
  - object 가 시스템 외부에 있을 때
  - object 를 필요할 때 생성하고 싶을 때 (비용이 비쌀 수도 있다)
  - object 에 대한 access control 이 필요할 때 (보안이 필요하거나)
  - object 에서 추가적인 기능을 지원하고 싶을 때 (proxy 에 대신한다.)
- 특정 객체에 대한 access 를 control 하기 위해 placeholder와 surrogate 를 제공한다.
- proxy 객체는 원래 객체와 부모도 같고 메소드도 동일하다. 

### Example : Monitoring Program


### Remote Proxy
- Gumball Monitor 와 Gumball Machine 이 서로 다른 네트워크에 위치한다.
- Gumball Monitor 는 proxy 를 통해서 Gumball Machine 에 접근한다.

### Proxy Pattern Collaborations
- Proxy
  - real subject 에 대한 reference 를 가진다.
  - interface 는 subject 와 동일하다. client 입장에서는 구별하지 못한다.

### Applicability
1. Remote Proxy - real subject 가 외부 공간에 있을 때
2. Virtual Proxy - real subject 의 일부 정보를 캐싱하고 있어서, real subject 에 대한 접근을 연기한다. virtual proxy 가 대신 일하다가 real subject 가 진짜 필요할 때만 보낸다.
   1. 실제 객체를 생성하는데 비용이 많이 들 수도 있다. 생성되는 동안 프록시가 처리하다가 생성되면 delegation 해줄 수 있다.
3. Protection Proxy - real subject 에 대한 access permission 을 관리해서 보낼지 말지 결정한다.

### Virtual Proxy in Action
- 실제 객체가 이미지일 때 이미지를 로딩하는데 오래 걸리면 "Loading..." 메시지를 보여주기 위해 프록시 객체를 사용한다.
- ImageProxy 실행 과정
  - 1. ImageProxy 는 ImageIcon(실제객체)를 생성하고, loading 을 시작한다.
  - 2. 실제 객체를 생성하는 동안 "loading..." 메시지를 제공한다.
  - 3. 로딩이 완료되면, ImageProxy 는 method call 을 ImageIcon 에게 delegation 한다.


```java
class ImageProxy implements Icon{
    ImageIcon imageIcon; // 실제 객체에 대한 reference 저장
    boolean retrieving = false;

    //...

    public void paintIcon(final Component c){
        if(imageIcon != null){
            imageIcon.paintIcon(c,g,x,y);
        }
        else{
            g.drawString("loading...");
            if(!retrieving){
                retrieving = true; // 이 조건문은 단 한번만 실행된다.
                retrievalThread = new Thread(new Runnable(){
                    public void run(){
                        try{
                            imageIcon = new ImageIcon(imageURL); // 실제 객체 생성을 요청한다.
                            c.repaint();
                        }
                        catch(Exception e){
                            e.printStackTrace();
                        }
                    }
                })
            }
        }
    }
}

public class ImageProxyTestDrive{
    ImageComponent imageComponent;
    public ImageProxyTestDrive() throws Exception{
        Icon icon = new ImageProxy();
        imageComponent = new ImageComponent(icon);
    }
}
```

### Using the JAVA API's Proxy to Create a Protection Proxy
- java 에서 제공하는 proxy 


### Matchmaking in Objectville
- 사람은 본인 정보를 수정할 수 있지만, 다른 사람 정보는 수정하지 못한다. 
- 그러나, hotOrNot 점수는 수정할 수 없고 보는 것만 가능하다.
- owner, others 따라 access control 동작 방식이 다르기 때문에 proxy 는 두 개 필요하다.
  - ownerProxy  - setHotOrNotRating 금지, 기타 정보 수정 메소드 허용
  - othersProxy - setHotOrNotRating 허용, 기타 정보 수정 메소드 금지

```java
public interface PersonBean{
    String getName();
    int getHotOrNotRating();
    void setName();
    void setHotOrNotRating(int rating);
}

public class PersonBeanImpl implements PersonBean{
    String name;
    int rating;

    // 위 메소드 implementation 
}
```


1. invocationHandler 2개를 생성한다.
   1. ownerInvocationHandler, notOnwerInvocationHandler 클래스 생성
2. Proxy 는 자동으로 생성되고, invocationHandler 로 delegation 한다.


```java
public class OnwerInvocationHandler implements InvocationHandler{
    PersonBean person;

    public Object invoke(Object proxy, Method method, Object[] args){

        // .. 생략
        if(method.getName().startsWith("get")){
            return method.invoke(person, args); // 본인이 본인 정보 받아오는 것은 문제 없으므로 real subject 에 전달한다.
        }    
        else if(method.getName().equals("setHotOrNotRating")){
            throw new IllegalAccessException(); // 평가를 변경하는 것은 불가하므로 exception 발생시킨다.
        }
    }
}

public class MatchMakingTestDrive {
	public static void main(String[] args) {
    // ...
    }

    public void drive(){
        PersonBean joe = getPersonFromDB("JOE");
        PersonBean ownerProxy = getOwnerProxy(joe);
        ownerProxy.setHotOrNotRating(10); // exception 발생!
    }

    PersonBean getOnwerProxy(PersonBean person){
        return (PersonBean)Proxy.newProxyInstance(person.getClass().getClassLoader(), person.getClass().getInterfaces(), new OwnerInvocationHandler(person))); // jdk 에서 만들어주는 proxy 를 생성하고 이를 리턴한다. invocationHandler 도 인자로 넘겨준다.
    }
}
```

### Related Patterns
- Adapter Pattern
  - 인터페이스가 다른 것을 해결하고자 하는 목적이지만, Proxy 는 본인과 proxy 의 인터페이스 이미 동일하다. decorator pattern 도 장식자와 장식대상의 인터페이스가 동일하지만, 더 기능이 추가된 인터페이스를 제공한다.
- Decorator pattern 과 구조는 유사하나, 목적이 전혀 다르다. 
  - 둘 다 다른 객체에 대한 indirection 을 제공하여 client 와 실제 객체 간의 coupling 을 낮추는 이점이 있다.
    - adapter, abstract factory, decorator, proxy pattern


### Proxy Zoo
- Firewall Proxy
- Smart Reference Proxy - 접근 횟수 카운팅
- Caching Proxy
- Synchronization Proxy - 멀티 쓰레드 환경에서 subject 에 대한 안정적인 접근 지원
- Complexity Hiding Proxy(Facade proxy)
- Copy-on-Write Proxy - copy 에다가 write