---
title: "Ch17. Bridge Pattern"

categories:
  - designpattern

tags:
  - design pattern
---

## Bridge Pattern
- Purpose
  - coupling 을 제한하기 위해 abstract object structure와 implementation object structure를 독립적으로 정의한다.
- Use when
  - abstractions, implementation 이 compile time 에 고정되지 않는다.
  - abstractions, implementation 이 독립적으로 확장 가능해야 한다.
  - abstraction 의 implementation 에서의 변화가 clients 에 영향이 없어야 한다.
  - implementation detail 이 client 에게 노출되면 안된다.


### Problem
- 간단한 drawing program 이 있다.
  - rectangle 을 그리고 싶다. 두 개의 drawing library 가 있다. (DP1, DP2)
- Solution
  - abstract class Rectangle 를 만든다.

```java
abstract class Rectangle{
    public void draw(){
        drawLine(_x1, _y1, _x2, _y1);
        drawLine(_x2, _y1, _x2, _y2);
        drawLine(_x2, _y2, _x1, _y2);
        drawLine(_x1, _y2, _x1, _y1);
    }

    abstract protected void drawline(double x1, double y1, double x2, double y2);
}

class V1Rectangle extends Rectangle{
    protected void drawLine(double x1, double y1, double x2, double y2){
        DP1.draw_a_line(x1, x2, y1, y2);
    }
}

class V2Rectangle extends Rectangle{
    protected void drawLine(double x1, double y1, double x2, double y2){
        DP2.drawLine(x1, x2, y1, y2);
    }
}
```

### Requirement Changes
- 위와 같이 코드를 짰는데, 요구사항이 변하면?
  - Circle 도 지원하고 싶다. 
  - Shape 이라는 새로운 class 를 만들고, Rectangle, Circle 이 이를 상속하도록 한다.
  - client 는 shape objects 만 알기 때문에 문제가 되지 않는다.

```java
abstract class Shape{
    abstract public void draw(); // 어떤걸 그리는지 모른다.
}

abstract class Circle extends Shape{
    public void draw(){
        drawCircle(x, y, r); 
    }
    abstract protected void drawCircle(double x, double y, double r); // 어떤 library 사용하는지 모른다.
}

class V1Circle extends Circle{
    protected void drawCircle(){
        DP1.draw_a_circle(x, y, r);
    }
}

class V2Circle extends Circle{
    protected void drawCircle(){
        DP2.drawCircle(x, y, r);
    }
}
```

### The Combinatorial Problem!
- drawing library 가 추가되면? 2 shapes * 3 implementations = 6 classes (V1Circle, V2Circle, V3Circle, ...)
- type of shape 추가되면? 3 shapes * 3 implementations = 9 classes
- class explosion!
  - why? abstraction, implementation 과의 tight coupling(하나의 class 내에 함께 있음). shape 의 각 type 이 무슨 drawing library 를 쓸지 알고 있어야 한다.
- Solution 
  - variations in absraction, variations in implementation 을 분리해야 한다. 그러면 class 개수를 linear 하게 증가시킬 수 있다.


### Another Approach with Inheritance Only
- implementation 먼저 구별하고, abstraction 구별해도 class 개수는 변함 없다.
- 문제 분석하려면, 두 가지 일을 해야한다.
  - commonality analysis
    - common things : shape, drawing. 이 둘을 encapsulation 하자.
  - variability analysis
    - shape : rectangle, circle
    - drawing : v1Drawing, v2Drawing
  - 공통적인 것을 abstract class 로 표현하고, variations 를 concrete class 로 표현해라.

### Relating each other
- Realting the classes by having one use the other (object composition)
- Option 1 - drawing uses shape
  - drawing objects 가 shape 을 직접 그릴 수 있으면, shape 에 대한 정보(어떻게 생겼는지, 무엇인지)를 갖고 있어야 하므로, information expert 원칙에 위배된다.
  - 이것은 또한 shape 정보가 노출되었으므로 encapsulation 도 위배한다. high coupling, low cohesion. 
