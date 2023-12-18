## hook
함수 컴포넌트에서 **React state와 생명주기 기능을 연동(hook into)** 할 수 있게 하는 함수.

React 16.8 버전 이후 추가되었으며, hook의 등장으로 상태 관리를 위해 클래스 컴포넌트를 사용할 필요가 없어졌다.

### hook의 규칙
#### 1. 최상위에서만 hook을 호출해야 한다.
반복문, 조건문 혹은 중첩된 함수 내에서 hook을 호출하지 않는다. 

#### 2. 오직 React 함수 내에서 hook을 호출해야 한다.
hook을 일반적인 JavaScript 함수에서 호출할 수 없다.

<br/>

## hook의 종류
### 1. useState
`state` 값과 그 값을 갱신하는 함수를 반환한다.
최초로 렌더링을 하는 동안, 반환된 `state`는 첫 번째 전달된 인자(`initialState`)의 값과 같다.
`setState` 함수를 통해 `state`를 갱신할 수 있으며, 새 `state` 값을 받아 컴포넌트 리렌더링을 *큐에 등록*한다.

```jsx
const [state, setState] = useState(initialState);
```


### 2. useEffect
컴포넌트가 렌더링 될 때마다 특정 작업을 실행할 수 있도록 하는 hook.
컴포넌트의 마운트, 업데이트, 언마운트 시 함수를 실행할 수 있으며, 화면에 UI가 렌더링된 후 실행된다.

기존 클래스에서 사용하던 `componentDidMount`, `componentDidUpdate`, `componentWillUnmount`를 대체한다.

```jsx
useEffect(func, dependencies?)
```
- `func`: 수행하고자 하는 작업
- `dependencies`: 검사하고자 하는 특정 값, 혹은 빈 배열

#### 업데이트 (조건부 effect)
effect를 수행하고, 이를 **한 번만 실행**하고 싶을 때에는 `dependencies`에 빈 배열(`[]`)을 전달한다.

props나 state에서 가져온 어떤 값에도 의존하지 않으므로, 다시 실행할 필요가 없다는 것을 의미한다.

```jsx
useEffect(() => {
	// code 
	...
}, [])
```

**`props.source`가 변경될 때마다** 함수를 실행한다.

```jsx
useEffect(() => {
	// code 
	...
}, [props.source])
```

#### 언마운트
메모리 누수 방지를 위해 UI에서 컴포넌트를 제거하기 전에 수행된다.

```jsx
useEffect(() => {
	const subscription = props.source.subscribe();
	return () => {
		// clean up the subscription
		subscription.unsubscribe();
	}
})
```


### 3. useReducer
`useState`의 대체 함수.
`(state, action) => newState`의 형태로 reducer를 받고 `dispatch` 메소드와 짝의 형태로 현재 `state`를 반환한다.
컴포넌트의 상태 업데이트 로직을 컴포넌트 로직에서 분리시킬 수 있다.

1) 다수의 하위값을 포함하는 복잡한 정적 로직을 만드는 경우나,
2) 다음 state가 이전 state에 의존적인 경우
`useState`보다 `useReducer`를 선호하여 사용한다.

```jsx
const [state, dispatch] = useReducer(reducer, initialArg, init);
```

#### useState vs. useReducer
1) useState

```jsx
function Counter({initialCount}) {
  const [count, setCount] = useState(initialCount);
  return (
    <>
      Count: {count}
      <button onClick={() => setCount(initialCount)}>Reset</button>
      <button onClick={() => setCount(prevCount => prevCount - 1)}>-</button>
      <button onClick={() => setCount(prevCount => prevCount + 1)}>+</button>
    </>
  );
}
```

2) useReducer
```jsx
const initialState = {count: 0};

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}
```


### 4. useMemo
처음에 계산한 값을 메모리에 저장해, 컴포넌트가 계속 렌더링되어도 계산식을 재호출하지 않고 **메모리에 저장되어 있는 계산된 값을 가져와 재사용**할 수 있게 한다.

기존 함수 컴포넌트는 렌더링 -> 컴포넌트 함수 호출 -> **모든 내부 변수 초기화**의 순서를 거칠 때,

`useMemo`를 사용하여 렌더링 -> 컴포넌트 함수 호출 -> **Memoized된 함수를 재사용**하는 순서를 거친다.

