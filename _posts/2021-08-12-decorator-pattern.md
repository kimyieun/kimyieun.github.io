---
title: "Decorator Pattern"

categories:
  - software engineering

tags:
  - design pattern
---

### Decorator Pattern
- Purpose
  - 이미 존재하는 objects 를 dynamic wrapping 을 통해, 기존의 responsibility와 behavior 를 더하거나 수정할 수 있도록 한다.
- Use When
  - object responsibility, behavior 가 dynamically 변경될 때
  - concrete implementation이 responsibility, behavior 랑 분리되어야 할 때
  - 변경을 위한 subclassing 이 비효율/불가능할 때
  - 구체적인 기능이 object 계층 구조의 상위에 위치하지 않을 때
  - 많은 작은 objects 들이 있을 때 
- 기존의 class 에 추후에 기능을 추가할 때 보통 subclass 를 만드는데, class 개수가 많아진다. 이 때 decorator pattern 사용한다.

### Original Design
- Beverage class - HouseBlend, DarkRoast, Decaf, Espresso
- condiments 를 추가하려면?
  - beverage 종류 * condiment 종류 개의 class 생성 (class number explosion)
  - bevarage class 의 attribute 로 추가하면?


```java
public class Beverage{
    protected String description;
    boolean milk, soy, mocha, whip;
    public float cost(){
        float condimentCost = 0.0;
        if(hasMilk()) condimentCost += milkCost;
        else if(hasSoy()) condimentCost += soyCost;
        ...
        return condimentCost;
    }
}
```

- condiment 별 가격이 달라지거나 새로운 condiment 추가되면?
- 새로운 음료가 추가되는데, 기존 condiments 에 적합하지 않으면?
- double mocha 원하면?

### OCP
- OCP 를 매번 적용하려고 하지 마라. 
  
### Decorator Pattern
- DarkRoast object 를 생성한다.
- Mocha object 로 이를 감싼다.
- Whip object 로 이를 감싼다.
- cost() method 를 호출하면 delegation 을 통해 condiment cost를 계산한다.

### Review of Decorator Idea
- decorator 는 본인의 behavior 를 더한다.
- object wrapping 을 위해 하나 이상의 decorator 사용 가능하다.
- decorators 는 decorate 하는 object 와 동일한 super type 을 갖는다.
- runtime 에 dynamically decorating 가능하다.


![Validation](/assets/images/decorator.png){:width="400px" height="200px"}{: .center}

- 하나의 decorator 와 association 관계를 갖는 component 객체는 하나이다. 이 때, component 객체는 자신이 wrapping 하고자 하는 객체를 가리킨다.
- Decorator pattern 에서는 특이하게 두 class 간의 관계가 association, inheritance 둘 다 있다.
- 상속은 wrapping 을 무한히 하기 위해서 필수적이다. 


```java
public abstract class Beverage{
    protected String description = "unknown beverage";
    public String getDescription(){
        return description;
    }
    public abstract double cost();
}

public class Espresso extends Beverage{
    public Espresso(){
        description = "espresso";
    }
    public double cost(){
        return 1.99;
    }
}

public abstract class CondimentDecorator extends Beverage{
    protected Beverage beverage; // association
    public abstract String getDescription();
}

public class Mocha extends CondimentDecorator{
    public Mocha(Beverage beverage){
        this.beverage = beverage;
    }
    public String getDescription(){
        return beverage.getDescription() + ", mocha";
    }

    public Double cost(){
        return .20 + beverage.cost();
    }
}
```

### Decorator Test Drive

```java
public class StarbuzzCoffee{
    public static void main(String args[]){
        Beverage beverage = new Espresso();
        beverage = new Mocha(beverage);
        beverage = new Mocha(beverage);
        beverage = new Whip(beverage);
    }
}
```

### Using Decorator pattern in JAVA I/O
- File 로부터 data 읽어올 때 사용한다.
- fileinputstream 은 decorated component 이다. 
- decorators : linenumberinputstream(line number counting), bufferedinputstream(성능 향상을 위한 buffer 제공) 등.

```java
public class LowerCaseInputStream extends FilterInputStream{
    public LowerCaseInputStream(InputStream in){
        super(in); // 부모 class 
    }

    public int read() throws IOException{
        int c = super.read();
        ...
    }

    public int read(byte[] b, int offset, int len) throws IOException{
        int result = super.read(b, offset, len);
        ...
    }
}

public class InputTest{
    public static void main(String[] args) throws IOException{
        int c;
        try{
            InputStream in = new LowerCaseInputStream(
                new BufferedInputStream(
                    new FileInputStream('test.txt')));
            ...
        }
        catch(IOException e){
            ...
        }
    }
}
```

### Related Patterns
- Adapter pattern - 다른 interface 제공
- proxy - same interface 제공. real subject 가 있고 그것과 똑같은 interface 를 제공하는 proxy 가 있어서 client 는 그 둘을 구별하지 않고 사용한다
- decorator - enhanced interface 제공
  - 관련 design principle : OCP
  - 장점 : additional responsibilities to an object dynamically, 기능을 확대하기 위해 subclassing 대신 flexible 대안 제시. class explosion 방지
  - **decorator class mirrors the type of components they are decorating**
  - 단점 : 작은 class 를 많이 생성한다(JAVA I/O), 이 패턴에 익숙하지 않으면 이해하기 어렵다.

