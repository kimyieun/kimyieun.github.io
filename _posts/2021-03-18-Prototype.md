---
title: "Prototype"

categories:
  - javascript

tags:
  - prototype, inheritance, proto, for in 문, hasOwnProperty
---

- Prototype 이란?

- 모던 자바스크립트 Deep Dive 책을 읽고 정리한 포스팅이다.

## Prototype

- javascript 는 클래스가 없다. Class 라는 예약어는 함수의 한 종류이다. 따라서, **javascript 는 prototype 을 기반으로 상속을 구현한다.**
  - 예를 들어, 사람에 관한 property, method 를 가진 user 객체가 있는데 user와 상당히 유사하지만, 약간의 차이가 있는 admin, guest 객체를 만들어야 하는 상황.

## [[Prototype]]

- js 객체는 명세서에 명명한 [[Prototype]] 이라는 숨김 프로퍼티를 갖는다. 이 숨김 property 의 값은 null 이거나, 다른 객체에 대한 참조가 되는데, 다른 객체를 참조하는 경우 참조 대상을 'property'라 부른다.
  - **프로토타입 상속** - object 에서 property 를 읽을 때, 해당 property 가 없으면 js 는 자동으로 prototype 에서 property 를 찾는다.
  - **proto**를 사용하면 prototype 에 접근하여 값을 설정할 수 있다.
    (엄밀하게는 [[Prototype]]의 getter 이자 setter 이다.)
  - 근래에는 **proto** 대신, Object.getPrototypeOf 또는 Object.setPrototypeOf 를 써서 get,set 한다.

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

- circular reference 는 허용하지 않는다.
- **proto** 의 값은 객체나 null 만 가능하다.
- 객체는 두 개의 객체를 상속받지 못한다.
- 'this' 는 프로토타입에 영향을 받지 않는다. method 를 객체에서 호출했든 프로토타입에서 호출했든 상관없이 this 는 언제나 . 앞에 있는 객체가 된다.
  - 메소드는 공유하지만, 객체의 상태는 공유하지 않는다.

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

## for ... in 반복문

- for...in 은 상속 프로퍼티도 순회대상에 포함시킨다.

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
- Object.keys, Object.values 와 같은 메서드는 대부분 상속 프로퍼티는 제외하고 동작한다.
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
