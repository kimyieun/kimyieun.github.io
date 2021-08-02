---
title: "React Toolkit"

categories:
  - react

tags:
  - react-toolkit
---

## Writing Reducers with Immer
- Redux toolkit 의 createReducer,  createSlice 는 기본적으로 Immer 를 사용하도록 되어 있다.
- 그렇다면 Immer 은 언제, 왜 사용해야 할까?

### Reducers and Immutable Updates
- Redux 의 3가지 규칙 중 하나는 *reducers 는 original/current state values 를 변경해서는 안된다.* 는 것이다.


```javascript
state.value = 123 // Illegal !

return { // legal !
    ...state,
    value : 123,
}
```

- Why?
  - UI 가 가장 최신 값을 사용해 적절하게 업데이트를 하지 않을 수 있다.
  - state 가 어떻게 업데이트 되었는지 추적하기 어렵다.
  - test 작성하기 어렵다.
  - time-travel debugging 을 올바르게 사용하기 어렵다.
  - redux 가 의도하는 사용 패턴과 spirit 에 위배된다.

### Immutability
- javascript objects, arrays 는 모두 mutable 하다. 
- **값을 immutably 변경하기 위해서는 기존의 objects/arrays 의 복사본을 생성하여 그것을 변경해야 한다.**
  - javascript array/object spread operators 를 사용하면, 기존의 array/object 의 mutation 된 복사본을 반환한다.


```javascript
const obj1 = {
    a : {c : 3},
    b : 2
}

const obj2 = {
    ...obj1,
    a : {
        ...obj1.a,
        c : 42
    }
}
// {
//     a : { c : 42},
//     b : 2
// }
```

- 그렇다면, spread operators 나 다른 function 을 사용해서 immutable update 코드를 작성하면 되나?
  - 문제가 있다. **nested data** 의 경우!
  - nesting 된 level 마다 copy 를 생성해야 한다. 일일이 코드를 작성해 복사하면 될 수도 있지만, depth 를 사전에 알 수 없는 경우에는? 미리 알더라도 실수하기도 너무 쉽다.
- **Accidentally mutating state in reducers is the single most common mistake Redux users make.**

### Immutable Updates with Immer
- 이제 Immer 를 사용해야 하는 이유는 알았다. 어떻게 사용하면 될까?
- produce function - original state와 callback function 을 인자로 받는다.

```javascript
import produce from 'immer';

const baseState = [
    {
        todo : 'learn typescript',
        done : true,
    },
    {
        todo : 'try immer',
        done : false,
    }
];

const nextState = produce(baseState, (draftState) => {
    draftState.push({todo : 'tweet about it'})
    draftState[1].done = true;
});

console.log(baseState === nextState); // false
console.log(baseState[0] === nextState[0]); // true
console.log(baseState[1] === nextState[1]); // false
```

### Redux Toolkit and Immer
- Redux toolkit 의 crateReducer API 는 Immer 를 내부에서 자동으로 사용한다. 

```javascript
const todosSlice = createSlice({
  name: 'todos',
  initialState: [],
  reducers: {
    todoAdded(state, action) {
      state.push(action.payload)
    },
  },
})
```

### Immer 사용 방법
### 1. Immer 은 당신이 기존의 state 를 mutation 하거나, 새로운 state 를 생성하여 반환하는 작업 중 하나만 수행할 것이라 기대한다. 그러므로 하나의 function 에서 두 개를 동시에 수행하면 안된다.

```javascript
const todosSlice = createSlice({
  name: 'todos',
  initialState: [],
  reducers: {
    todoAdded(state, action) {
      // "Mutate" the existing state, no return value needed
      state.push(action.payload)
    },
    todoDeleted(state, action.payload) {
      // Construct a new result array immutably and return it
      return state.filter(todo => todo.id !== action.payload)
    }
  }
})
```

- 화살표 함수에서 state 를 mutation 하는 것은 주의해야 한다. 암묵적으로 return 을 할 수 있기 때문에 error 발생할 수 있다.

```javascript
const todosSlice = createSlice({
  name: 'todos',
  initialState: [],
  reducers: {
    // ❌ ERROR: mutates state, but also returns new array size!
    brokenReducer: (state, action) => state.push(action.payload),
    // ✅ SAFE: the `void` keyword prevents a return value
    fixedReducer1: (state, action) => void state.push(action.payload),
    // ✅ SAFE: curly braces make this a function body and no return
    fixedReducer2: (state, action) => {
      state.push(action.payload)
    },
  },
})
```

- nested immutable update 하는 것은 어렵다. spread operator 를 사용해서 여러 개의 field 를 한 번에 업데이트하거나 각각의 field 를 assign 하는 방식을 사용한다.
- Object.assign 을 사용하면 한번에 multiple fields mutation 할 수 있다.

```javascript
function objectCaseReducer3(state, action) {
  const { a, b, c, d } = action.payload
  Object.assign(state, { a, b, c, d })
}
```

- state 를 replace 하거나 초기값으로 리셋하기 위해서는 주의가 필요하다. state 에 assign 하지 않고, 바로 return 해야 변경 가능하다.

```javascript
const initialState = []
const todosSlice = createSlice({
  name: 'todos',
  initialState,
  reducers: {
    brokenTodosLoadedReducer(state, action) {
      // ❌ ERROR: does not actually mutate or return anything new!
      state = action.payload
    },
    fixedTodosLoadedReducer(state, action) {
      // ✅ CORRECT: returns a new value to replace the old one
      return action.payload
    },
    correctResetTodosReducer(state, action) {
      // ✅ CORRECT: returns a new value to replace the old one
      return initialState
    },
  },
})
```

- state 를 디버깅하고 싶다면?
- console.log(state) 를 실행하면 브라우저는 logged Proxy instances 를 보여준다. 보기 어렵고 해석도 어렵다.
- Immer 은 wrapped data 의 copy 를 추출하는 current function 을 제공한다. 

```javascript
import { current } from '@reduxjs/toolkit'

const todosSlice = createSlice({
  name: 'todos',
  initialState: todosAdapter.getInitialState(),
  reducers: {
    todoToggled(state, action) {
      // ❌ ERROR: logs the Proxy-wrapped data
      console.log(state)
      // ✅ CORRECT: logs a plain JS copy of the current data
      console.log(current(state))
    },
  },
})
```

- 이 외에도 original, isDraft function 을 제공한다.
  - Original function - draft 가 original state 와 같은지 확인
  - isDraft - proxied instance 인지 알고 싶을 때 사용한다. boolean 반환

