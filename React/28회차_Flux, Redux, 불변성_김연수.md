[TOC]

> 💡 **한 줄 개념 요약**
>
> **Flux** : MVC패턴에 양방향 데이터 흐름의 문제를 해결하기 위해 단방향 데이터 흐름을 따르는 아키텍쳐 패턴  
> **Redux** : Flux 아키텍쳐로 구현한 라이브러리, Flux와 비슷하지만 리듀서(Reducer)를 추가해서 상태변경

# Flux

**Flux 패턴**은 기존의 MVC 패턴의 양방향 데이터 흐름 때문에 복잡성이 생겼으므로 이를 단방향 데이터 흐름으로 만들어낸 아키텍쳐이다.

Flux 패턴으로 구현된 프로젝트는 데이터가 단방향으로만 전달되기 때문에 데이터의 흐름을 파악하기가 용이하고, 그 결과를 쉽게 예측할 수 있다는 장점이 있다.

Flux 패턴은 사용자 입력을 기반으로 Action을 생성하고, 이를 Dispatcher에 전달하여 Store의 데이터를 변경한 뒤 View에 반영하는 **단방향의 데이터 흐름**을 가지는 소프트웨어 아키텍처이다.

<details>
  <summary>
     [ 🔍 MVC 패턴 ]
  </summary>



> MVC 패턴이란 Model에 데이터를 저장하고, Controller를 사용하여 Model의 데이터를 관리하는 소프트웨어 아키텍처 패턴 중 하나이다. MVC 패턴은 사용자가 View를 통해 데이터를 입력하면 View에서도 Model를 업데이트 할 수 있기 때문에 데이터가 양방향으로 전달된다.
>
> 하지만 이렇게 구현된 프로젝트는 프로젝트의 규모가 커질수록 수많은 Model과 View가 생성되고, 이때 Model의 상태에 변화가 생길 경우 Model과 View 사이에 엄청난 양의 데이터가 양방향으로 전달되게 된다. 이로 인해 데이터의 흐름을 예측하기가 점점 힘들어지고, 수많은 버그를 발생 시키는 원인이 된다.
>
> <div align="center"><img src="https://www.tcpschool.com/lectures/img_react_mvc-pattern.JPG" width="50%" alt="MVC패턴구조" /><div><small>[ MVC 패턴 ]</small></div</div>

</details>

<br>

### 구성 요소

- **액션(Action)**

  - 어떤 행위인지( `type` )와 그 행위로부터 넘겨받은 값( `payload` )들을 가진 하나의 객체를 말한다. 

  - 액션은 기존 상태를 변경하기 위한 액션의 생성을 담당하며 해당 액션을 생성해서 디스패쳐에 넘겨준다.

- **디스패처(Dispatcher)**

  - 모든 액션들을 받아서 의존성을 적절히 처리해준 다음 모든 스토어에게 넘긴다. 이때, 모든 스토어가 모든 액션을 받는다.

- **스토어(Store)**

  - 모든 액션을 받아서 자신에게 적합한 액션이 어떤 건지 필터링한다. 
  - 이후 상태값을 변경하고 자신에게 연결된 컨트롤러 뷰에게 상태가 변화되었음을 알린다.

- **뷰(View)**

  - 뷰는 변화된 상태를 받고 그 상태에 따라 다시 렌더링이 된다.

<br>

### 작동 흐름

Flux패턴은 **View**에서 **Action**을 발생시켜 **Dispatcher**로 전달하면, **Dispatcher**는 등록된 모든 **Store**에게 **Action**을 전달한다. **Store**에서는 전달받은 **Action**을 처리하여 내부 상태를 업데이트하고, **View**에게 변경된 상태를 전달한다. 

<div align="center">
  <img src="https://www.tcpschool.com/lectures/img_react_flux-pattern.JPG" width="50%" alt="Flux패턴구조" />
  <div>
    <small>[ Redux 패턴 ]</small>
  </div>
</div>



<br>

# Redux

프로젝트의 규모와 복잡도가 증가할수록 컴포넌트끼리 state를 공유해야 할 때, `Props Drilling`이 발생할 수 있다. 따라서 state를 보다 효과적으로 관리할 수 있는 방법의 중요성이 대두되었으며, 이 때 등장한 여러 상태 관리 라이브러리 중 하나가 바로 Redux이다.

