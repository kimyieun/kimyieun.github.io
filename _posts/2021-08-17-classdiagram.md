---
title: "Ch6. Class Diagram"

categories:
  - ooad

tags:
  - uml
---

### UML Class Diagrams
- 동일한 UML Class Diagram 이 다양한 관점에서 사용될 수 있다.
  - conceptual perspective - Domain Model (OOA)
  - design perspective - Design Class Diagram (OOD)


### Object
- system 의 개별 요소
- notations
  - **object name은 소문자로 시작하고, class name 은 대문자로 시작한다.** 
  - object name:class name - maxMiller:Person
  - :class name - :Person (anonymous objects = no object name)
    - 현재 시점에서 object name 결정하지 못했거나, 할 필요가 없는 경우
  

### UML Object Diagram
- **특정 시점**에서의 objects 와 그들의 관계를 표현한다.


### From Object to Class
- class 는 system 에서 유사한 objects 집합에 대한 construction plan 이다.
  - objects are instances of classes.
- Attributes
  - class 의 구조적인 특징
  - 각 instance (object) 마다 다른 값을 가진다.
- Operations
  - class 의 행동
    - class 내의 모든 objects 가 동일하므로, object diagram 에서 표현하지 않는다.


### Attribute syntax - Visibility
- attribute 에 대해 누가 접근 가능한지 표현한다
  - \+ : public. everybody. encapsulation X. information hiding X.
  - \- : private. object 본인만 접근 가능하다. default attribute.
  - \# : protected, class 본인과 subclasses 만 접근 가능하다.
  - \~ : package, 동일한 package 에 있는 classes 만 접근 가능하다.
- attribute 앞에 아무것도 없으면 자동으로 \-, operation 앞에 아무것도 없으면 자동으로 \+.


### Attribute syntax - Derived Attribute
- 내가 직접 값을 넣는게 아니라, 다른 attributes, association 으로부터 계산되어 도출된다.
- e.g., \+/age : int (/ : derived attribute)

![Validation](/assets/images/attributesyntax.png){:width="500px" height="300px"}{: .center}
- Data types
  - primitive data type
    - pre-defined : Boolean, Integer, Unlimited Natural, String
    - User-defined : << primitive >>
    - Composite data type : << datatype >>
  - Enumerations : << enumeration >>
- User-defined classes

### Attribute syntax - Multiplicity
- default value : 1
- [min, ..., max]
  - e.g., no upper limit : [\*], [0..\*]
  - e.g., String[1,..\*] - 1줄 ~ 여러 줄

### Attribute syntax - Default Value
- 사용자가 명시적으로 세팅하지 않고, class 생성부터 default value 를 준다. 
- e.g., password : String = "pw123"

### Operations and Methods
- an operations is not a method.
  - UML operations 은 declaration 이다.
    - name, params, return type, exceptions list, pre-and post-conditions 를 포함해야 한다.
  - implementation 이 아니다. method 가 operation 을 구현한 implementations(code) 이다. 
- class diagram 에서 그리는 함수는 operation 이다.
  - body 가 없고, declaration 수준이다. 


### Class Variable and Operation
- class variable 과 class operation 은 class 만 사용하고, object(instance)는 사용할 수 없다.
- class varaible(class attribute, static attribute)
  - class 마다 한번만 선언 가능하다. class 의 instances 에서 공유 가능하다. 예를 들어, class new 를 몇 번 했는지 저장할 때 유용하다.
- class operation(static operation)
  - **instance 가 없을 때 사용된다.**
  - constructors, counting operations
- notation : class diagram 에서 밑줄 긋는 것

```java
private static int pNumber;
public static int getPNumber(){...};
```

### Operations to Access Attributes in DCDs
- accessing operations - get or set all private attributes
- private attribute 는 getter 없으면 접근이 불가능하다.
- 개수가 너무 많아서 보통 class diagram 에서 표현하지 않는다.

### Note Symbols
- UML note symbol
  - UML note or comment - 정의에 semantic 한 영향을 주지 않는다.
  - UML constraint
  - Method body - UML operation 의 implementation
  
### Different Levels of Class Detail
- coarse-grained -> fine-grained
  - class 이름만 표기 -> class 이름, attribute visibility, name, type, derived value, operations ... 등등 아주 디테일하게 표현

