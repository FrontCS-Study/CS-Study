# Hooks

> **Hooks 는 리액트 버전 16.8부터 React 요소로 새로 추가되었습니다. Hook을 이용하여 기존 Class 바탕의 코드를 작성할 필요 없이 상태 값과 여러 React의 기능을 사용할 수 있습니다.** - React 공식문서

### 🙄 Hook의 등장배경

##### 1️⃣ Class 컴포넌트 vs Function 컴포넌트

리액트의 컴포넌트는 클래스형과 함수로 나누어진다. hook이 나오기 전까지는 state접근, life Cycle 을 사용하기 위해 클래스형 컴포넌트 선언을 해주어야 했다. 함수형 컴포넌트는 한 번 호출되고 메모리 상에서 사라져 상태값 접근과 라이프사이클 구현이 불가능 했다. 그러나 클래스형 컴포넌트는 아래와 같은 문제점이 있었다.

##### 2️⃣ Class형 컴포넌트의 문제점

1. 컴포넌트 간에 로직의 재사용이 불가능하다.
   - render props나 HOC(High Order Component, 고차 컴포넌트) 와 같은 패턴을 통해 문제를 해결할 수 있지만 이러한 패턴은 코드의 추적을 어렵게 만든다.
   - 계속해서 이런 코드를 사용하면 많은 레이어로 둘러싸인 wrapper hell을 겪게된다.
2. 코드가 복잡해진다.

### 💡 Hooks 의 등장

리액트 hook은 React 16.8버전에 새로 추가된 기능으로, **함수형 컴포넌트에서 React state 와 life cycle 기능을 연동(hook into)할 수 있게 해준다.**
useState를 예시로 들면 class를 사용하지 않아도 상태를 가질 수 있게 된 것이다. (hook은 class 안에서는 동작하지 않는다.)

리액트는 useState, useEffect 같은 내장 Hooks을 제공하며, props, state, context, refs 그리고 lifeCycle과 같은 리액트 개념에 좀 더 직관적인 API를 제공한다. 
컴포넌트 간에 상태 관련 로직을 재사용하기 위해 hook을 직접 만드는 것도 가능하다.

### 📌 hooks의 규칙

##### 1️⃣ 최상위에서만 hook을 호출해야 한다. hook을 호출하는 순서는 항상 같아야 한다.

- 반복문, 조건문, 중첩된 함수 내에서 hook을 사용하면 안된다.
- 왜 hook의 호출 순서가 같아야 할까?
  - 리액트가 상태 값을 구분할 수 있는 유일한 정보는 hook이 사용된 순서이기 때문이다. 리액트가 hook이 호출된 순서에 의존한다는 것이다.
  - 예시로 반복문 안에서 hook을 호출했을 때 반복문이 true라면 괜찮겠지만 값이 false라면 건너뛰게 된다. 이렇게 하면 실행 순서가 바뀔 수 있어 오류를 일으킨다.
  - 조건문 혹은 반복문을 사용하고 싶을 때는 useEffect 안에 넣어서 사용하면 된다.

##### 2️⃣ React 함수 컴포넌트 내에서만 Hook을 호출해야 한다.

- 일반적인 JS함수에서는 Hook을 호출하면 안된다.
- 직접 작성한 custom hook에서는 사용이 가능하다.



### 1. useState

useState는 가장 기본적인 Hook으로서, 함수 컴포넌트에서도 가변적인 상태를 지닐 수 있게 해준다. 만약 함수형 컴포넌트에서 상태를 관리해야 하는 일이 발생한다면 이 hook을 사용하면 된다.

```react
const [value, setValue] = useState(0);
// const [읽기용 값, 쓰기용 함수] = useState(초기값);
```

### 2. useEffect

useEffect는 리액트 컴포넌트가 렌더링 될 때마다 특정 작업을 수행하도록 설정할 수 있는 hook이다. 클래스형 컴포넌트의 `componentDidMount`와 `componentDidUpdate`를 합친 형태로 보아도 무방하다.

```react
useEffect(콜백함수, 의존성배열)
useEffect(() => {
    실행할 코드;
}, [감시할 대상])
```

- **`useEffect`**

  - useEffect 는 생명주기 함수에서 `componentMount`와 같다.
  - [] : 의존성 배열이 비어있으면 렌더링 후 무조건 실행이 한 번만 되고 그 이후에는 실행되지 않는다.
  - [count] : 안에 변수가 있으면 변수 값이 변경될 때마다 함수가 호출된다.