> 🔍 **Props Drilling**
>
> props를 오로지 하위 컴포넌트로 전달하는 용도로만 쓰이는 컴포넌트들을 거치면서 React Component 트리의 한 부분에서 다른 부분으로 데이터를 전달하는 과정이다.  
> 컴포넌트끼리 state를 공유할 때 여러 컴포넌트를 거쳐 prop를 복잡하게 전달되는 경우를 `Props Drilling` 이라고 말한다.



Redux는 **Flux패턴을 발전**시킨 것으로 Dan Abramov이라는 개발자가 **Flux 패턴에 리듀서(Reducer)를 결합하여 만든 라이브러리**이다. 

Store(스토어)라는 변수를 이용하여 전역 상태 관리를 하기 때문에 props를 통해 부모 컴포넌트에서 자식 컴포넌트로, 자식의 자식 컴포넌트로 내려주지 않아도 사용할 수 있다.

Redux를 사용하면 state 관리 로직을 컴포넌트와 분리시킴으로써 효율적으로 관리할 수 있게 되며, 컴포넌트끼리 state를 공유할 때 여러 컴포넌트를 거쳐 prop를 복잡하게 전달(Props Drilling)하지 않아도 손쉽게 값을 전달할 수 있다.

<br>

### 구성 요소

- **액션(Action)** : 액션은 state를 변화시키려는 의도를 표현하는 객체.

- **리듀서(Reducer)**

  - Store를 변경하기 위한 로직을 저장하는 곳, 디스패처가 없는 대신에 리듀서가 상태 변화 로직을 담당한다. 
  - state와 Action을 인수로 전달 받아 Store에 접근하고, 전달 받은 Action을 참고하여 새로운 state를 만들어 반환한다.
  - 리듀서가 변경하는 state은 직접 변경된 state가 아닌 새로 생성되어 반환된 새로운 state이다.   
    즉, reducer는 **순수 함수**임을 지켜야 한다.

- **스토어(Store)**

  - Redux앱의 state가 관리되는 오직 하나 뿐인 저장소이다.

  - Flux에선 다수의 Store를 가지는 것과 다르게 Redux는 단 하나의 스토어를 가진다. 대신에 **상태 트리(State tree)** 로 상태 값을 유지한다.

    > 🔍 **상태 트리** : 상태 트리는 문제 해결의 중간 상태를 각각 한 노드로 나타낸 트리이다.  
    > Redux에서는 Redux API에서 저장소에 의해 관리되고 `getState()`에 의해 반환되는 하나의 상태 값만 가진 것을 의미한다.

- **디스패치 함수(Dispatch)**

  - Action을 발생 시키는 함수

  - 액션을 파라미터로 전달하여 호출한 뒤, 스토어가 리듀서 함수를 실행시켜 해당 액션을 처리하게 된다. 

    ```react
    <button onClick={()=>{ props.dispatch({ type: 'INCREASE'}) }}>추가</button>
    ```

- **구독(Subscribe)**

  -  Store의 내장 함수이며, 함수 형태의 값을 파라미터로 받아와  Action이 Dispatch 될 때마다 받은 함수가 호출되게 된다.

<br>

### 작동 흐름

1. 사용자가 UI를 통해 컴포넌트 내에 존재하는 이벤트를 호출한다.  
2. 이벤트와 연결된 액션 생성 함수(Action Creator)가 호출한다.  
3. 액션 생성 함수에서 생성된 Action이 호출한다.  
4. 호출된 Action이 Reducer로 전달(Dispatch)한다.  
5. Reducer에서 Dispatch 된 Action에 따라 state 값을 변경합니다.  
6. 변경된 state가 렌더링되어 UI에 표시됩니다.

<div align="center">
  <img src="https://www.tcpschool.com/lectures/img_react_redux-operating-principle.JPG" width="50%" alt="Redux패턴구조" />
  <div>
    <small>[ Redux 패턴 ]</small>
  </div>
</div>



<br>

### Redux의 3가지 원칙

**1. Single source of truth : 하나의 어플리케이션은 하나의 store만 가진다.**
Redux에는 데이터를 저장하는 Store라는 단 하나뿐인 공간 관리된다. 이렇게 하면 디버깅이 쉬워지고, 서버로부터 가져온 state가 직렬화가 된 채 전달될 수 있으며, 클라이언트에서는 이를 추가적인 코드 없이도 곧바로 사용할 수 있다.