### Types of Class Relationship (5 types)
1. dependency - parameter 로 받아서 잠깐 같이 일하고 마는 경우
2. association - 특정 기간동안 같이 일을 하는 경우
3. aggregation - reference 를 소유하지만 다른 class 와도 공유하는 경우
4. composition - reference 를 나만 소유하는 경우
5. inheritance - 다른 class 의 type 이 되는 경우


### 1. Dependency
- 점선 화살표로 표현한다.
- class 간의 가장 약한 관계를 모델링 한다.
- briefly 하게 objects 를 사용하기 위한 것으로, parameter 로 받는다.
- class diagram 보다, component diagram 에서 사용된다.
- a ---> b : a 가 b에 dependency 를 갖는다. a 의 operation 의 parameters 로 b를 받는다. operation 실행이 종료되면 dependency 가 끊어진다.
    - b 가 변경되면 a 가 영향을 받는다!


### 2. Association
- 실선으로 표현한다. 화살표 사용할 수도 아닐수도.
- instances 간의 가능한 관계를 모델링한다. object 가 다른 class 의 object 와 for some prolonged amount of time 동안 함께 일을 한다.
- binary association
  - 두 개의 class 의 instances 를 잇는다.

### 2. Binary Association - Navigability
- Navigability
  - object 가 상대 object 를 알고, visible attributes, operations 에 접근 가능하다는 의미이다.
  - open arrow head, cross 로 표현한다.
  - A x-> B =  A -> B
    - A 는 B 의 visible attributes, operations 에 접근 가능하다. (not private, but public)
    - B 는 A 에 대해서 아무 것도 모른다.
    - A 는 B 를 role 이름을 가진 attribute 로서 갖는다. 
- Navigability undefined
  - bidirectional navigability (- = <->)
- e.g., Student -> Professor

![Validation](/assets/images/binary.png){:width="300px" height="250px"}{: .center}

```java
class Professor{}
class Student{
    public Professor[] lecturer; 
}
```

### UML Attributes 표기하는 방법
1. attribute text 로 표현
2. association line 으로 표현
   1. navigability arrow, multiplicity, role name
3. both of them

### Attribute Text vs. Association Lines for Attributes
- 일반적인 data type objects 를 위한 attribute text notation 을 사용하고, 다른 것들은 association 사용해서 작성해라.
  - association line 을 보여주는게 visual 강조할 수 있다.


### n-ary Association
- sw 개발보다는 비지니스 process 개발에 사용한다.
- 관계에 2개 이상의 objects 가 포함된다.
- no navigation directions
- ternary association


### Association Class
![Validation](/assets/images/associationclass.png){:width="400px" height="250px"}{: .center}


- class 자신이 아니라, class 간의 관계에 attribute 를 할당하는 것이다.
- association 자체를 class 로 취급하고, attributes, operations, other features 를 추가한다.
- 만약 company 가 많은 사람을 채용하고, empolys association 으로 모델링을 하면 association 자체를 employment class 로 모델링할 수 있다. 
  - salary - 이 정보가 company 에 저장되면 attr 가 너무 많고 복잡하다. person attr 로 저장하면 되지만, 한 명이 여러 회사를 다니면 또 company 별로 필요하다...

### Singleton Classes
- class 의 instance 가 한개만 있다. OOP 에서는 권장하지 않는다.

### Active Class
- class의 instance 가 나중에 thread 가 된다. (active object)
- active object 의 class 를 active class 라 한다.


### Interfaces
- UML 에서 interface realization 표기 방법이 3가지가 있다.
  - socket + lollipop notation
  - dependency line notation
  - interface implementation - class diagram 에서 제일 많이 사용. 점선 삼각형 화살표 (realization)

### 3. Arregation
- association 의 특별한 형태
  - class 가 다른 class 의 일부로 표현된다.
- 특징
  - transitive : b 가 a 의 일부이고, c 가 b 의 일부이면, c 는 a 의 일부이다.
  - asymmetric : a가 b 의 부분이면서 b 가 a 의 부분인 것은 불가능하다.
