---
title : "Variable Hoisting and Data Type"

categories :
    - javascript

tags :
    - javascript, variable hoisting, var, const, let, template literal, data type, dynamic type language, block, operator

---
- 모던 자바스크립트 Deep Dive 책을 읽고 정리한 포스팅이다.

## variable hoisting
- 변수 선언문이 런타임이 아닌 그 이전 단계에서 먼저 실행되어, 코드의 선두로 끌어 올려진 것처럼 동작한다.

```javascript
console.log(score); //undefined
var score; 
```
- 변수 뿐만 아니라, var, let, const, function, class 키워드를 사용해 선언하는 모든 식별자는 호이스팅된다. 모든 선언문은 런타임 이전 단계에서 실행되기 때문이다.

```javascript
console.log(score);
score = 80;
var score;
console.log(score); // ?
```

## Template literal
- ES6 부터 도입되었다. 편리한 문자열 처리 기능을 제공하고, 런타임에 일반 문자열로 변환되어 처리된다.
- Backtick (``) 을 사용해 표현한다.


1. multi-line string
- 일반 문자열 내에서는 줄바꿈 허용되지 않는다. 이스케이프 시퀀스를 사용해야한다.


```javascript
var template = `<ul>
    <li>a</li>
    <li>b</li>
</ul>`;
console.log(template);
/*
<ul>
    <li>a</li>
    <li>b</li>
</ul>
*/
```

2. 표현식 삽입

```javascript
console.log(`1+2=${1+2}`);
``` 

## undefined / null type
- undefined 는 개발자가 의도적으로 할당한 값이 아니라 js 엔진이 변수를 초기화할 때 사용하는 값이다.
- 반면에 null 은 변수에 값이 없다는 것을 의도적으로 명시할 때 사용된다. (intentional absence)

## Dynamic Typing
- static / strong type language
  - 변수 선언 시, 변수에 할당 가능한 값의 종류와 타입을 명시해야 한다. **explicit type declaration**
  - 컴파일 시점에 타입 체크를 수행하여 에러를 발생시킴으로써, 런타임에 발생하는 에러를 줄인다.
- dynamic / weak type language
  - js, python 등의 언어는 값을 할당하는 시점에 변수의 타입이 동적으로 결정되고 변수의 타입을 언제든 변경 가능하다.

### 쉼표 연산자 / 지수 연산자
```javascript
var x, y, z;
x = 1, y = 2, z = 3; // 3(제일 마지막 피연산자 평가 결과 반환)
```

```javascript
2 ** 2; // 4
Math.pow(2, 2); // 4 (ES7 지수 연산자 도입 전 사용)
```

### block
```javascript
function sum(a, b) {
    return a + b;
}
// block 문 마지막에는 ; 을 붙이지 않는다. 
```