**2. State is read-only : 상태는 읽기 전용이다.**
Redux의 상태는 읽기 전용으로 직접 변경할 수 없고 Action객체가 있어야만 상태를 변경할 수 있다. 이를 통해 모든 state 변화를 중앙에서 관리할 수 있으며, state 변경에 대한 추적이 용이해 진다.

**3. Changes are made with pure functions : 리듀서는 순수 함수여야 한다.**
Reducer는 현재 state와 Action을 전달 받아 다음 state를 반환하는 순수 함수입니다. 즉, 이전 state를 변경하는 것이 아니라 새로운 state 객체를 생성하여 반환 해야 합니다.

<br>

### Redux 장단점

**장점**

• 크고 복잡한 앱에서 **확장성이 높음 **  
• 액션에 따른 **모든 변경을 추적 가능**  
• 특정 상태 조각이 언제 변경되었으며 데이터는 어디에서 왔는지 **동작을 예측 가능**

**단점**

• 코드를 작성하는 가장 짧거나 빠른 방법은 아님  
• 배워야 할 개념과 작성해야 할 코드가 많음

<br>

### Redux의 필요성

- **필요한 경우**

  • 앱의 여러 위치에서 필요한 상태의 양이 많을 때  
  • 시간이 지남에 따라 상태가 자주 업데이트될 때  
  • 큰 규모의 코드베이스를 가지고 많은 사람들이 작업할 때  
  • 시간이 지남에 따라 상태가 어떻게 업데이트되는지 확인해야 할 때

<br>

전역 상태 관리는 Redux 뿐만 아니라 Mobex, Recoil, Zotai 등 다른 상태 관리 라이브러리를 대신 사용할 수도 있으며, React의 Context API를 통해서도 충분히 동일한 작업을 수행할 수 있다. 

Redux는 공유 상태 관리를 매우 효과적으로 수행할 수 있지만 다른 도구와 마찬가지로 장단점이 있다.

따라서 상태 관리를 위해서는 반드시 Redux를 사용해야 한다는 것이 아니라 해당 프로젝트의 특징과 확장성 등을 충분히 고려하여 Redux의 도입 여부를 결정해야 한다.

<br>

# 리액트 불변성

> **불변성** : 불변성이란 값이나 상태를 변경할 수 없는 것
>
> 불변성을 지킨다의 의미는 **메모리 영역에서 값을 변경할 수 없게 한다**, 라는 의미이다.

### 불변성을 지켜야 하는 이유

리액트에서 불변성을 지켜야하는 이유는 리액트가 상태 업데이트를 하는 원리 때문이다.

**리액트의 state 변화 감지 기준은 콜 스택의 주소 값** 이다.  
리액트는 콜 스택의 주소값만을 비교해 상태 변화를 감지하는데, 이를 **얕은 비교**라고 한다.  
즉, 리액트는 배열이나 객체의 속성 하나하나를 비교하는게 아니라 이전 참조값과 현재 참조값만을 비교하여 상태 변화를 감지한다.

⭐ **참조 타입으로 값만 변경하면** 실제로 콜스택의 주소값은 변경이 없어 state **감지가 되지 않아 리렌더링이 되지가 않는다.**

이것이 리액트의 state를 빠르게 감지할 수 있는 장점이자 불변성을 유지해야 하는 이유이다.

#### 1. 효율적인 상태 업데이트 (얕은 비교 수행)

얕은 비교는 계산 리소스를 줄여주기 때문에 리액트는 효율적으로 상태를 업데이트 할 수 있다.

#### 2. Side Effect 방지 및 프로그래밍 구조의 단순성

> Side Effect(부수 효과)  : 원래의 목적과 다른 효과 또는 부작용

리액트에 state는 참조 타입이다. 참조 타입은 불변성을 지키지 않기 때문에 객체나 배열의 값을 변경할 때 원본 데이터가 변경될 수 있다. 원본 데이터가 변경될 경우, 원본 데이터를 참조하고 있는 다른 객체에서 예상치 못한 오류가 발생할 수 있으며, 프로그래밍의 복잡도도 올라간다. 따라서 불변성을 지켜주면 사이드 이펙트를 방지하고 프로그래밍의 구조를 단순하게 유지할 수 있다.

