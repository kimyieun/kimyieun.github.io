---
title: "Memento Pattern"

categories:
  - designpattern

tags:
  - designpattern
---

### Memento Pattern
- 객체의 상태를 특정 기준으로 보존한다.
  - undo, redo, history, snapshot 목적
- caretaker 가 어떤 시점에 memento 를 달라고 originator 에게 요청하면 현재 상태를 memento 에 담아서 반환한다.
- Command pattern 은 request 를 객체로 만들어서 보관


![Validation](/assets/images/mementoc.PNG){:width="500px" height="300px"}{: .center}

- originator 
  - memento **생성**하여 현재 상태 반환하는 역할
  - memento 로부터 이전 상태 받아와서 자신의 상태로 복귀하는 역할
- originator 와 memento 는 강한 coupling 관계이지만, caretaker 와 memento 는 loose coupling 관계이다.


### Example Program
- 특정 a 조건을 만족하면 현재 상태를 보존하는 게임
- 특정 b 조건을 만족하면 이전에 보존해둔 상태로 복귀한다.


```java
public class Memento{
    int money;
    Vector fruits;
    public int getMoney(){
        return money;
    }
    void addFruit(String fruit){
        fruits.add(fruit);
    }
}

public class Gamer{
    public Memento createMemento(){
        Memento m = new Memento(money);
        Iterator it = frrits.iterator();
        while(it.hasNext()){
            String f = (String) it.next();
			if (f.startsWith("맛있다.")) { // 과일은 맛있는 것만 보존
				m.addFruit(f);
			}
        }
        return m;
    }
    public void restoreMemento(Memento memento) { 
		this.money = memento.money;
		this.fruits = memento.fruits;
	}
}

public class Main {
    public static void main(String[] args) {
        Gamer gamer = new Gamer(100); 
        Memento memento = gamer.createMemento();
        for(int i=0;i<100;i++){ // 게임을 진행하는 동안
            if(a){
                memento = gamer.createMemento(); // 새로운 memento 생성
            }
            else{
                gamer.restorMemento(memento); // 이전 상태로 복귀
            }
        }
    }
}

```

![Validation](/assets/images/mementos.PNG){:width="500px" height="300px"}{: .center}


### Discussion
- 배열을 사용하면 여러 개의 memento instance 가질 수 있다.
- caretaker 와 originator 분리하는 의미
  - caretaker 는 언제 snapshot 을 찍어서 undo 를 실행할지 결정하고, memento 를 보관한다.
  - originator 는 memento 역할을 생성하고, caretaker 에게 memento 를 받아서 자신의 상태를 원래대로 되돌리는 일을 한다.
  - undo 로직이나 내용을 변경하고 싶을 때, originator 는 변경할 필요가 없다.
