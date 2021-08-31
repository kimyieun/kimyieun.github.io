---
title: "Effective Typescript"

categories:
  - javascript

tags:
  - typescript
---

### Item 16. number 인덱스 시그니처보다는 Array, Tuple, ArrayLike 사용하기
- javascript 에서 객체란 키/값 쌍의 모음이다. 키는 보통 문자열이다. 복잡한 객체를 키로 사용하려고 하면 toString 메서드가 호출되어 객체가 문자열로 변환된다.
- 특히, 숫자를 키로 사용하려고 하면 자바스크립트 런타임은 문자열로 변환한다.

```js 
const x = {};
x[[1,2,3]] = 2;
x; // {'1,2,3' : 1}
const y = {1 : 2, 3 : 4};
y; // {'1' : 2,'3' : 4}
```
- 배열은? 배열도 객체다. 그러므로 숫자 인덱스를 사용하는 것이 당연하다.

```js
x = [1,2,3];
x[0]; // 1
x['1']; // 2 - 문자열 키로도 접근 가능!
Object.keys(x); // ['0', '1', '2']
```

- 타입스크립트는 이 문제를 해결하기 위해 숫자 키를 허용하고, 문자열 키와 다르다고 인식한다. 

```ts
interface Array<T>{
    // ...
    [n : number]: T;
}
```

- 런타임에는 문자열 키로 인식하므로 이 코드는 완전히 가상이지만, 타입 체크 시점에 오류를 잡을 수 있다.

```ts
const xs = [1,2,3];
const x0 = xs[0];
const x1 = xs['1']; // error!

const keys = Object.keys(xs); // string[]
for(const key in xs){ 
    key; // string
    const x = xs[key]; // number (not type error?)
}
```
- typescript 에서 string 이 number 에 할당될 수 없지만, 배열 순회하는 코드 스타일에 대한 실용적인 허용이다. 배열 순회에 좋은 방법은 아니다.

```ts
for(const x of xs){
    x; // number
}

// index's type
xs.forEach((x,i) => {
    i; // number;
    x; // number;
})

for(let i=0;i<xs.length;i++){
    const x = xs[i];
    if(x < 0) break;
}
```

- 숫자를 사용해 인덱스할 항목을 지정하고 싶으면 Array, Tuple 을 사용하는 것이 좋다.
- 어떤 길이를 가지는 배열과 비슷한 형태의 튜플을 사용하고 싶다면 typescript 의 ArrayLike 타입을 사용한다.

```typescript
const tupleLike: ArrayLike<string> = {
    '0' : 'A',
    '1' : 'B',
    length : 2
};

function checkedAccess<T>(xs : ArrayLike<T>, i : number): T{
    if(i < xs.length){
        return xs[i];
    }
    throw new Error();
}
```

### Item 17. 변경 관련된 오류 방지를 위해 readonly 를 사용하기
- **readonly** number[] vs. number[]
    - 배열의 요소를 읽을 수 있지만, 쓸 수 없다.
    - length 읽지만, 바꿀 수 없다.
    - 배열 변경하는 메서드 호출할 수 없다. 
    - number[] 는 readonly number[] 보다 기능이 많기 때문에 readonly number[]의 subtype 이 된다.

```ts
const a : number[] = [1,2];
const b : readonly number[] = a;
const c : number[] = b; // error!
```

- 매개변수를 readonly 로 선언하면?
    - 매개변수가 함수 내에서 변경되는지 체크한다.
    - 호출하는 쪽에서 함수가 매개변수를 변경하지 않는다는 것을 보장받는다.
    - 호출하는 쪽에서 함수에 readonly 배열을 매개변수로 넣을 수 있다.
- 자바스크립트도 명시적으로 언급하지 않지만, 함수가 매개변수를 변경하지 않는다고 가정한다. 타입스크립트는 이를 명시적인 방법으로 확정시켜준다.

```ts
function arraySum(arr : readonly number[]){
    let sum = 0;
    for(const num of arr){
        sum += num;
    }
    return sum;
}
```

