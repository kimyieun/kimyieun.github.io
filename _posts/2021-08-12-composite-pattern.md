---
title: "Composite Pattern"

categories:
  - softwareengineering

tags:
  - design pattern
---

### Composite Pattern
- Purpose
  - a set of nested objects 나 each object 를 동일한 interface 로 취급하는 것을 목표로 한다.
  - object hierarchy 생성을 용이하게 한다.
- Use When
  - object 의 계층 구조 표현이 필요할 때
  - object, compositions of objects 를 동일하게 다루고 싶을 때


### Problems

```java
if(current instanceOf(menuItem)){
// Menu Item (leafnode) 를 다룬다.
}else if(o instanceOf(Menu)){
// Menu (internal node) 를 다룬다.
}
```

### Composite Pattern Diagram

![Validation](/assets/images/compositepattern.gif){:width="400px" height="400px"}{: .center}

- Composite 입장에서는 자식이 Leaf 또는 Composite 이 될 수 있다. 
  - shared aggregation 으로 Component 에 연결하면 2개의 관계를 1개의 관계로 줄일 수 있다(Decorator Pattern).

### Component Class

```java
public class MenuComponent{ // Concrete Class - why??
  public void add(MenuComponent menuComponent){
    throw new UnsupportedOperationException();
  }
  ...
  // 모든 operation 에 대해서 exception 발생 (동일한 interface 제공을 위한 class 이므로 MenuComponent 를 만드는건 의미가 없다)
}
```

```java
// leaf class
public class MenuItem extends MenuComponent{
  String name;
  String description;
  double price;
  boolean vegetarian;
  public MenuItem(String name, String description, boolean vegetarian, double price){
    this.name = name;
    this.description = description;
    ...
  }

  public String getName(){
    return name;
  }

  public boolean isVegetarian(){
    return vegetarian;
  }

  public void print(){
    System.out.print("" + getName());
    if(this.isVegetarian()) System.out.println("(v)");
  }
}

// composite class
public class Menu extends MenuComponent{
  ArrayList menuComponents = new ArrayList();
  String name;
  String description;
  public Menu(String name, String description){
    this.name = name;
    this.description = description;
  }

  public void add(MenuComponent menuComponent){
    menuComponents.add(menuComponent);
  }
  public void print(){
    Iterator iterator = menuComponents.iterator();
    while(iterator.hasNext()){
      MenuComponent menuComponent = (MenuComponent)iterator.next();
      menuComponent.print();
    }
  }
}
```

### Test Prgram

```java
public class MenuTestDrive{
  public static void main(String args[]){
    MenuComponent pancakeHouseMenu = new Menu("pancake house menu", "breakfase");
    MenuComponent cafeMenu = new Menu("cafe menu", "dinner");
  }

  MenuComponent allMenus = new Menu("all menu", "all menus combined");
  allMenus.add(pancakeHouseMenu);
  allMenus.add(cafeMenu);
  pancakeHouseMenu.add(new MenuItem("k&b pancake breakfast", "~~", true, 2.99));
  pancakeHouseMenu.add(new MenuItem("regular pancake breakfast", "~~", false, 2.99));

  waitress waitress = new Waitress(allMenus);
  waitress.printVegetarianMenu();
}

public class Waitress{
  MenuComponent allMenus;

  public Waitress(MenuComponent allMenus){
    this.allMenus = allMenus;
  }

  public void printMenu(){
    allMenus.print();
  }
}
```

### Iterator for Tree?
- 앞의 예제는 print 를 호출한 node 의 자식들만 출력한다. 
- Tree 의 전체 node 를 순회하고 싶으면?

```java
public class Waitress{
  MenuComponent allMenus;

  public Waitress(MenuComponent allMenus){
    this.allMenus = allMenus;
  }

  public void printMenu(){
    allMenus.print();
  }

  public void printVegetarianMenu(){
    Iterator iterator = allMenus.createIterator();
    int counter = 0;
    while(iterator.hasNext()){
      MenuComponent menucomponent = (MenuComponent)iterator.next();
      try{
        if(menuComponent.isVegetarian()){
          menuComponent.print();
          counter++;
        }
      }catch(UnsupportedOperationException e){}
    }
  }
}
```

