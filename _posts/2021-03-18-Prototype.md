---
title: "Prototype"

categories:
  - javascript

tags:
  - prototype, inheritance, proto, for in 문, hasOwnProperty
---


- 모던 자바스크립트 Deep Dive 책을 읽고 정리한 포스팅이다.

## Prototype

- javascript 는 클래스가 없다. Class 라는 예약어는 함수의 한 종류이다. 따라서, **javascript 는 prototype 을 기반으로 상속을 구현한다.**
- 프로토타입은 어떤 객체의 상위 객체 역할을 하는 객체로, 다른 객체에 공유 프로퍼티(method 포함)를 제공한다.
  - 예를 들어, 사람에 관한 property, method 를 가진 user 객체가 있는데 user와 상당히 유사하지만, 약간의 차이가 있는 admin, guest 객체를 만들어야 하는 상황.

## Prototype 을 기반한 상속 구현

### 1. 상속 구현 X
- getDiameter 메소드는 모든 인스턴스가 동일한 내용의 메소드를 공유하므로 하나만 생성해서 모든 인스턴스가 공유하는 것이 바람직하다.
- 그러나, 아래 코드에서는 Circle 생성자 함수가 인스턴스를 생성할 때마다 getDiameter 메소드를 중복해서 생성한다.

```javascript
function Circle(radius){
  this.radius = radius;
  this.getDiameter = function(){
    return 2*this.radius;
  };
}
const circle1 = new Circle(5);
const circle2 = new Circle(10);
console.log(circle1.getDiameter === circle2.getDiameter); // false
```

### 2. 상속 구현 O
- 모든 인스턴스는 프로토타입 Circle.prototype 으로부터 getDiameter 메소드를 상속받는다. 즉, 하나의 getDiameter 메소드를 공유한다.

```javascript
function Circle(radius){
  this.radius = radius;
}
Circle.prototype.getDiameter = function(){
  return 2*this.radius;
}
const circle1 = new Circle(5);
const circle2 = new Circle(10);
console.log(circle1.getDiameter === circle2.getDiameter); // true
```

## 프로토타입 객체 = 프로토타입
- 모든 객체는 [[Prototype]]이라는 내부 슬롯을 가진다. 모든 객체는 하나의 프로토타입을 갖는다. 그리고 모든 프로토타입은 생성자 함수와 연결되어 있다. 

![Validation](/assets/images/prototype.png){:width="400px" height="200px"}{: .center}


## \__proto__ vs. prototype
- \__proto__ 접근자 프로퍼티는 모든 객체가 가진다. \__proto__ 를 통해 [[Prototype]] 내부 슬롯에 간접적으로 접근할 수 있다.
- Object.prototype 의 접근자 프로퍼티인 \__proto__ 는 getter/setter 함수라 불리는 접근자 함수를 통해 프로토타입을 취득하거나 할당한다.

```javascript
const obj = {};
const parent = {x : 1};
obj.__proto__ = parent;
console.log(obj.x); // 1
```
- \__proto__ 접근자 프로퍼티는 객체가 직접 소유하지 않고, Object.prototype 의 프로퍼티이다. 모든 객체는 상속을 통해 \__proto__ 접근자 프로퍼티에 접근한다.
- \__proto__ 접근자 사용 이유? 
  - 상호 참조에 의해 프로토타입 체인이 cyclic linked list 를 이루지 않도록 하기 위해서이다. 체크를 통해서 프로토타입에 접근하고 교체할 수 있도록 구현되어 있다.
- \__proto__ 접근자 코드 내의 직접 사용을 권장하지 않는다.
  - 모든 객체가 \__proto__ 접근자 프로퍼티를 사용할 수 있는 것은 아니다. 직접 상속을 통해 Object.prototype 을 상속받지 않는 객체가 있다.

- **prototype 프로퍼티는 함수 객체만 소유한다. 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.**
- 따라서 생성자 함수로서 호출할 수 없는 함수, non-constructor 인 화살표 함수와 메서드(ES6 메서드 축약 표현)은 prototype 프로퍼티 소유하지 않으며, 프로토타입도 생성하지 않는다. 

