---
title: "Ch8. Iterator Pattern"

categories:
  - designpattern

tags:
  - designpattern
---

### Iterator pattern
- cursor
- purpose
    - aggregate objects element 에 접근하되, underlying representation 을 노출하지 않도록 한다.

### breaking news...
- diner, pancake house 메뉴를 통합하고 싶다.

```java
public class MenuItem{
    String name;
    String description;
    boolean vegetarian;
    double price;
    public MenuItem(String name, String description, boolean vegetarian, double price){
        ...
    }
}

public class PancakeHouseMenu{
    ArrayList menuItems;
    ...
    public ArrayList getMenuItems(){
        return menuItems;
    }
}

public class DinerMenu{
    MenuItem[] menuItems;
    public MenuItem[] getMenuItems(){
        return menuItems;
    }
}
```

### client code

```java
PencakeHouseMenu pancakeHouseMenu = new PencakeHouseMenu();
ArrayList breakfastItems = pancakeHouseMenu.getMenuItems();
DinerMenu dinerMenu = new DinerMenu();
MenuItem[] lunchItems = dinerMenu.getMenuItems();

for(){
    MenuItem menuItem = (MenuItem)breakfastItems.get(i);
}

for(){
    MenuItem menuItem = lunchItems[i];
}
```
- problems
    - interface 가 아닌, concrete implementations 에 코딩을 했다.
    - dinerMenu 를 hashtable 로 변경하면, 코드 변경이 많이 필요하다.
    - client 가 각 menu 내부의 collection 표현 방식을 알아야 한다.
    - print for loop 이 거의 동일한 로직 코드가 중복된다.

### Iterator

### java's Iterator

```java
import java.util.Iterator;
public class DinerMenuIterator implements Iterator{
    MenuItem[] list;
    public Object next(){

    }

    public boolean hasNext(){

    }

    public void remove(){

    }
}

public interface Menu{
    public Iterator createIterator();
}
// client code
public class waitress{
    Menu pancakeHouseMenu;
    Menu dinerMenu;

    public void printMenu(){
        Iterator pancakeIterator = pancakeHouseMenu.createIterator();
        Iterator dinerIterator = dinerMenu.createIterator();
        printmenu(pancakeIterator);
        printmenu(dinerIterator);
    }

    private void printmenu(){
        while(iterator.hasnext()){
            MenuItem menuItem = (MenuItem) iterator.next();
        }
    }
}
```

### single responsibility
- class 는 한가지 이유에 의해서만 변화되어야 한다.
- aggregate and iteration - 두 개의 다른 responsibilities 로 iterator pattern 에서는 분리한다.
- cohesion
    - high cohesion 하게 만들고 싶다.

### menu with hashtable
- collections
    - ArrayLists, Hashtable, Vector, LinkedList
    - iterator 사용 가능하다.
- built-in array
    - iterator implementation 해야 한다.


### Related Patterns
- Iterator 는 composite 탐색하기 위해 사용한다.
- polymorphic iterators 는 factory methods 를 사용한다. (iterator subclass 객체 생성하기 위해서)
- memento(현재 상태 보관하는 패턴)는 iterator 와 같이 사용된다. iterator 는 iteration state 를 캡쳐하기 위해 사용한다.