- 함수가 매개변수를 변경하지 않는다면, readonly로 선언하면 된다. 그러면 해당 함수를 호출하는 다른 함수들도 readonly로 만들어야 한다. 다른 라이브러리의 함수를 호출하려면 타입 선언이 불가능하니, 타입 단언문을 사용해야 한다.

```ts
let currPara: readonly string[] = [];
currPara = [];
currPara = ['a'];
currPara = currPara.concat(['b']); 
currPara.push('b'); // error!
```

```ts
const paragraphs : string[][] = [];
// readonly 로 선언된 변수(currPara)를 paragraphs 에 포함시키고 싶으면?

// 1. currPara 의 복사본을 만든다.
paragraphs.push([...currPara]);

// 2. 새로운 변수를 readonly string[] 의 배열로 변경한다.
const paragraphs : (readonly string[])[] = [];
// 변경 가능한 배열의 readonly 배열

// 3. 단언문 사용한다.
paragraphs.push(currPara as string[]);
```

- **readonly 는 shallow 하게 동작한다.**
    - 객체에 readonly 배열이 있다면, 그 객체 자체는 readonly 가 아니다.
- Readonly 제너릭도 마찬가지이다.
- deep readonly 를 사용하고 싶다면? 
    - 제너릭을 만들면 되는데 까다롭기 때문에, 라이브러리를 사용하는 것이 좋다. 
        - ts-essentials의 DeepReadonly generic 

```ts
const dates : readonly Date[] : [new Date()];
dates.push(new Date()); // error! 배열을 변경하는 것은 불가능
dates[0].setFullYear(2037); // 배열 내 객체 변경은 가능
```

```ts
// 인덱스 시그니처에도 readonly 사용 가능
let obj : {readonly [k:string] : number} = {};
obj.hi = 45; // error!
obj = {...obj, hi : 12}; 
obj = {...obj, bye : 12};
```

### Item 18. 매핑된 타입을 사용하여 값을 동기화하기

```ts
interface ScatterProps{
    // data
    xs : number[];
    ys : number[];

    // display
    xRange : [number, number];
    yRange : [number, number];
    color : string;
     
    // event
    onClick : (x : number, y : number, index : number) => void;
}
```

- data, display 가 바뀔 때만 다시 그리고 싶다. 어떻게 최적화?

```ts
// 1. 보수적, fail close 접근법 - 오류 발생 시 적극적으로 대처
function shouldUpdate{
    oldProps : ScatterProps,
    newprops : ScatterProps
}{
    let k : keyof ScatterProps;
    for(k in oldProps){
        if(oldProps[k] !== newProps[k]){
            if(k !== 'onClick') return true;
        }
    }
    return false;
}
// 2. fail open 접근법
function shouldUpdate{
    oldProps : ScatterProps,
    newprops : ScatterProps
}{
    return (
        oldProps.xs !== newProps.xs ||
        oldProps.ys !== newProps.ys ||
        ...
        // (no check for onClick)
    )
}
```
- 1번은 차트가 정확하지만 너무 자주 그려질 가능성이 있고, 2번은 차트를 다시 그려야하는데 누락되는 일이 생길 수 있다.
- 새로운 속성이 추가되면 매번 shouldUpdate 를 업데이트 해줘야 한다.
- 타입 체커가 자동으로 해당 일을 하도록 변경해보자.

```typescript
const REQUIRES_UPDATE: {[k in keyof ScatterProps] : boolean} = {
    xs : true,
    ys : true,
    xRange : true,
    yRange : true,
    color : true,
    onClick : false,
}

function shouldUpdate{
    oldProps : ScatterProps,
    newprops : ScatterProps
}{
    let k : keyof ScatterProps;
    for(k in oldProps){
        if(oldProps[k] !== newProps[k] && REQUIRES_UPDATE[k]){
            return true;
        }
    }
    return false;
}
```
- ScatterProps 에 새로운 속성이 추가되면 REQUIRES_UPDATE 정의에 오류가 발생하므로 정확한 동기화를 할 수 있다.


## Ch 3. Type Inference

### Item 19. 추론 가능한 타입을 사용해 장황한 코드 방지하기
- 타입스크립트에서는 사실 많은 타입 구문이 불필요하다. 간단한 타입부터 복잡한 객체까지 추론이 가능하다. 
- 사람보다 더 정확하게 추론하기도 한다.