> **useMemo는 꼭 필요한 상황에만 사용한다.**
> 
> 값을 재활용하기 때문에 따로 메모리를 소비하여 저장하는 것이기 때문에, 불필요한 값이 모두 Memoization 되면 오히려 성능이 안좋아질 수 있다.

```jsx
const memoizaedValue = useMemo(()=>{
	computedExpensiveValue(a, b),
}, [a, b]);
```
- `computedExpensiveValue(a, b)` : 콜백 함수
- `[a, b]` : 의존성 배열. **배열 안에 있는 값이 업데이트 될 때에만 콜백 함수를 다시 호출**하여 메모리에 저장된 값을 업데이트 한다.
	- 빈 배열을 넣는다면 `useEffect`와 마찬가지로 마운트 될 때에만 값을 계산하고, 그 이후론 계속 memoization된 값을 꺼내와 사용한다.

### 5. useCallback
특정 함수를 새로 만들지 않고 재사용하고 싶을 때 사용한다.

```jsx
const memoizedCallback = useCallback(() => {
	doSomething(a, b);
}, [a, b]);
```
- `doSomething(a, b)`: 콜백 함수
- `[a, b]`: 의존성 배열. 배열 안에 있는 값이 업데이트 될 때에만 콜백 함수를 다시 호출한다.

#### useCallback을 사용하는 경우
상위 컴포넌트에서 하위 컴포넌트에 전달하는 콜백 함수가 inline 함수로 사용되거나, 컴포넌트 내에서 함수를 생성하고 있다면 새로운 함수가 만들어지게 된다.
예를 들어, `Counter` 컴포넌트 안에 `increment` 함수들은 컴포넌트가 리렌더링 될 때마다 새로 만들어지게 된다.

함수를 재선언하는 것은 CPU나 큰 메모리를 차지하는 것은 아니지만, 한 번 만든 함수를 재사용하고, 필요할 때만 재생성하는 것은 중요하며, 예제에서 `useCallback`를 사용히여 컴포넌트를 최적화할 수 있다.

```jsx
import React, { useState } from "react";

const CounterButton = ({onClicks, count}) => {
	return <button onClick={onClicks}>{count.num}</button>
}

const Counter = () => {
	const [counte1, setCount1] = useState({ num: 0 });
	const increment1 = () => {
		setCount1({ num: count1.num + 1 });
	};
	
	const [counte2, setCount2] = useState({ num: 0 });
	const increment2 = () => {
		setCount2({ num: count2.num + 1 });
	};
	
	return (
		<div className="App">
		  <div>{count1.num}</div>
		  <div>{count2.num}</div>
		  <CounterButton onClicks={increament1} count={count1} />
		  <CounterButton onClicks={increament2} count={count2} />
		</div>
	)
}
```

위 코드에서 **버튼을 클릭하면 `CounterButton` 컴포넌트가 두 번 렌더링**되는데, 이는 props, state, 부모 컴포넌트의 변화가 있었기에 React가 리렌더링 된다.
1. 첫 렌더링이 되고, `increment` 함수와 count state가 생성되어 렌더링된다.
2. 버튼을 클릭하게 되면 `increment`가 작동하게 되고, `setState`로 인해 `state`가 변경된다.
3. `state`가 변경되었으니, 부모 컴포넌트는 리렌더링이 되고, 변경된 `props`를 내린다.
4. 자식 컴포넌트가 `props`를 받아 다시 렌더링한다.

#### useCallback 사용
React의 리렌더링을 방지하는 React.memo와 함께 사용한다.
결과로, 두 번씩 렌더링되던 `CounterButton` 컴포넌트가 한 번만 렌더링된다.
 
```jsx
import React, { useState, useCallback } from "react";

const CounterButton = React.memo(({onClicks, count}) => {
	return <button onClick={onClicks}>{count.num}</button>
});

const Counter = () => {
	const [counte1, setCount1] = useState({ num: 0 });
	const increment1 = useCallback(() => {
		setCount1({ num: count1.num + 1 });
	}, [count1]);
	
	const [counte2, setCount2] = useState({ num: 0 });
	const increment2 = useCallback(() => {
		setCount2({ num: count2.num + 1 });
	}, [count2]);
	
	return (
		<div className="App">
		  <div>{count1.num}</div>
		  <div>{count2.num}</div>
		  <CounterButton onClicks={increament1} count={count1} />
		  <CounterButton onClicks={increament2} count={count2} />
		</div>
	)
}
```