- Option2 - shape uses drawing (better!)
  - shape 는 shape 을 그리기 위해 drawing objects 를 사용한다.
  - shape 은 어떤 타입의 drawing objects 를 써야하는지 모른다. 

### Visualizing the Solution

![Validation](/assets/images/bridgepattern.jpeg){:width="500px" height="300px"}{: .center}
- 변하는 것을 찾고 encapsulation 해라.
- inheritance 보다 object composition 사용해라.
- 둘 다, 전략 패턴과 유사하다.

```java
class Client{
    public static void main(String argv[]){
        Shape r1, r2;
        Drawing dp;
        dp = new V1Drawing();
        r1 = new Rectangle(dp, 1, 1, 2, 2);

        dp = new V2Drawing();
        r2 = new Circle(dp, 2, 2 ,3);

        r1.draw();
        r2.draw();
    }
}

abstract class Shape{
    abstract public void draw();
    private Drawing _dp; // 사용할 라이브러리 보관한다.
    Shape(Drawing dp){
        _dp = dp;
    }
    public void drawLine(double x1, double y1, double x2, double y2){
        _dp.draw_a_line(x1, x2, y1, y2);
    }
    public void drawCircle(double x, double y, double r){
        _dp.draw_a_circle(x, y, r);
    }
}

abstract public class Drawing{
    abstract public void drawLine(double x1, double y1, double x2, double y2);
    abstract public void drawCircle(double x, double y, double r);
}

class V1Drawing extends Drawing{
    public void (double x1, double y1, double x2, double y2){
        DP1.draw_a_line(x1, x2, y1, y2);
    }
    public void drawCircle(double x, double y, double r){
        DP1.draw_a_circle(x, y, r);
    }
}

class Rectangle extends Shape{
    public Rectangle(Drawing dp, double x1, double x2, double y1, double y2){
        super(dp); // 상위 class constructor 사용
        _x1 = x1; _x2 = x2; _y1 = y1; _y2 = y2;
    }
    public void draw(){
        drawLine(_x1, _y1, _x2, _y1);
        drawLine(_x2, _y1, _x2, _y2);
        drawLine(_x2, _y2, _x1, _y2);
        drawLine(_x1, _y2, _x1, _y1);
    }
}
```

![Validation](/assets/images/bridgepattern2.png){:width="500px" height="300px"}{: .center}

### Structure of Bridge Pattern
- Abstraction, RefinedAbstraction is a high-level control layer for some entity.
  - 이 layer 는 실제 어떤 일을 하지는 않고, implementation layer (platform) 에 delegation 하는 역할만 한다.
- abstract object, abstract structure - abstract class 와 개념이 다르다! 혼동하지 말자.
- abstraction
  - abstraction's interface 를 정의한다.
  - implementor 의 reference 가진다.
  - implementor 에 요청을 포워딩한다. (collaboration, delegation)
- RefinedAbstraction
  - abstraction interface 확장한다. 변종이 필요할 때 사용.
- Implementor
  - implementation 을 위한 interface 를 정의한다.
- ConcreteImplementation
  - implementor interface 를 implement 한다.

### Comparsion with Adapter
- 두 패턴 모두 내부의 implementation details 은 숨긴다. 둘 다 구조 관련 패턴이다.
- adapter pattern 
  - 관련없는 컴포넌트끼리 일을 함께 하도록 할 때. 디자인이 이미 되고 나서 시스템에 적용된다.(reengineering, interface angineering)
- bridge pattern
  - up-front in a design. 뻔히 class explosion 이 예상될 때, 발생하기 전에 미리 하는 디자인.
  - 동물원에 새로운 동물이 추가되는 것은 당연하다.
- 구조적 차이
  - adapter 는 하나의 interface 만 abstract 하지만, bridge 는 복잡한 entity 를 abstract 한다.
- Summary
  - bridge pattern 은 하나 또는 매우 관련있는 여러 개의 클래스들을 두 개의 분리된 계층구조로 나눈다. (abstraction, implementation)
  - 이 둘은 독립적으로 발전된다.