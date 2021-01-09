---
title : "Asynchronous Programming (Chapter 42)"

categories :
    - javascript

tags :
    - javascript

---
  ** 모던 자바스크립트 Deep Dive 책을 읽고 정리한 포스팅이다.

### 동기 처리와 비동기 처리
- 함수가 호출되면 함수 코드가 평가되어 함수 실행 컨텍스트가 생성된다. 이 함수 실행 컨텍스트는 실행 컨텍스트 스택(call stack)에 푸시되고 함수 코드가 실행된다. 함수 코드 실행이 종료되면 함수 실행 컨텍스트는 실행 컨텍스트 스택에서 pop되어 제거된다.
- 실행 컨텍스트 스택에 함수 컨텍스트 스택이 푸시되는 것은 바로 함수 실행의 시작을 의미한다.
- 자바스크립트 엔진은 단 하나의 실행 컨텍스트 스택을 갖는다.
  - 함수를 실행할 수 있는 창구는 단 하나이며,
  - 동시에 2개 이상의 함수를 동시에 실행할 수 없다는 것을 의미한다.
- 자바스크립트 엔진은 single thread 방식으로 동작한다. 한 번에 하나의 태스크만 실행하므로 처리에 시간이 걸리는 태스크를 실행하는 경우 blocking 이 발생한다.

```javascript
//Synchronous 처리
function sleep(func, delay){
    const delayUntil = Date.now() + delay;
    while(Date.now() < delayUntil);
    func();
}

function foo(){ console.log('foo')};
function bar(){ console.log('bar')};
sleep(foo, 3*1000);
bar();
//bar 함수는 sleep 함수의 실행이 종료된 이후에 호출되므로, 3초 이상 blocking 된다.
```

```javascript
//Asynchronous 처리
function foo(){ console.log('foo')};
function bar(){ console.log('bar')};
//setTimeout은 일정 시간이 경과한 이후에 콜백 함수 foo 를 호출한다. blocking 하지 않는다.
setTimeout(foo, 3*1000);
bar();
//bar 호출 -> (3초 경과 후) foo 호출
```

- 동기 처리
  - 실행 순서가 보장된다. 앞선 태스크가 종료할 때까지 이후 태스크들이 블로킹된다.
- 비동기 처리
  - 실행 순서 보장되지 않는다. 블로킹이 발생하지 않는다.
  - 비동기 함수는 전통적으로 **콜백 패턴**을 사용한다.
    - 콜백 지옥 - 나쁜 가독성, 에러의 예외처리가 어려움. 여러 개의 비동기 처리 한번에 처리 어려움 -> 프로미스 탄생
  - setTimeout, setInterval, HTTP 요청, 이벤트 핸들러는 비동기 처리 방식으로 동작한다.


### 이벤트 루프와 태스크 큐
- 자바스크립트 엔진은 싱글 스레드로 동작한다. 하지만, 브러우저가 동작하는 것을 살펴보면 많은 태스크가 동시에 처리되는 것처럼 느껴진다.
- Event loop
  - 브라우저에 내장된 기능으로, 자바스크립트의 concurrency 를 담당한다.
- 자바스크립트 엔진
  - call stack
    - 실행 컨텍스트 스택. 자바스크립트 엔진은 단 하나의 콜 스택을 사용한다.
  - heap
    - 객체가 저장되는 메모리 공간으로 실행 컨텍스트는 힙에 저장된 객체를 참조한다. 객체는 메모리 공간의 크기를 런타임에 결정(동적 할당)해야 한다. 
- js 엔진은 단순히 태스크가 요청되면 콜 스택을 통해 요청된 작업을 순차적으로 실행할 뿐이다. **비동기 처리에서 소스코드의 평가와 실행을 제외한 모든 처리**는 자바스크립트 엔진을 구동하는 환경인 브라우저나 Node.js 가 담당한다.

- browser
  - task queue/event queue/callback queue
    - 비동기 함수의 콜백 함수나 이벤트 핸들러가 일시적으로 보관되는 영역이다. 
  - event loop
    - call stack에 현재 실행 중인 실행 컨텍스트가 있는지, task queue에 대기 중인 함수들이 있는지 반복해서 확인한다. call stack이 비어있고, task queue에 대기 중인 함수가 있다면 event loop 는 First in first out 으로 task queue 에 대기 중인 함수를 call stack 으로 이동시킨다. 이 때 call stack 으로 이동한 함수는 실행된다. 즉, task queue 에 있는 함수들은 비동기 처리 방식으로 동작한다.

```javascript
function foo(){ console.log('foo')};
function bar(){ console.log('bar')};
setTimeout(foo, 0);
bar();
```
1. 전역 코드가 평가되어 전역 실행 컨텍스트가 생성되고 call stack에 푸시된다.
2. setTimeout 함수 호출된다. 이 함수가 현재 실행 중인 실행 컨텍스트가 된다. 
3. setTimeout 함수가 실행되면 callback 함수를 호출 스케줄링하고 종료되어 call stack에서 pop된다. 호출 스케줄링, 즉 타이머 설정과 타이머가 만료되면 callback 함수를 task queue에 푸시하는 것이 브라우저의 역할이다.
4. (1) 브라우저는 타이머 설정하고 만료 기다린다. 만료되면 foo가 task queue에 푸시된다. delay 0 이지만, 최소 지연 시간 4ms 가 설정된다. 따라서 **4ms 후에 foo가 태스크 큐에 푸시되어 대기한다.** 정확히 지연 시간 후에 호출된다는 보장이 없다. 콜 스택이 비어야 하므로 약간의 시간차는 존재한다.
4. (2) js 엔진은 bar 함수를 호출해 수행하고 call stack에서 pop된다. 이 때 브라우저가 타이머를 설정한 후 4ms 가 지났으면 foo 함수는 아직 task queue 에 있다.
5. 전역 코드 실행이 종료되고, 전역 실행 컨텍스트가 call stack 에서 pop된다. call stack이 비어있다.
6. event loop에 의해 call stack이 비어있음을 감지하고 task queue에서 대기 중인 foo 함수를 call stack에 푸시한다. 함수 실행 후 종료되고 call stack에서 pop된다.

- **싱글 스레드로 동작하는 것은 브라우저가 아니라 브라우저에 내장된 자바스크립트 엔진이다. 모든 자바스크립트 코드가 자바스크립트 엔진에서 싱글 스레드 방식으로 동작한다면 비동기로 동작할 수가 없다. 브라우저는 멀티 스레드로 동작한다.**