```java
public class Menu extends MenuComponent{
  Iterator iterator = null;
  public Iterator createIterator(){
    if(iterator == null){
      iterator = new CompositeIterator(menuComponents.iterator());
      return iterator;
    }
  }
}
public class MenuItem extends MenuComponent{
  public Iterator createIterator(){
    return new NullIterator(); // Null Object Pattern
  }
}
```

- 자식이 있는 node/없는 node가 존재하지만, 모든 node가 자식이 있다고 가정하는 것이 좋다. 그게 아니면 conditional code, exceptional code 를 사용해야 하는데, 일관적으로 프로그래밍을 하기 위해 **Null Object Pattern** 을 사용한다.

### Null Iterator
```java
public class NullIterator implements Iterator{
  public Object next(){
    return null;
  }

  public boolean hasNext(){
    return false;
  }

  public void remove(){
    throw new UnsupportedOperationException();
  }
}
```

### Iterator for Composite

```java
public class CompositeIterator extends Iterator{
  Stack stack = new Stack();
  public CompositeIterator(Iterator iterator){
    stack.push(iterator);
  }

  public Object next(){
    if(hasNext()){
      Iterator iterator = (Iterator)stack.peek();
      MenuComponent component = (MenuComponent)iterator.next();
      if(component instance of Menu){
        stack.push(component.createIterator());
      }
      return component;
    }
    else return null;
  }
}
```

### Things to Consider
- composite object 는 자식에 대한 reference 를 갖고 있다. 반대로 부모에 대한 reference 가 필요할까?
  - application 에 따라 다르다. COR Pattern 을 활용하면 부모 refer 갖고 chain 을 따라 request 전달하는데 사용할 수 있다.
- add, remove, getChild 와 같이 child 관리를 위한 method signature 를 어디에 둬야할까?
  - 두 가지 implementation 이 있다.
  - Option 1.
    - transparency 위주.
    - Component 내에 선언하면, Leaf, Composite 구별하지 않고 Component 라는 동일한 포인터를 사용한다. 
    - 단점으로는 Component class 에서 exception 을 일으켜야 한다. leaf는 자식이 없으니까, add, remove, getChild 에서 exception 을 일으켜야 하기 때문이다. 그리고 Composite class 에서는 적절하게 override 한다. **이것은 runtime 에 알 수 있다. compile time 에는 알 수가 없다.** 어떤게 매칭될지 몰라서.
  - Option 2.
    - Safety 위주.
    - component 에는 진짜 공통 method 만 포함한다. 내가 다루는 node가 composite instance 라면, component pointer 로 add, remove, getChild 사용 불가하다. 그러면 pointer를 Composite type 으로 downcast 해야한다. 즉, 더이상 leaft, composite 을 동일한 인터페이스로 사용할 수 없다. compile time 에 leaf node 에 대해 add, remove, getChild 호출하는 것을 걸러낼 수 있다.

### Related Patterns
- decorator pattern
  - 구조적으로 유사하다. **recursive composition.** 자신의 상위 타입으로의 composition 관계. object 개수가 무한하게 늘어나거나 wrapping 할 수 있도록 지원.
  - 의도가 다르다.
    - decorator - subclassing을 하지 않고 이미 존재하는 객체에 대해서 새로운 responsibilities 를 추가하기 위해서 사용한다.
    - composite - tree 모양의 계층 구조를 **표현**하기 위해서 사용한다. 
  - 혼용해서 같이 쓰는 경우도 많다.

- iterator pattern
  - underlying representation 을 노출하지 않고도 일관된 elements of an aggregate object 에 대한 접근을 가능하게 해준다.
- Summary
  - composes objects into tree structures to represent whole-part hierarchies.
  - clients 가 individual, compositions 을 일관되게 다룰 수 있도록 지원.