```ts
const axis1 : string = 'x'; // string
const axis2 = 'y'; // 'y'

// 비구조화 할당문
interface Product{
    id : number;
    name : string;
    price : number;
}

function logProduct(product : Product){
    const {id, name, price} = product;
    console.log(id, name, price);
}
```
- 비구조화 할당문은 모든 지역 변수의 타입이 추론되도록 한다. 여기에 추가로 명시적 타입 구문을 넣으면 불필요한 타입 선언이 된다.

- 타입스크립트는 일반적으로 변수의 타입을 최종 사용처까지 고려하지 않고 처음 등장할 때 결정한다.
- 이상적인 타입스크립트 코드는 함수/메서드 시그니처에 타입 구문을 포함하지만, 함수 내에서 생성된 지역 변수에는 타입 구문을 넣지 않는다.
- 매개 변수에 타입 구문이 필요하지 않는 경우는?
    - 기본값이 있는 경우
    - 타입 정보가 있는 라이브러리에서의 콜백 함수

```ts
function parseNumber(str : string, base = 10){
    // ...
}

// express HTTP server library
app.get('/health', (req, res) => {
    res.send('ok');
})
```

- 타입 추론이 되지만 타입을 명시하고 싶은 경우
    - 객체 리터럴 정의할 때 
      - 타입을 명시하면 잉여 속성 체크가 동작한다. 그리고 변수를 할당하는 시점(실제로 실수가 발생한 부분)에 오류가 발생한다. 타입 구문이 없다면 잉여 속성 체크가 동작하지 않고, 변수를 사용하는 곳에서 타입 오류가 발생한다.

    ```ts
    // 1. 객체 리터럴 정의 시, 타입 명시하지 않은 경우
    const furby = {
        name : 'furby',
        id : 1234,
        price : 56
    };

    fn1(furby); // error!

    // 2. 객체 리터럴 정의 시, 타입 명시한 경우
    const furby : furbyType = {
        name : 'furby',
        id : 1234, // error!
        price : 56
    };

    fn1(furby); 
    ```

    - 함수의 반환 타입에도 타입 추론이 가능하더라도 타입 구문을 명시하는 것이 좋다.
        - 정확한 오류의 위치를 알려준다.
        - 함수에 대해 명확하게 알 수 있다. 시그니처를 먼저 작성하면 구현에 맞춘 주먹구구식을 방지할 수 있다.
        - 명명된 타입을 사용하기 위해서이다. 

```ts
interface Vector2D {x : number, y : number};
function add(a : Vector2D, b : Vector2D){
    return {x : a.x + b.x , y : a.y + b.y};
}
```

- linter 에서 no-inferrable-types 를 사용하면 작성된 모든 타입 구문이 정말로 필요한지 확인 가능하다.

### Item 20. 다른 타입에는 다른 변수 사용하기
- "변수의 값은 바뀔 수 있지만, 그 타입은 보통 바뀌지 않는다."

```ts
let id = '12-34-56';
id = 123456; // error!

// union type
let id2: string|number = '12-34-56';
id2 = 123456;
```
- union 타입으로 코드 에러는 방지할 수 있지만, 해당 변수를 사용할 때마다 타입을 확인해야 하므로 다루기 더 어려워진다.
- 차라리 별도의 변수를 도입해라.

- shadowed 변수와 구별해야 한다.
```ts
const id = '12-34-56';
{
    const id=123456;
}
```
- 두 id 는 이름만 같고 전혀 관계 없다. 대부분 린터 규칙을 사용해 shadowed 변수를 사용하지 못하도록 한다.

### Item21. 타입 넓히기
- 런타임에는 모든 변수가 유일한 값을 가지지만, 타입스크립트가 체크하는 정적 분석 시점에는 변수는 '가능한' 값들의 집합인 타입을 가진다.
- 타입 넓히기
    - 지정된 단일 값을 가지고 할당 가능한 값들의 집합을 유추하는 것

```ts
interface Vector3 {x : number; y : number; z : number;}
function getComponent(vector : Vector3, axis : 'x' | 'y' | 'z'){
    return vector[axis];
}

let x = 'x';
let vec = {x : 10, y : 20, z : 30};
getComponent(vec, x); // error! Argument of type 'string' is not assignable to parameter of type '"x" | "y" | "z"'.
```

