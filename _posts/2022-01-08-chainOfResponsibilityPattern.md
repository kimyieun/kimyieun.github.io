---
title: "Chain of Responsibility Pattern"

categories:
  - designpattern

tags:
  - designpattern
---

### Chain of Responsibility Pattern
- 어떤 요구가 발생했을 때, 그 요구를 처리할 객체를 바로 결정할 수 없으면 다수의 객체를 chain 으로 연결해두고 차례로 돌면서 목적에 맞는 객체를 결정하는 패턴


![Validation](/assets/images/chainofresp.png){:width="500px" height="300px"}{: .center}

```java
public class Trouble{
    private int number; // trouble number
    // constructor
    public int getNumber(){
        return number;
    }
}

public abstract class Support{
    private String name; // trouble 해결자 이름
    private Support next; // 자기 다음의 support 가리키는 변수

    public Support(String name) {
		this.name = name;
	}

    public Support setNext(Support next){
        this.next = next;
        return next;
    }

    public final void support(Trouble trouble){
        if(resolve(trouble)){ // 내가 처리할 수 있으면
            done(trouble); 
        }
        else if(next != null){ // 내가 처리하지 못했고, next support 가 있으면
            next.support(trouble);
        }
        else fail(trouble); // next support 가 없으면
    }

    protected abstract boolean resolve(Trouble trouble); // support subclass 가 각자 구현한다.
    protected void done(Trouble trouble){
        System.out.println(trouble + " is resolved by " + this);
    }
}

public class OddSupport extends Support{
    public OddSupport(String name){
        super(name);
    }

    protected boolean resolve(Trouble trouble){
        if() return true;
        else return false;
    }
}

public class Main {
	public static void main(String[] args) {
        Support alice = new NoSupport("Alice");
		Support bob = new LimitSupport("Bob", 100);
		Support charlie = new SpecialSupport("Charlie", 429);
		Support diana = new LimitSupport("Diana", 200);
		Support elmo = new OddSupport("Elmo");
		Support fred = new LimitSupport("Fred", 300);

        alice.setNext(bob).setNext(charlie).setNext(diana).setNext(elmo).setNext(fred);

        for (int i = 0; i < 500; i += 33) {
			alice.support(new Trouble(i));
		}
    }
}
```

![Validation](/assets/images/chainseq.png){:width="500px" height="300px"}{: .center}

### Discussion
- client 는 맨 처음에 있는 handler 에게만 요구를 한다. 
- 이 패턴을 사용하지 않으면, "이 problem 은 이 handler 가 해야한다" 라는 지식을 누군가 중앙 집중적으로 가지고 있어야 한다.
  - 요구자가 들고 있게 하는 것은 좋지 않다.
- 동적으로 handlers 의 순서를 바꿀 수 있다. 
- 유연성은 높지만, 처리 속도가 느릴 수 있다. 
  - problem 과 handler 의 관계가 고정적이고 처리 속도가 중요한 경우에는 이 패턴을 권장하지 않는다.