> 🔍 **원시 타입과 참조 타입**
>
> 자바스크립트 엔진은 call statck과 heep memory 2가지 메모리 공간을 가지고 있다.
>
> \*콜 스택: 지역변수를 저장하는 공간, 이곳에 원시 타입들이 저장.  
> \*힙 메모리: 참조 타입들이 할당되는 곳.
>
> - **원시 타입**
>   - 변수 a에 값 10을 저장을 했을 경우, 콜 스택의 변수 값에 10이 그대로 저장 한다. 
>   - 변수 값을 변경할 경우, 기존 콜스택의 값을 변경하지 않고 새로운 주소를 추가해 값을 저장하고 저장한 곳을 바라보게 한다. (**불변성 유지**)
> - **참조 타입**
>   - 변수 a,b에 array, object를 저장할 경우, 실제 값은 메모리 힙에 저장되고, 메모리의 힙의 주소가 콜 스택의 값에 저장이 된다. 
>   - 변수 값을 변경할 경우, 실제로는 변수 a,b가 바라보고 있는 콜 스택의 값이 변경되지 않고, 메모리 힙에 있는 데이터가 변경이 되어 **불변성이 유지가 되지 않는다**.

<br>

### 불변성을 지키는 방법

1. 리액트는 state를 업데이트 할 때 `setState`으로 새로운 참조 값을 가진 배열이나 객체를 생성한다.

2. spread operator, map, filter, slice, reduce 등 새로운 객체나 배열을 반환하는 메소드들을 활용하여 값을 넣어 준다.

```react
// 원시타입
const [number, setNumber] = useState(0)
setState(3)

// 참조타입
const [person, setPerson] = useState({ name: '', age: 30 })
setState({...person, name: 'pyo'})
```

<br>

### redux(reducer)에서 불변성을 유지하는 이유

내부적으로 데이터가 변경되는 것을 감지하기 위해 **얕은 비교**(shallow equality) 검사를 하기 때문이다.  
객체의 변화를 감지할 때 객체의 깊숙한 안쪽까지 비교하는 것이 아니라 겉핥기 식으로 비교하여 좋은 성능을 유지할 수 있다.

reducer는 이전 state와 action을 갖고 있고, 새로운 state를 리턴하는 함수이다.  
reducer에서 이전상태를 변경하지 않고, 새로운 객체를 생성하는 이유는 이전 상태와 다른 변화를 Redux가 추적할 수 있게 하기 위해서다.

### 전개 연산자 단점 보완

spread 연산자로 얕은 복사를 이용해서 충분히 불변성을 유지할 수 있지만, 객체의 깊이가 깊어질수록 코드가 길어지고 가독성이 떨어진다.  이를 해결하기 위해 등장한 것이 immer이다.   

immer 라이브러리를 이용해서 스프레드 연산자 없이 불변성을 유지해주고 사이드 이펙트를 막아준다.

#### immer 사용 방법

immer를 사용하기 위해서는 produce(prevState, callback) 함수로 감싸준다.  
callback함수의 인자로는 draft를 넣어 준다. draft는 prevState를 얕은 복사한 값이다.  
draft값을 변경해 불변성을 유지한 채 nextState를 만들 수 있다.

```react
// ...spread 방식
const nextState = {
  ...state,
  posts: state.posts.map((post) =>
    post.id === 1
      ? {
          ...post,
          comments: post.comments.concat({
            id: 3,
            text: "새로운 댓글",
          }),
        }
      : post
  ),
};

// immer 라이브러리
const nextState = produce(state, (draft) => {
  const post = draft.posts.find((post) => post.id === 1);
  post.comments.push({
    id: 3,
    text: "새로운 댓글",
  });
});
```

<br>

---

**참고_[Flux, Redux]**

[[Architecture] Flux와 Redux에 대한 이해](https://baeharam.netlify.app/posts/architecture/flux-redux/)

[[TCPschool] Redux 개요](https://www.tcpschool.com/react/react_redux_intro)

[[Redux] Redux의 데이터 흐름과 Flux패턴 - 꾸준함이 무기](https://summerr.tistory.com/56)

[Redux: 뜻밖의 상태 관리 여정](https://velog.io/@hang_kem_0531/Redux-뜻밖의-상태-관리-여정)

**참고_[불변성]**

[리액트 불변성이란 무엇이고, 왜 지켜야 할까?](https://hsp0418.tistory.com/171)

[[React] 불변성이란? 불변성을 지켜야 하는 이유](https://narup.tistory.com/268)

[[[Redux] reducer에서 immer를 이용한 불변성관리 예시코드.](https://gisastudy.tistory.com/90)](https://gisastudy.tistory.com/90)