```javascript
(function(){}).hasOwnProperty('prototype'); // true
({}).hasOwnProperty('prototype'); // false
```
- 생성자 함수로 호출이 불가능한 함수인 화살표 함수와 ES6 메서드 축약 표현으로 정의한 메소드는 prototype property 소유하지 않으며, 프로토타입도 생성하지 않는다.
- **모든 객체가 가지는 \__proto__ 접근자 프로퍼티와 함수 객체만 가지는 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킨다.**


```javascript
function Person(name){
  this.name = name;
}
const me = new Person('kim');
console.log(Person.prototype === me.__proto__); // true
```

## 프로토타입의 constructor 프로퍼티와 생성자 함수
- 모든 프로토타입은 constructor 프로퍼티를 갖는다. 이 constructor 프로퍼티는 prototype property로 자신을 참조하는 생성자 함수를 가리킨다.

## 프로토타입 생성시점
- 프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다. 생성자 함수와 프로토타입은 단독으로 존재할 수 없고 언제나 쌍으로 존재하기 때문이다.


## [[Prototype]]

- js 객체는 명세서에 명명한 [[Prototype]] 이라는 숨김 프로퍼티를 갖는다. 이 숨김 property 의 값은 null 이거나, 다른 객체에 대한 참조가 되는데, 다른 객체를 참조하는 경우 참조 대상을 'property'라 부른다.
  - **프로토타입 상속** - object 에서 property 를 읽을 때, 해당 property 가 없으면 js 는 자동으로 prototype 에서 property 를 찾는다.
  - \__proto__를 사용하면 prototype 에 접근하여 값을 설정할 수 있다.
    (엄밀하게는 [[Prototype]]의 getter 이자 setter 이다.)
  - 근래에는 \__proto__ 대신, Object.getPrototypeOf 또는 Object.setPrototypeOf 를 써서 get,set 한다.

```javascript
let animal = {
  eats: true,
  walk() {
    console.log("walk");
  },
};

let rabbit = {
  jumps: true,
};

rabbit.__proto__ = animal; // rabbit 의 prototype 은 animal이다 = rabbit 은 animal 을 상속받는다.
console.log(rabbit.eats, rabbit.jumps); //true true
console.log(rabbit.walk()); // walk
```

## Prototype Chaining
- 자바스크립트는 객체의 프로퍼티에 접근하려고 할 때, 없는 경우 [[Prototype]] 내부 슬롯의 참조를 따라 순차적으로 검색하는 것을 말한다. 
- prototype chaining 은 자바스크립트가 객체 지향 프로그래밍의 상속을 구현하는 메커니즘이다. 
- 프로토타입 체인의 최상위 객체는 언제나 Object.prototype 이다. Object.prototype 의 프로토타입은 null 이다.
- **프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘, 스코프 체인은 식별자 검색을 위한 메커니즘이다.**
- circular reference 는 허용하지 않는다.
- **proto** 의 값은 객체나 null 만 가능하다.
- 객체는 두 개의 객체를 상속받지 못한다.
- 'this' 는 프로토타입에 영향을 받지 않는다. method 를 객체에서 호출했든 프로토타입에서 호출했든 상관없이 this 는 언제나 . 앞에 있는 객체가 된다.
  - 메소드는 공유하지만, 객체의 상태는 공유하지 않는다.

## 오버라이딩과 프로퍼티 섀도잉
- 프로퍼티 섀도잉 - 프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 프로토타입 체인을 따라 검색해 덮어쓰지 않고 인스턴스 프로퍼티로 추가한다. 인스턴스 메서드가 프로토타입 메서드를 오버라이딩한다. 이 현상을 프로퍼티 섀도잉이라 한다.

```javascript
const Person = (function(){
  function Person(name){
    this.name = name;
  }
  Person.prototype.sayHello = function(){
    console.log(`hi ${this.name}`);
  };

  return Person;
}());

const me = new Person('kim');
me.sayHello = function(){
  console.log(`hi ${this.name}!`);
}
me.sayHello();
```