- **`useEffect(return문)`**

  - useEffect에 return문이 있으면 그 return문은 `componentUnmount`와 같다.
  - return문이 있을 때 [] 의존성 배열이 비어있으면 컴포넌트가 제거될 때 (unmount) return문이 호출된다.
  - return문이 있을 때 [count] 안에 변수가 있으면 언마운트 때는 물론 그 변수의 업데이트 직전에 호출된다.

  ```react
  // 배열이 생략된다면 리렌더링 때마다 실행
  useEffect(() => {
      if(count > 10) setCount(0)
  })
  // []가 비어있으면 렌더링 후 무조건 실행, 그 이후에는 실행되지 않는다.
  // DidMount와 같은 의미
  useEffect(() => {
      if(count > 10) setCount(0)
  }, [])
  // [감시대상] 가 있을 경우 해당 변수 값이 변경될 때마다 함수 호출(리렌더링)
  useEffect(() => {
      if(count > 10) setCount(0)
  }, [count])
  
  
  // useEffect 안에 return 이 있으면 compoenetWillUnmount와 같이 행동된다.
  // 컴포넌트가 unmonut 되거나 업데이트 되어서 리렌더링 될 때마다 호출된다.
  
  // claenup 함수 반환
  // 언마운트 될 때만 cleanup 함수를 하고 싶으면 배열을 빈값으로 선언
  useEffect(() => {
      if(count > 10) setCount(0)
      return () => {
          console.log("aaa")
      }
  }, [])
  // 특정값이 업데이트 되기 직전 호출 하고 싶으면 배열에 특정 값을 넣으면 된다.
  useEffect(() => {
      if(count > 10) setCount(0)
      return () => {
          console.log("aaa")
      }
  }, [count])
  ```

### 3. useLayoutEffect

> useEffect로 전달된 함수는 layout과 paint가 완료된 후에 **비동기적**으로 실행된다.
>
> 이때 만약 DOM에 영향을 주는 코드가 있을 경우, 사용자는 화면의 깜빡임과 동시에 화면 내용이 달라지는 것을 볼 수 있다. 중요한 정보일 경우, 화면이 다 렌더되기 전에 동기화 해주는 것이 좋은데, 이를 위해 useLayoutEffect라는 hook이 나왔으며, 기능은 동일하되 실행 시점만 다르다.
>
> React 18부터는 useEffect에서도 layout 과 paint 전에 동기적으로 함수를 실행할 수 있는 flushSync 라는 함수가 추가되었다. 하지만 강제로 실행하는 것이다 보니, 성능상 이슈가 있을 수 있다.

