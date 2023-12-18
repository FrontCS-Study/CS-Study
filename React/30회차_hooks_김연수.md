[TOC]

# React Hook

리액트 컴포넌트는 '클래스형'과 '함수형'으로 나뉘어진다.

기존에는 state이나 Life Cycle Method를 사용을 위해 클래스형 컴포넌트를 사용하는 방식이었다.  
하지만 **클래스형 컴포넌트**에는 몇 가지 **단점**이 존재한다.

• 클래스 문법이 어렵다.  
• 코드 재사용성이 떨어지고 코드 구성이 어렵다.  
• 코드의 복잡성 : 함수형 컴포넌트에 비해 코드가 상대적으로 길고 복잡하다. 이로 인해 가독성이 저하된다.  
• 연관성이 없는 로직을 생명주기 메서드 하나에 구현하는 경우가 많다.  

리액트 16.8 버전 이후, Hook이 등장하면서 함수형 컴포넌트에서도 상태 관리 뿐만 아니라 기존 클래스형 컴포넌트에서만 가능하던 여러 기능을 사용할 수 있게 되었다.

> 🔍 **함수형 컴포넌트 Hook 사용에도 재현할 수 없는 메서드**
>
> - getSnapshotBeforeUpdate
>
> - getDerivedStateFromError
> - componentDidCatch

## Hook

Hook이란, **함수형 컴포넌트에서 State와 Lifecycle 기능을 연동해주는 함수**이다.

'클래스형'에서는 동작하지 않으며, '함수형'에서만 사용 가능하다.  

### Hook의 규칙

Hook에는 규칙이 있다. 이를 꼭 지켜야 정상적으로 hook이 실행되고 코드가 꼬이지 않는다.

1. **최상위에서만 Hook을 호출**

   - React 함수(컴포넌트)의 최상위에서만 Hook을 호출 할 것.

   - 반복문, 조건문, 중첩된 함수등에서 호출 불가능

2. **React 함수에서만 Hook을 호출**

   - 함수형 컴포넌트, Custom Hook에서만 호출 가능하다.

   - 일반적인 Javascript 함수에서는 호출 불가능

3. **React는 Hook 호출되는 순서에 의존**

   - 한 컴포넌트에서 여러개의 hook이 사용되는 경우

   - hook은 위에서부터 아래로 순서에 맞게 동작한다.

### Hook의 종류

> **기본 Hook**
>
> 1\. useState (동적 상태 관리)  
> 2\. useEffect (side effect 수행, 컴포넌트 렌더링 후에 발생 -mount/unmount/update )  
> 3\. useContext (컴포넌트를 중첩하지 않고도 전역 값 쉽게 관리)  
>
> **추가 Hook**
>
> 4\. useReducer (복잡한 컴포넌트들의 state를 관리 -분리)  
> 5\. useMemo (연산한 값 재사용)    
> 6\. useCallback (특정 함수 재사용)    
> 7\. useRef (DOM선택, 컴포넌트 안에서 조회/수정할 수 있는 변수 관리)  
> 8\. useLayoutEffect (컴포넌트 렌더링 전에 발생)  
>
> **커스텀 Hooks**
>
> 9\. Custom Hook

#### 1. useState

useState는 가장 기본적인 훅이며, **상태를 관리**하는 훅이다.

• 함수형 컴포넌트 안에 state를 추가하여 사용한다.  
• 현재 상태를 나타내는 state값과 이 상태를 변경하는 setState값을 한 쌍으로 제공한다.  
• state는 초기값을 설정할 수 있으며 초기값은 첫 렌더링 때 한번 사용된다.  
• state는 객체일 필요 없이 문자열, 숫자, boolean, 배열, null, 객체 등의 여러가지 다양한 값을 넣을 수 있다.

```react
// const [읽기용 값, 쓰기용 함수] = useState(초기값);
const [count, setCount] = useState(1); 
```

#### 2. useEffect

useEffect는 리액트 컴포넌트가 **리렌더링 될 때마다 특정한 작업**을 수행하도록 설정할 수 있게 하는 훅이다.  
컴포넌트가 렌더링 되고 화면에 **layout 배치와 paint 후에 비동기적으로 실행**한다.

클래스형 컴포넌트에서 사용했던 `componentDidMount, componentDidUpdate, componentWillUnmount`를 통합한 것이다.

• useEffect는 기본적으로 useEffect(function, deps)의 형태를 사용한다.  
• function에는 실행시킬 함수를 넣고 deps에는 의존성 배열을 담는다.  

