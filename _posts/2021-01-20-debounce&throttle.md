---
title: "Debounce and Throttle"

categories:
  - javascript

tags:
  - javascript
---

- 모던 자바스크립트 Deep Dive 책을 읽고 정리한 포스팅이다.

- scroll, resize, input, mousemove, mouseover 같은 이벤트는 짧은 시간 간격으로 연속하여 발생한다. 이러한 이벤트에 바인딩된 이벤트 핸들러는 과도하게 호출되어 성능에 악영향을 줄 수 있다.
  debounce 와 throttle 함수를 사용해 이 문제를 해결할 수 있다.

### Debounce

- 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간이 경과한 후에 이벤트 핸들러가 한 번만 호출되도록 한다. 이벤트들을 그룹화해서 마지막에 한 번만 이벤트가 발생하도록 한다.

```javascript
const input = document.querySelector("input");
const msg = document.querySelector(".msg");

const debounce = (callback, delay) => {
  let timerId;
  return (event) => {
    if (timerId) clearTimeout(timerId);
    timerId = setTimeout(callback, delay, event); // (콜백함수, 타이머 만료 시간, 콜백함수의 인자1, 2, ...);
  };
};

input.oninput = debounce((e) => {
  msg.textContent = e.target.value;
}, 300);
```

- 위 예제의 코드는 간략하게 구현된 것으로 완전하지 않다. 실무에서는 lodash debounce 함수나 Underscore의 debounce 함수 사용 권장한다.

#### 함수 호출 성공 여부도 알고 싶다면?

- debounce 함수는 setTimeout 을 사용해서 비동기 프로그래밍이 필요하다.

### Throttle

- 짧은 시간 간격으로 이벤트가 연속해 발생하더라도 **일정 시간 간격으로 이벤트 핸들러가 최대 한 번만 호출되도록 한다.** 연속적으로 발생한 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만든다.
- delay 시간 간격으로 콜백 함수를 호출한다.

```javascript
const throttle = (callback, delay) => {
  let timerId;
  return (event) => {
    if (timerId) return;
    timerId = setTimeout(
      () => {
        callback(event);
        timerId = null;
      },
      delay,
      event
    );
  };
};

container.addEventListener(
  "scroll",
  throttle((e) => {
    msg.textContent = e.target.value;
  }, 100)
);
```

- scroll 이벤트 처리나 **infinite scrolling UI 구현** 등에 유용하게 사용된다.
  실무에서는 Lodash, Underscore 의 throttle 함수 사용 권장한다.
