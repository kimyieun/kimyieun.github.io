---
title: "Ch7. Template Method Pattern"

categories:
  - ooad

tags:
  - uml
---

### Template Method Pattern
- purpose
    - algorithm's framework 를 만들어 각 classes 가 실제 behavior 를 정의하도록 한다.
- use when
    - 하나의 알고리즘의 abstract implementation 이 필요할 때
    - subclasses 간 공통 behavior 가 공통 class 에 위치해야할 때


### class coffee and class tea
- class tea, class coffee 코드가 거의 비슷하다.
- 새로운 음료를 추가하면 계속 중복된 코드가 생성된다.

```java
public abstract class CaffeineBeverage{
    // template method - prepareRecipe
    final void prepareRecipe(){ // 하위 class 에서의 override 금지
        boilwater();
        brew();
        pourincup();
        addCondiments();
    }
    abstract void brew();
    abstract void addCondiments();

    public void boilwater(){
        System.out.println("boilwater");
    }

    public void pourincup(){
        System.out.println("pourincup");
    }
}

public class Coffee extends CaffeineBeverage{
    public void brew(){
        ...
    }

    public void addondiments(){
        ...
    }
}
```

### More general approach
- changes
    - 두 subclsees 는 general algorithm 을 상속한다.
    - 몇 method 는 concrete 하지만(same actions), 다른 methods 는 abstract 하다.(class-specific actions)
- advantages
    - CaffeineBeverage class 는 algorithm을 protect/control 하는 역할
    - superclass 는 method 재사용을 돕는다.
    - code 변화가 한 군데에서만 발생하고, 새로운 음료를 추가하기 쉽다.

### Template method pattern
- prepareRecipe 는 template method pattern implementation
    - algorithm template 으로 사용된다. template 에서는 각 step 이 표현된다.
- template pattern 은 algorithm steps 를 정의하고, subclasses 가 그 중 몇개를 선택해 implementation 하도록 하는 디자인 패턴이다.
    - class scope pattern
- algorithm encapsulation (template 생성을 통해)
- algorithm skeleton 정의 및 steps 정의
- subclasses 는 algorithm's structure 바꾸지 않고 redefine steps 가능하다.

### Hook Method
- hook
    - abstract class 에 정의된 method 중 implementation 이 없거나 default 인 경우.
    - subclasses 가 override 할 수 있다.

```java
public abstract class CaffeineBeverage{
    // template method - prepareRecipe
    final void prepareRecipe(){ // 하위 class 에서의 override 금지
        boilwater();
        brew();
        pourincup();
        if(customerWantsCondiments()) addCondiments();
    }
    abstract void brew();
    abstract void addCondiments();

    public void boilwater(){
        System.out.println("boilwater");
    }

    public void pourincup(){
        System.out.println("pourincup");
    }

    boolean customerWantsCondiments(){
        return true;
    }
}

public class Coffee extends CaffeineBeverage{
    public void brew(){
        ...
    }

    public void addondiments(){
        ...
    }

    public boolean customerWantsCondiments(){
        if(...) return true;
        else retrun false;
    }
}

```

### design principle : hollywood principle
- don't call use, we'll call you.
- dependency rot 방지
    - high-level component 가 low-level component 에 의존하고, low-level component 가 high-level component 에 의존하는 것.
- hollywood principle
    - high level component 가 low level component 가 언제 어떻게 필요할지 결정한다.
    - low level components can participate in the computation.

### related patterns
- template method pattern 은 algorithm 구조 변경하지 않고 algorithm 의 특정 부분을 다양화하기 위해 inheritance 사용한다.
- 전략 패턴은 전체 알고리즘을 다양화하기 위해 delegation 사용한다.
- factory method 는 template method 의 특별한 형태이다.
