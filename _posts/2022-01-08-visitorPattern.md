---
title: "Visitor Pattern"

categories:
  - designpattern

tags:
  - designpattern
---

### Visitor Pattern
- 자료 구조 안에 저장된 많은 요소에 대해서 무언가 "처리" 하고자 할 때 사용한다.
- 새로운 처리가 필요할 때마다 자료 구조가 포함된 클래스를 수정하고 싶지 않다.
- 자료 구조와 처리를 분리하자.
  - 새로운 처리를 추가하고 싶으면 새로운 "visitor" 를 만들자.
  - 자료 구조는 "visitor" 를 받아들이기만 하면 된다.

### Example Program
- visitor 가 방문하는 자료 구조 - Composite pattern 을 적용한 file 과 directory
- visitor 는 방문 하여 파일 리스트를 출력한다.

![Validation](/assets/images/visitor.png){:width="500px" height="300px"}{: .center}

![Validation](/assets/images/visitordirectory.png){:width="500px" height="300px"}{: .center}

- Acceptor - visitor class 의 instance 를 받아들이는 자료 구조를 나타내는 인터페이스
- 

```java
public abstract class Visitor{
  // method overload 이용
  public abstract void visit(File file);
  public abstract void visit(Directory directory);
}

public interface Acceptor {
  public abstract void accept(Visitor v);
}

public abstract class Entry implements Acceptor{
  public Entry add(Entry entry) throws FileTreatmentException {       // 엔트리를 추가
      throw new FileTreatmentException();
  }
  public Iterator iterator() throws FileTreatmentException {    // Iterator의 생성
      throw new FileTreatmentException();
  }
}

public class File extends Entry {
  private String size;
  private int size;
  public void accept(Visitor v){ // visitor 를 받아들이세요 라고 요청할 때 호출하는 메소드
    v.visit(this);
  }
}

public class Directory extends Entry {
  private Vector dir = new Vector();
  public Iterator iterator() {      // Iterator의 생성
        return dir.iterator();
  }
  public void accept(Visitor v){ // visitor 를 받아들이세요 라고 요청할 때 호출하는 메소드
    v.visit(this);
  }
}

public class ListVisitor extends Visitor { // concrete visitor
  private String currentdir = "";
  public void visit(File file){
    System.out.println(currentdir + "/" + file);
  }
  public void visit(Directory directory) {  
    System.out.println(currentdir + "/" + directory);
    String savedir = currentdir;
    currentdir = currentdir + "/" + directory.getName();
    Iterator it = directory.iterator();
    while(it.hasNext()){
      Entry entry = (Entry)it.next();
      entry.accept(this);
    }
    currentdir = savedir;
  }
}

public class Main {
  public static void main(String[] args) {
    Directory rootdir = new Directory("root");
    Directory bindir = new Directory("bin");
    Directory tmpdir = new Directory("tmp");
    Directory usrdir = new Directory("usr");
    rootdir.add(bindir);
    rootdir.add(tmpdir);
    rootdir.add(usrdir);
    bindir.add(new File("vi", 10000));
    bindir.add(new File("latex", 20000));
    rootdir.accept(new ListVisitor()); 
  }
}

```

### Discussion
- Visitor와 Acceptor 의 상호 호출
  - 더블 디스패치

```java
acceptor.accept(visitor);
visitor.visit(acceptor);
```

- ConcreteVisitor 추가 및 기능 확장도 용이하다.
- Open-Closed Principle
  - 그러나, ConcreteAcceptor 역할 추가는 어렵다.
  - acceptor 의 구조가 변경되면 이를 처리하는 visitor 내부도 변경되어야 하므로.
- iterator pattern 
  - 두 패턴 모두 어떤 자료 구조 상에서의 처리를 실행하는 목적이 있다.
  - iterator pattern 은 자료 구조가 보관하는 요소 하나 하나 얻고자 하는 목적인 반면, visitor pattern 은 특정 처리를 가하는데 목적이 있다.
- 자료 구조가 composite pattern 이 되는 경우가 있다.