- 위 코드는 실행은 되지만, 편집기에서 오류가 발생한다.
- x 타입은 할당 시점에 넓히기가 동작해서 string 으로 추론된다. string 타입은 "x" \| "y" \| "z" 타입에 할당이 불가능하다.

- 넓히기를 제어할 수 있는 방법들
    - let 대신 const 사용하면 더 좁은 타입이 된다. 재할당될 수 없으므로 더 좁은 타입으로 추론이 가능하기 때문이다. 아래 코드는 여전히 해결 불가능하다.
    ```ts
    const mixed = ['x', 1]; // (string|number)[]
    ```
    - 객체의 경우 각 요소를 let 으로 할당된 것처럼 다룬다. 다른 속성을 추가할 수도 없다. 따라서 객체는 한번에 만드는 것이 좋다. 
    ```ts
    const v = {x : 1};
    v.x = 3; 
    v.x = '3'; // error!
    v.y = 4; // error! 
    ```

- 타입 추론의 강도를 제어하기 위해 기본 동작 재정의 방법
    1. 명시적 타입 구문 제공
    2. 타입 체커에 추가적인 문맥 제공
    3. const 단언문 사용
        - as const 사용하면 최대한 좁은 타입으로 추론
    ```ts
    const v1 = {
        x : 1 as const,
        y : 2,
    } // {x : 1, y : number}
    const v2 = {
        x : 1, 
        y : 3,
    } as const;  // {readonly x : 1; readonly y : 2}
    ```

### Item 22. 타입 좁히기
- 타입스크립트가 넓은 타입으로부터 좁은 타입으로 진행하는 과정
- 타입 좁히는 방법
  - 분기문
  - instanceof
  - 속성 체크
  - Array.isArray 와 같은 일부 내장 함수
  - 명시적 태그 붙이기 - tagged union, discriminated union

```ts
// null check
const el = document.getElementById('foo'); // HTMLElement | null
if(el){
    el // HTMLElement
    el.innerHTML = 'Party time'.blink();
}else{
    el // null
    alert('no element'); 
}

// instanceof
function contains(text : string, search : string|RegExp){
    if(search instanceof RegExp){
        search // RegExp
        return !!search.exec(text);
    }
    search // string
    return text.includes(search);
}
```

- 잘못된 타입좁히기 예시
    - typeof null = 'object'
    - '', 0 = false



```ts
const el = document.getElementById('foo'); // HTMLElement | null
if(typeof el === 'object') el; // HTMLElement | null

function foo(x? : number|string|null){
    if(!x){
        x; // string|number|null|undefined
    }
}
```

```ts
// tagged union, discriminated union
interface UploadEvent {type : 'upload'; ...};
interface DownloadEvent {type : 'download'; ...};
type AppEvent = UploadEvent | DownloadEvent;
function handleEvent(e : AppEvent){
    switch(e.type){
        case 'download':
            ...
        case 'upload':
            ...
    }
}
```

- 사용자 정의 가드 타입
  - 타입스크립트가 타입을 식별하지 못하면, 식별을 돕기 위해 커스텀 함수를 도입할 수 있다.

```ts
function isInputElement(el : HTMLElement) : el is HTMLInputElement {
    return 'value' in el;
}

function getElementContent(el : HTMLElement){
    if(isInputElement(el)){
        el; // HTMLInputElement
        return el.value;
    }
    el; // HTMLElement
    return el.textContent;
}
```

```ts
const arr = ['a', 'b', 'c', 'd', 'e'];
const mem = ['f', 'g'].map(item => arr.find(n => n === item)); // (string|undefined)[]

// 1. filter - not working!
const mem = ['f', 'g'].map(item => arr.find(n => n === item)).filter(item => item !== undefined); // (string|undefined)[]

// 2. 사용자 정의 타입 가드
function isDefined<T>(x : T|undefined): x is T{
    return x !== undefined;
}
const mem = ['f', 'g'].map(item => arr.find(n => n === item)).filter(isDefined); //string[] 
```