- types
  - shared aggregation
    - a가 지워져도 b가 다른 애한테도 공유되고 있거나, 지워지지 않는 경우
  - composition
    - a가 지워지면 b도 지워지는 경우

### 3. Shared Aggregation
- 하나의 class 가 다른 class 의 object 의 reference 를 가지면서 share 한다.
- 전체에 대한 부분의 약한 belonging
  - 부분은 전체와 또 독립적으로 존재할 수 있다.
  - a -<> b : b(end) 가 a(head) 를 가진다.
  - aggregating end 에서의 multiplicity 는 > 1 일 수도 있다.
    - 동시에 하나의 element 가 여러 다른 elements 의 부분이 될 수 있다.
    - directed acyclic graph

![Validation](/assets/images/sharedaggregation.png){:width="400px" height="250px"}{: .center}
- student is part of labclass.
- course is part of studyprogram.
- labclass 가 0..1 로, 0 이면 labclass 는 없지만 student 는 있어야 한다. 그러므로 빈 마름모를 사용한다.
- studyprogram 이 1..*로 없어지면 course 도 없다.

### 4. Composition
- class 가 다른 class object 를 contain 한다. 
- one part 는 특정 타임에 최대 하나의 composite object 에만 속할 수 있다.
- composite object 가 지워지면, parts 도 지워진다.
- multiplicity at the aggregating is max = 1
- composite objects form a tree

![Validation](/assets/images/composition.png){:width="400px" height="250px"}{: .center}
- beamer 는 lecturehall 의 부분이고, lecturehall 은 building 의 부분이다.
- building 이 없으면 lecturehall 은 존재할 수 없다.
- lecturehall 이 1이면, beamer 는 lecturehall 에 속하므로 building 이 사라지면, lecturehall 도 사라지고 beamer 도 사라진다. 
- lecturehall 이 0이면, beamer 자체는 있을 수 있다. 0 인 경우는 shared aggregation 이라고 보면 된다. 



![Validation](/assets/images/sharedexample.png){:width="400px" height="250px"}{: .center}
1. O. car 가 0 이어도 타이어 4개는 있을 수 있다. car = 1 이면 타이어는 4개를 가진다. 
2. X. tire 는 car 가 없으면 존재할 수 없다. composition 을 shared aggregation 으로 바꾸면 맞는 말이다.
3. X. car 여러 개가 4개의 tire 를 공유한다. 하나의 타이어는 여러개의 차에 속할 수 없다.
4. O. car 여러 개가 1-2개의 tire type 을 공유한다.

### 5. Generation(Inheritance)
- class 의 모든 것이 subclasses 에 전달된다.
  - subclass 의 모든 instance 는 동시에 superclass 의 indirect instance 이다.
  - subclass 는 **private 을 제외하고** superclass 의 attribute, operations, association 관계, aggregations 관계를 모두 상속받고 추가로 더 가질 수 있다.
- generalizations are transitive.

### Generalization - Abstract Class
- {abstract} A 또는 *class name* 
- **subclass 의 공통 속성을 강조하기 위해 사용한다.**
- superclass 의 직접적인 instance 가 없고, descendants 만 instance 를 만들 수 있다.
- 공통의 attr, op 는 fully 정의되어 있어야 하고, 비어있는 경우는 abstract operation 이라 한다.

### Generalization - Multiple Inheritance
- UML 은 multiple inheritance 지원한다.
  - JAVA 는 불가능. C++ 은 가능.
- inheritance 사용하면 이해는 쉬운데, class 개수가 많아지고 attribute 를 일일이 찾아야 한다.


### Creating a Class Diagram
- 자연어에서 class, attribute, association 완벽하게 자동으로 추출하는 것은 불가능하다.
- guidelines
  - noun - class
  - 작은 명사? adjectives - attribute values
  - verbs - operations
- 잘 안맞을 수 있고, 경험이랑 능력이 짱이다.

### Quiz 
- 모든 attribute, operation 은 visibility 가 항상 표현되어야 한다. (x)
- 모든 private attribute 에 대해서는 getter, setter operations 을 항상 만들어야 한다. (o)
- everything of a general class are passed on to its subclasses. (x)
- a -<> b - b 와 a는 동등한 개념이다 (x) b가 a 를 가진다.