>**useMemo vs. useCallback**
>useMemo는 함수의 연산량이 많을 때 이전의 결과값을 재사용하기 위한 목적으로 사용한다.
>useCallback은 함수가 재생성되는 것을 방지하기 위한 목적으로 사용한다.



### 6. useRef
`useRef`는 저장공간 또는 DOM 요소에 접근하기 위해 사용하는 hook. 
`.current` 프로퍼티로 전달된 인자(`initialValue`)로 초기화된 변경 가능한 `ref`객체를 반환한다.

1. 저장공간(변수 관리)
컴포넌트는 state가 변할 때마다 다시 렌더링이 되면서 컴포넌트 내부 변수들이 초기화가 된다.
이 때 state 대신 ref 안에 값을 저장하면, **ref 값을 변경해도 컴포넌트는 다시 렌더링 되지 않으므로** 불필요한 렌더링을 막을 수 있다. 또한, **ref 안에 저장된 값은 변화되지 않고 그대로 유지**된다. 

2. DOM 요소에 접근
DOM 요소에 직접 접근하여 요소의 property를 관리할 수 있다.

```jsx
const refContainer = useRef(initialValue);
```


### 7. useLayoutEffect
`useLayoutEffect`는 컴포넌트가 렌더된 후 실행되며, 그 이후에 페인트의 과정을 거친다.
**모든 DOM 변경 후에 동기적으로 발생**하며, 페인트가 되기 전에 실행되기 때문에 DOM을 조작하는 코드가 존재하더라도 깜빡임을 경험하지 않는다.

`state`가 존재하며 해당 `state`가 조건에 따라 첫 페인팅 시 다르게 렌더링 되어야 할 때,
`useEffect` 사용 시 처음에 0이 보여지고, 이후에 리렌더링이 되며 화면에 깜빡거린다. 

```jsx
const Test = () => {
	const [value, setValue] = useState(0);

	useEffect(()=>{
		if (value === 0) {
			setValue(10 + Math.random() * 200);
		}
	}, [value]);

	return (
		<button onClick={()=>setValue(0)}>
			value: {value}
		</button>
	);
};
```

`useLayoutEffect` 사용 시 해당 깜빡임을 방지할 수 있다.

```jsx
const Test = () => {
	const [value, setValue] = useState(0);

	useLayoutEffect(()=>{
		if (value === 0) {
			setValue(10 + Math.random() * 200);
		}
	}, [value]);

	return (
		<button onClick={()=>setValue(0)}>
			value: {value}
		</button>
	);
};
```

로직이 복잡한 경우에는 사용자가 레이아웃을 보기까지 시간이 오래 걸리는 단점이 있다.
따라서 **data fetch, event handler, state reset등의 작업은 `useEffect`를 사용**한다.

<br/>

---
**Reference**

[Hooks API Reference](https://ko.legacy.reactjs.org/docs/hooks-reference.html)

[[React] useMemo란?](https://velog.io/@jinyoung985/React-useMemo%EB%9E%80)

[[React] useMemo 사용법 및 예제](https://itprogramming119.tistory.com/entry/React-useMemo-%EC%82%AC%EC%9A%A9%EB%B2%95-%EB%B0%8F-%EC%98%88%EC%A0%9C)

[[TIL] React.useCallback 이란?](https://velog.io/@rjsdnql123/TIL-React.useCallback-%EC%9D%B4%EB%9E%80)

[React.memo, useMemo, useCallback 역할 및 차이점](https://velog.io/@sunkim/React.memo-useMemo-useCallback-%EC%97%AD%ED%95%A0-%EB%B0%8F-%EC%B0%A8%EC%9D%B4%EC%A0%90)

[[React] useRef란?](https://velog.io/@jinyoung985/React-useRef%EB%9E%80)

[[React] useEffect 와 useLayoutEffect 의 차이는 무엇일까?](https://medium.com/@jnso5072/react-useeffect-%EC%99%80-uselayouteffect-%EC%9D%98-%EC%B0%A8%EC%9D%B4%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C-e1a13adf1cd5)
