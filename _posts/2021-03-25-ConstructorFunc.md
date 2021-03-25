---
title: "생성자 함수"

categories:
  - javascript

tags:
  - javascript, constructor function,
---

- 모던 자바스크립트 Deep Dive 책을 읽고 정리한 포스팅이다.

## Constructor
- new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수. 

## 객체 리터럴에 의한 객체 생성 방식의 문제점
- 단 하나의 객체만 생성한다. 동일한 프로퍼티를 갖는 객체 생성해야 할 때 비효율적이다.
- **생성자 함수를 이용해 객체를 생성하면 템플릿(클래스)처럼 생성자 함수를 사용하여 동일한 프로퍼티 갖는 객체를 여러 개 만들 수 있다.**

```javascript
function Circle(radius){
    this.radius = radius;
    this.getDiameter = function(){
        return 2 * this.radius;
    }
}
```

## 생성자 함수의 인스턴스 생성 과정
- 인스턴스 생성, 인스턴스 초기화(optional)
  
```javascript
function Circle(radius){
    // 1) 암묵적으로 빈 객체가 생성되고 this에 바인딩된다.
    console.log(this); // Circle{}

    // 2) 인스턴스 초기화한다.
    this.radius = radius;
    this.getDiameter = function(){
        return 2 * this.radius;
    }
    // 3) 완성된 인스턴스가 바인딩된 this를 암묵적으로 반환한다.
}
```
- 만약 this 가 아닌 다른 객체를 명시적으로 반환하면, this 는 무시되고 return 문에 명시한 객체가 반환됨. (원시 값을 반환하면 원시 값 반환이 무시된다.) -> **생성자 함수에서 리턴을 쓸 일은 없다!**

## 함수와 객체의 차이
- 함수는 객체이므로 일반 객체와 동일하게 동작한다. 함수 객체는 일반 객체가 가지는 내부 슬롯과 내부 메서드를 모두 가지고 있기 때문이다.
- 차이점은 **일반 객체는 호출할 수 없지만, 함수는 호출할 수 있다.**
- 함수는 함수 객체만을 위한 [[Environment]], [[FormalParameters]] 등의 내부 슬롯과 [[Call]], [[Construct]] 같은 내부 메서드를 가진다.
- 일반 함수로 호출되면 [[Call]] 이 호출되고, new 연산자와 함께 생성자 함수로 호출되면 [[Construct]] 가 호출된다.
- callable - [[Call]] 메서드 갖는 함수 객체(모든 함수는 호출 가능하다), constructor - [[Construct]] 메서드를 갖는 함수 객체, [[Construct]] 가 없으면 non-constructor 이다.
- 즉, 모든 함수는 callable 이지만, constructor 인 것은 아니다.

## constructor vs. non-constructor
- 함수 정의 방식에 따라 구분한다.
- **constructor - 함수 선언문, 함수 표현식, 클래스 (클래스도 함수다)**
- **non-constructor - 메서드(ES6 메서드 축약 표현만 포함. 일반 함수로 정의된 것은 constructor), 화살표 함수**

```javascript
function foo(){}
const bar = function(){};
const baz = {
    x : function(){}
};

// constructor
new foo();
new bar();
new baz.x();

const arrow = () => {};
const obj = {
    x(){}
};

// non-constructor
new arrow(); // TypeError: arrow is not a constructor
new obj.x(); // TypeError: obj.x is not a constructor
```

## new.target (es6 도입)
- 생성자 함수를 파스칼 케이스 컨벤션을 사용해도 new 연산자 없이 호출될 수 있는 위험이 있다.
- 함수 내부에서 new.target 을 사용하면 new 연산자와 함께 생성자 함수로서 호출되었는지 확인할 수 있다.
- 생성자 함수로 호출되면 new.target 은 함수 자신을 가리키지만, 일반 함수로 호출되면 undefined 이다.

```javascript
function Circle(radius){
    if(!new.target){
        return new Circle(radius); // 생성자 함수 재귀 호출하여 생성된 인스턴스 반환한다.
    }
    this.radius = radius;
    this.getDiameter = function(){
        return 2 * this.radius;
    }
}
```