**의존성 배열**

• 의존성 배열 없이 useEffect를 실행시키게 되면 페이지가 렌더링 될 때마다 데이터를 불러온다.  
• 의존성 배열에 빈배열(`[]`)을 담아주게 될 경우에는 첫 렌더링 시에만 함수를 실행한다.  
• 의존성 배열에 `state`나 `props`로 상속받은 데이터 값 등을 담아주게 되면 해당 값이 변할 때마다 콜백 함수가 실행된다.  

**리턴 함수**

• 리턴하는 함수는 컴포넌트가 언마운트 될 때 호출되어, 이벤트를 제거할 수 있다.  
• return 문이 있을 때 [] 의존성배열이 비어있으면 컴포넌트가 제거될 때(unMount) return문이 호출된다.  
• return 문이 있을 때 의존성 배열 안에 변수가 있으면 unMount와 그 변수가 업데이트 직전에 return문이 호출된다.

```react
/* 
useEffect(콜백함수, 의존성배열)
useEffect(() => {
    실행할 코드;
    return () => {
    	cleanup 함수
  	};
}, [감시할 대상])
*/
```

#### 3. useContext

useContext는 프로젝트 내에서 **전역적으로 사용되는 데이터를 여러 컴포넌트에서 사용**할 수 있도록 도와주는 기능을 제공하는 훅이다.

부모 컴포넌트에서 자식 컴포넌트로 props를 넘기는 과정에서 props drilling에 빠지지 않는다.  
또한, 함수형 컴포넌트에서 Context를 보다 쉽게 사용할 수 있다.

```react
const TestContext = createContext();

const App = () =>{
  return(
    <div className="App">
      <TestContext.Provider value='context!!'>
        <UseContextExample/>
      </TestContext.Provider>
    </div>
  )
}

const UseContextExample = () => {
  const context = useContext(TestContext);
  return (
    <div>
      {context}
    </div>
  )
}
```

#### 4. useReducer

useReducer는 컴포넌트의 **상태값을 리덕스의 리듀서 처럼 관리** 하는 훅이다.  

Reducer는 현재 상태와 업데이트를 위해 필요한 정보를 담은 action 값을 받아 새로운 상태를 반환해주는 함수이다.  
useReducer는 데이터의 흐름이 리덕스와 같은 방식으로 state를 직접 업데이트 하지 않고 dispatch action 을 통해 관리한다.

#### 5. useMemo

useMemo는 기존에 수행한 연산의 결과 값을 어딘가에 저장해주고 동일한 입력이 들어오면 **기존 값을 재사용**하는 훅이다.  
이전 값을 기억해 **성능을 최적화하는 용도**로 사용한다.

기존에 수행한 연산의 결과 값을 어딘가에 저장해주고 특정 값이 바뀌었을 때만 연산을 실행하며, 만약 특정 값이 바뀐 것이 아니라면 이전에 연산했던 결과를 다시 사용하는 방식이다.

useState 훅을 사용하면 state가 수정될 때마다 setState 함수가 반복적으로 호출되며, 렌더링 될 때마다 함수 내에서 계산이 진행되는데, useMemo Hook을 사용하면 이러한 작업을 최적화 시킬 수 있다. 

```react
// useMemo(콜백함수, 의존성배열)
const value = useMemo(() => { 
    return calculate(); 
}. [item]);
```

> 🔍 **React.memo**
>
> React.memo는 Higher-Order Components(HOC)이다. 
> (HOC : 컴포넌트를 인자로 받아서 새로운 컴포넌트를 return해주는 구조의 함수)
>
> React.memo는  **props의 변경 여부**를 체크하여 **렌더링**한다.
> React.memo는 이전과 같은 props가 들어올때는 렌더링 과정을 스킵하고 가장 최근에 렌더링된 결과를 재사용 한다.
>
> 하지만 컴포넌트 내부에서 useState같은 훅을 사용 하고 있는 경우에는 상태가 변경 되면 리렌더링 된다.  
>
> 주로 '같은 props로 렌더링이 자주일어나는 컴포넌트','렌더링에 리소스 소모가 큰 컴포넌트'에 사용된다.

#### 6. useCallback

useCallback은 함수가 바뀔경우에만 리렌더링이 되어, 특정 함수를 새로 만들지 않고 **함수를 재사용**하고 싶을 때 사용한다.  
이전 함수를 기억해 **성능을 최적화하는 용도**로 사용한다.