```javascript
let animal = {
  sleep() {
    this.isSleeping = true;
  },
};

let rabbit = {
  name: "하얀 토끼",
  __proto__: animal,
};

rabbit.sleep();

console.log(rabbit.isSleeping); //true
console.log(animal.isSleeping); // undefined (프로토타입에는 isSleeping 이라는 property 가 없다.)
```
## instanceof 연산자
- 우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상 존재하면 true, 아니면 false 리턴한다.

```javascript
function Person(name){
  this.name = name;
}
const me = new Person('kim');

console.log(me instanceof Person); /// true
console.log(me instanceof Object); /// true

```

## in 연산자
- 객체 내에 특정 프로퍼티가 존재하는지 여부 확인
- 프로토타입 체인상 상속받은 모든 프로토타입의 프로퍼티를 확인하므로 주의
  
```javascript
const person = {
  name : 'kim',
  address : 'seoul'
}
console.log('name' in person); // true
console.log('toString' in person); // true - toString 은 Object.prototype 의 메소드다.
```

## Object.prototype.hasOwnProperty 메소드
- 객체 고유의 프로퍼티 키인 경우에만 true 반환, 상속받은 프로토타입 프로퍼티 키인 경우 false 반환.

```javascript
const person = {
  name : 'kim',
  address : 'seoul'
}
console.log(person.hasOwnProperty('name')); //true
console.log(person.hasOwnProperty('toString')); //false

```

## for ... in 반복문

- for...in 은 상속 프로퍼티도 순회대상에 포함시킨다. 열거 가능한 프로퍼티만 포함.
- 열거 시 순서 보장하지 않는다. 그러나 대부분의 모던 브라우저에서 순서 보장한다.
- 배열에는 이 반복문 대신, 일반적 for 문이나 for of, forEach 메서드 사용을 권장한다.

```javascript
let animal = {
  eats: true,
};

let rabbit = {
  jumps: true,
  __proto__: animal,
};

for (let prop in rabbit) {
  let isOwn = rabbit.hasOwnProperty(prop);
  if (isOwn) {
    console.log(`객체 자신의 property : ${prop}`); //jumps
  } else {
    console.log(`상속받은 property : ${prop}`); //eats
  }
}
```

- 상속 관계 - rabbit -> animal -> Object.prototype -> null
- animal 이 Object.prototype 을 상속받는 이유는 객체 리터럴 방식으로 선언되었기 때문이다.
- hasOwnProperty 메소드는 Object.prototype.hasOwnProperty 으로부터 온 메소드인데, 왜 출력이 안되었나?
  - hasOwnProperty 메소드는 열거 가능한 (enumerable) property 가 아니기 때문이다.
  - **for...in 문은 오직 enumerable 한 프로퍼티만 순회 대상에 포함한다.**
- **Object.keys, Object.values/entries 와 같은 메서드를 사용하면 객체 자신의 고유 프로퍼티만 열거 가능하다.**
- 모던 엔진에서 객체에서 프로퍼티를 가져오는 것과 객체의 프로토타입에서 프로퍼티를 가져오는 것 사이에 성능적인 차이는 없다!

```javascript
let hamster = {
  stomach: [],

  eat(food) {
    this.stomach.push(food);
    //this.stomach = [food]; // 1
  },
};

let speedy = {
  __proto__: hamster,
  //stomach : [] // 2
};

let lazy = {
  __proto__: hamster,
  //stomach : [] // 2
};

speedy.eat("apple");
console.log(speedy.stomach); // apple
console.log(lazy.stomach); // apple
```

- 왜 위와 같은 일이 생길까?
  - speedy.eat("apple") 을 호출하면 eat 메소드를 프로토타입인 hamster 에서 발견한다. 그리고 this 에 speedy 가 할당되어 실행된다.
  - 그러나, speedy 에서는 stomach 가 없으므로 프로토타입 체인을 거슬러 올라가 hamster 의 stomach 를 변경한다. 그러므로, lazy, speedy 는 같은 stomach property 를 공유하게 된다.
  - 1번이나 2번과 같이 코드를 변경하면 이러한 문제를 해결할 수 있다.

## references

- https://ko.javascript.info/prototype-inheritance
- 모던 자바스크립트 Deep Dive