[useLayoutEffect](https://ko.reactjs.org/docs/hooks-reference.html#uselayouteffect)는 useEffect와 동일하지만, 렌더링 후 layout과 paint 전에 **동기적**으로 실행된다. 때문에 설령 DOM을 조작하는 코드가 존재하더라도, 사용자는 깜빡임을 보지 않는다.

스크롤 위치를 찾거나 어떤 element의 스타일 요소를 변경하는 등 직접적으로 DOM을 조작하는 곳 제외하고는 useEffect를 사용하는 것을 추천한다. 공식 문서에서도 useEffect를 먼저 사용한 후에, 문제가 생긴다면 그때 useLayoutEffect를 사용하는 것을 추천하고 있다.

### 4. useRef

useRef 는 함수형 컴포넌트에서 ref를 쉽게 사용할 수 있게 해주는 hook이다.

useState나 useEffect 처럼 많이 쓰이지는 않지만 어떤 특정 DOM을 직접 선택해 포커스를 주거나 특정 엘리먼트의 크기나 색상을 변경하는 경우 쓰인다.
단, Reat 개념 또는 특징 상 특별한 경우를 제외하고는 DOM을 직접 접근해 사용하는 것은 올바른 사용법이 아니라고 말한다.

```react
import { useRef } from 'react';

export default function Day05A4() {
    const inputPrev = useRef();
    const inputNext = useRef();
    
    const onNext = () => {
        inputNext.current.focus()
    }
    
    const onPrev = () => {
        inputPrev.current.focus()
    }
    
    // 렌더링이 되고 리턴이 되고 나서 밑에 있는 input을 접근할 방법이 없다.
    // 하지만 useRef를 사용하면 접근 가능하다.
    return (
        <div>
        	<div>
            	<input type="text" ref={inputPrev}/>
                <button conClick={onNext}>다음</button>
            </div>
            <div>
            	<input type="text" ref={inputNext}/>
                <button conClick={onPrev}>이전</button>
            </div>
        </div>
}
// 렌더링이 되고 리턴이 되고 나서 input 을 접근할 방법이 없기 때문에 useRef를 통해 접근한다.
// 접근하고 싶은 태그에 속성으로 ref로 useRef를 연결시켜줘야 한다.
// useRef는 .current 프로퍼티로 전달된 인자로 초기화된 변경 가능한 ref 객체를 반환한다.
// 그리고 .current 프로퍼티를 변경하더라도 이 것이 리렌더링을 발생시키진 않는다.
// 값이 변경되더라도 render를 발생시키지 않고, render 되더라도 값이 유지되는 특징을 가진다.
// 위에는 다음이나 이전을 클릭할 때마다 커서 포커스를 이동하는 useRef이다.
```

### 5. useReducer

useReducer는 useState보다 컴포넌트에서 더 다양한 상황에 따라 다양한 상태를 다른 값으로 업데이트 해주고 싶을 때 사용하는 hook이다. 

리듀서는 현재 상태와, 업데이트를 위해 필요한 정보를 담은 액션(action) 값을 전달 받아 새로운 상태를 반환하는 함수이다. 리듀서 함수에서 새로운 사앹를 만들 때는 꼭 불변성을 지켜주어야 한다.

```react
function reducer(state, actoin) {
    return {...}; // 불변성을 지키면서 업데이트한 새로운 상태를 반환한다.
}
```

액션 값은 주로 다음과 같은 형태로 이루어져있다.

```json
{
	type: 'INCREMENT',
    // 다른 값들이 필요하면, 추가적으로 들어간다.
}
```

Redux에서는 액션 객체에는 어떤 액션인지 알려 주는 type 필드가 꼭 있어야 하지만, useReducer에서 사용하는 액션 객체는 꼭 type을 지니고 있을 필요가 없다. 심지어, 객체가 아니라 문자열이나, 숫자여도 상관 없다.

### 6. useMemo

useMemo를 사용하면 함수형 컴포넌트 내부에서 발생하는 연산을 최적화 할 수 있다. 

Memo는 Memoization을 뜻하는 말로 기존에 수행한 연산의 결과 값을 어딘가에 저장해주고 동일한 입력이 들어오면 재활용하는 프로그래밍 기법을 뜻한다.

```react
// 사용방법
useMemo(콜백함수, 의존성 배열)

const value = useMemo(() => {
    return calculate();
}, [item])
```

### 6. useCallback

useCallback은 useMemo와 상당히 비슷하다. 주로 렌더링 성능을 최적화 해야하는 상황에서 사용한다. 이 hook을 사용하면 이벤트 핸들러 함수를 필요할 때만 생성할 수 있다.

useMemo는 특정 결과 값을 재사용할 때 사용하는 반면, useCallback은 특정 함수를 새로 만들지 않고 재사용 하고 싶을 때 사용한다.

```react
// 사용방법
useCallback(콜백함수, 의존성배열)

const value = useCallback(() => {
    return calculate();
}, [item])
```

### 7. React.memo

컴포넌트의 props가 바뀌지 않았다면 리렌더링을 방지해 컴포넌트의 리렌더링 성능을 최적화하는 hook이다.

리액트에서 변경된 바로 그 컴포넌트 DOM에 업데이트를 하게 해주는 memo기능으로, 특정 부분을 기억하고 변경이 발생했을 때 이전 렌더와 변경된 렌더를 비교한다.
만약 같다면 DOM을 업데이트 하지 않고 만약 다르다면 DOM을 업데이트 한다.





## Reference

- [Hook의 개요](https://ko.legacy.reactjs.org/docs/hooks-intro.html)
- [React Hooks](https://velog.io/@velopert/react-hooks)
- [[React] Hooks란?](https://velog.io/@niboo/React-Hooks-%EB%9E%80)
- [[react] 리액트 훅(react hook)이란? - 클래스형 컴포넌트와 비교](https://codingbroker.tistory.com/23)
- [[React] 리액트 훅(React Hooks)](https://tadaktadak-it.tistory.com/159)

- [useEffect와 useLayoutEffect의 차이](https://www.howdy-mj.me/react/useEffect-and-useLayoutEffect)