이벤트 핸들러 함수를 필요할 때만 생성할 수 있는 기능을 제공한다.

```react
// useCallback(콜백함수, 의존성배열)
const App =()=>{
  const [name,setName] = useState('');
  
  const change = useCallback((e) => {
    setName(e.target.value);
  },[]);

  return(
    <div className="App">
      <input type="text" onChange={change}/>
      {name}
    </div>
  )
}
```

#### 7. useRef

useRef는 **특정 DOM에 접근**하여 DOM의 조작을가능하게 하는 훅이다.

useRef를 사용하여 ref를 설정하면 useRef를 통해 만든 객체 안의 current 값이 실제 엘리먼트를 가르키게 된다. 

• 접근하고 싶은 태그에 속성으로 ref로 useRef를 연결시켜줘야 한다.  
•  useRef는 .current 프로퍼티로 전달된 인자로 초기화된 변경 가능한 ref 객체를 반환한다.  
• .current 프로퍼티를 변경하더라도 render를 발생시키지 않고, render 되더라도 값이 유지되는 특징을 가진다.

단, React 개념 또는 특징 상 특별한 경우를 제외하고는 DOM을 직접 접근하여 사용하는 것은 올바른 사용법이 아니라고 한다.

```react
function App() {
  const inputRef = useRef(null);

  const onClickInputFocus = () => {
    inputRef.current.focus();
  };

  return (
    <div className="App">
      <input type="text" ref={inputRef} />
      <button onClick={onClickInputFocus}>input 포커스 하기</button>
    </div>
  );
}
```

#### 8. useLayoutEffect

useLayoutEffect는 useEffect와 동일하지만, 렌더링 후 **layout 배치와 paint 전에** **비동기적**으로 실행되는 훅이다.

때문에 DOM을 조작하는 코드가 존재하더라도, 사용자는 깜빡임을 보지 않는다.

```react
/* 
useLayoutEffect(콜백함수, 의존성배열)
useLayoutEffect(() => {
    실행할 코드;
    return () => {
    	cleanup 함수
  	};
}, [감시할 대상])
*/
```

#### 9. Custom Hook

Custom Hook은 위의 useState와 useEffect들과 같이, 특정 상태관리나 라이프사이클 로직들을 추상화하여 묶어서 재사용이 가능하도록 제작이 가능한 함수를 뜻한다.

즉, 특정 상태와 관련된 로직을 useState으로 정의하고, 이 state을 변경시킬 함수들을 객체로 담아 리턴하여 캡슐화한다

<br>

> 💡**useEffect, useLayoutEffect 차이**  
>
> useEffect의 이펙트는 DOM이 화면에 그려진 이후에 호출된다.  
> useLayoutEffect의 이펙트는 DOM이 화면에 그려지기 전에 호출된다.
>
> 따라서 렌더링할 상태가 이펙트 내에서 초기화되어야 할 경우, 사용자 경험을 위해 useLayoutEffect를 활용을 하는게 좋다.

> ##### 💡 useMemo, useCallback, React.memo 차이
>
> useMemo, useCallback, React.memo은 모두 **메모이제이션**과 관련있어 불필요한 렌더링 또는 연산을 제어하여 **애플리케이션 성능을 최적화**를 위해 많이 사용한다.
>
> 하지만 세 가지에는 분명한 차이점이 있다.
>
> - React.memo는 HOC이고, useMemo와 useCallback은 hook이다.
> - React.memo는 HOC이기 때문에 클래스형 컴포넌트, 함수형 컴포넌트 모두 사용 가능하지만, useMemo는 hook이기 때문에 함수형 컴포넌트 안에서만 사용 가능하다.
> - useMemo는 메모이제이션된 값을 반환하여 값을 재사용하는 목적이고, useCallback은  메모이제이션된 함수를 반환하여 함수가 재생성 되는것을 방지하기 위한 목적이다.

<br>

---

[함수형 컴포넌트와 Hook](https://naon.me/posts/til118)

[[TIL] React Hooks 종류](https://velog.io/@joohyeson/React-Hooks-%EC%A2%85%EB%A5%98)

[[React] 리액트 훅(React Hooks)](https://tadaktadak-it.tistory.com/159)

[[React] 리액트 훅에 대해 알아보기(React Hooks란?)](https://choijying21.tistory.com/60#1)

[React Hooks의 종류와 사용법](https://klmhyeonwooo.tistory.com/67)

[[React] React Hook 종류](https://lss3070.github.io/2021/06/25/React-React-Hook/)
