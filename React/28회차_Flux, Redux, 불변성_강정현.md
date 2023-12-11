## Flux란
MVC 패턴의 양방향 전달로 데이터의 흐름이 예측 불가능해지고, 이로 인한 수많은 버그가 발생됨을 해결하기 위한 아키텍처 패턴.

Flux 패턴은 사용자 입력을 기반으로 액션을 생성하고, 이를 디스패쳐에 전달하여 스토어의 데이터를 반경한 뒤 뷰에 반영하는 단방향 데이터 흐름을 가진다. 
데이터가 단방향으로만 전달되기 때문에 데이터의 흐름을 파악하기 용이하고, 그 결과를 쉽게 예측할 수 있다.

## Flux 구성 요소
### 액션 생성자 (Action Creator)
액션이란 어떤 행위인지와, 그 행위로부터 넘겨받은 값들을 가진 하나의 객체를 말한다.
액션 생성자는 **기존 상태를 변경하기 위한 액션의 생성**을 담당하며, 해당 액션을 생성해서 디스패쳐에 넘긴다.
- `type`: 어떤 행위인지 가리킴. 시스템에 정의된 액션들 중 하나.
- `payload` : 행위로부터 넘겨받은 값.

### 디스패쳐 (Dispatcher)
디스패쳐는 모든 액션들을 받아서 의존성을 처리한 후 모든 스토어에게 넘긴다.
액션 타입과는 관계 없이, 등록된 모든 스토어로 보낸다. - 스토어가 특정 액션만 구독하지 않고 모든 액션을 일단 받은 뒤, 처리할 지 말지를 결정한다.

### 스토어 (Store)
스토어는 모든 액션을 받아서, 자신에게 적합한 액션이 어떤 것인지 필터링한다. (주로 switch statement)
이후 상태값을 변경하고, 변경 이벤트(change event)를 내보내어 자신에게 연결된 컨트롤러 뷰에게 상태가 변화되었음을 알린다.

### 컨트롤러 뷰 (Controller View)와 뷰 (View)
View: 상태를 가져오고, 유저에게 보여주고 입력 받을 화면을 렌더링함.
Controller View: 전체적으로 화면에 나타나는 자식 View들과 스토어를 연결하는 매개체.

상태가 변경되었을 때 스토어가 컨트롤러 뷰에게 전달하면, 컨트롤러 뷰는 자식 뷰에게 새로운 상태를 넘겨준다.
자식 뷰들은 컨트롤러 뷰에게 변화된 상태를 받고, 그 상태에 따라 다시 렌더링이 된다.

![FLUX 흐름도](https://baeharam.netlify.app/media/architecture/flux.png)

--- 

<br/>

## Redux란
Flux를 개선한 아키텍쳐로, 자바스크립트 어플리케이션 상태 관리 라이브러리.
구성 요소는 Flux와 비슷하지만, 리듀서(Reducer)를 추가하여 상태 변경의 개념이 다르다. 

Redux는 state 관리 로직을 컴포넌트와 분리시킴으로써 효율적으로 관리할 수 있게 되며,
컴포넌트끼리 state를 공유할 때 여러 컴포넌트를 거쳐 props를 복잡하게 전달(Props Drilling)하지 않아도 쉽게 값을 전달할 수 있게 된다.

## Redux 구성 요소

### 액션 생성자 (Action Creator)
상태에 어떠한 변화가 필요하게 되면, 액션을 발생시킨다. 액션은 객체 형태로 이루어지며, type 필드를 필수적으로 갖는다. 그 외 속성값은 옵션으로 추가할 수 있다.
```javascript
{
	type: "TOGGLE_VALUE",
	data: {
		id: 0,
		text: "get toggle value"
	}
}
```

액션 생성자는 사용자가 요청한 액션을 만들어서 포맷에 맞게 돌려준다.
```javascript
export function addTodo (data) {
	return {
		type: "ADD",
		data
	};
}

// 화살표 함수로도 구현할 수 있다.
export const changeInput = text => ({
	type: "CHANGE_INPUT",
	text
})

```

발생된 액션을 Dispatch 함수를 통해 Reducer에 전달한다.

### 스토어 (Store)
전역 상태 저장소.
Flux에선 다수의 스토어를 가질 수 있지만, Redux는 단 하나의 스토어를 가진다.
내장 함수를 제공하며, 이러한 내장 함수들을 사용하여 스토어의 데이터에 접근하고, 변경을 요청할 수 있다.
상태 트리(State tree)라고 불리는 상태값을 유지한다.

### 리듀서 (Reducer)
리듀서가 **상태 변화 로직을 담당**하며, 스토어를 변경하기 위한 로직을 저장하는 곳.
두 가지의 파라미터(state, action)을 받아, 현재의 상태(state)와 전달 받은 액션(action)을 참고하여 새로운 상태를 만들어 반환한다.

리듀서는 전체 리듀서를 관리하는 루트 리듀서와 하위의 서브 리듀서로 나뉜다.
1. 루트 리듀서는 상태 객체의 키(key)를 기준으로 조각조각 나누어서 서브 리듀서에게 보낸다. 
2. 서브 리듀서는 이전 상태를 변경하지 않고 복사해서 변경한다. 이렇게 상태 객체를 업데이트 시키고, 이를 루트 리듀서에게 돌려준다.
3. 서브 리듀서의 작업이 끝나면, 루트 리듀서가 받은 상태 객체들을 취합해서 스토어로 보낸다.

```JSX
const count = 1;

const counterReducer = (state = count, action) => {
	switch (action.type) {
		case 'INCREASE': 
			return state + 1;
		case 'DECREASE':
			return state - 1;
		case 'SET_NUMBER':
			return action.payload
		default:
			return state;
	}
}
```

### Dispatch 함수
액션 객체를 리듀서로 전달하는 함수.
```javascript

// 액션 객체를 직접 작성하는 경우.
dispatch( { type: 'INCREASE' } );
dispatch( { type: 'SET_NUMBER', payload: 5 } );

// 액션 생성자(Action Creator)를 사용하는 경우.
dispatch( increase() );
dispatch( setNumber(5) );
```

### 뷰 레이어 바인딩 (View-Layer binding)
스토어와 뷰를 연결하기 위한 것으로, 뷰에서 스토어의 상태 값들에 접근하기 위해 필요하다.
공급 컴포넌트(Provider Component)를 통해서 스토어가 뷰에 상태를 공급하며, 뷰에선 특정 함수 (ex. connect)를 통해서 스토어에 연결된다.

![Redux 흐름도](https://baeharam.netlify.app/media/architecture/redux.png)

<br/>

## Redux 상태 관리 순서
1. 상태가 변경되어야 하는 이벤트가 발생하면, 변경될 상태에 대한 정보가 담긴 Action 객체를 생성한다.
	- 액션 객체는 직접 생성할 수도, 액션 생성자를 호출하여 액션을 생성할 수도 있다.
2. Action 객체는 Dispatch 함수의 단일인자로 전달된다.
3. Dispatch 함수는 Action 객체를 Reducer 함수로 전달한다.
4. Reducer 함수는 Action 객체의 값을 확인해서, 전역 상태 저장소 Store의 상태를 변경한다.
5. 상태가 변경되면, React가 리렌더링하여 UI를 업데이트한다.

## Redux의 세 가지 원칙
### 1. Single source of truth
하나의 애플리케이션 안에는 하나의 Store만 사용해야 한다.
이렇게 함으로써 서버로부터 가져온 state가 직렬화(serialization)된 채 전달될 수 있으며, 클라이언트 측에서는 이를 추가적인 코드 없이도 곧바로 사용할 수 있다.

### 2. State is read-only
Redux의 상태(state)는 읽기 전용으로, 직접 변경할 수 없고 액션 객체가 있어야만 상태를 변경할 수 있다.
이를 통해 모든 state의 변화를 중앙에서 관리할 수 있으며, state 변경에 대한 추적이 용이해진다.

### 3. Changes are made with pure functions
state의 변화를 일으키는 Reducer는 순수 함수로만 이루어진다.
Reducer는 현재 state와 Action을 전달 받아 다음 state를 반환하는 순수 함수로, 이전 state를 변경하는 것이 아니라, 새로운 state 객체를 생성하여 반환한다.

<br/>

## 불변성
### 원시 타입의 불변성

```javascript
let string = 'data1';
string = 'data2';
```

`string` 변수는 `data1` -> `data2` 로 값이 변경된 것처럼 보이나,
실제 메모리 영역에는 `data1`, `data2` 둘 다 존재한다.

메모리 영역이 1 ~ 10 영역까지 10개가 있다고 가정할 때,

```javascript
let string = 'data1'; // 1. string: 'data1'이 메모리 영역 1에 저장됨.
string = 'data2'; // 2. string: 'data2'가 메모리 영역 2에 저장됨.
```

`string`은 `data1`이었고, 여기에 `data2`를 재할당하는데
기존 메모리 영역 1에 있는 `data1`의 값은 그대로 두고, 메모리 영역 2에 `data2`를 새로 할당한다.
즉, **메모리 영역에서 `data2`는 `data1`을 대체하는 것이 아니라 새로운 영역에 할당**되고, 이를 불변성이라고 한다.

### 참조 타입의 불변성
```javascript
let array = [1, 2, 3, 4]; // 메모리영역 1
array.push(5); // 메모리영역 1

array = [1, 2, 3, 4]; // 메모리영역 2 (새로운 참조값)
```

`array.push(5)` 는 원본 배열을 수정하면서 불변성을 지키지 않고 있고,
`array = [1, 2, 3, 4]` 는 원본 배열을 수정하는 것이 아니라 새 참조값을 가진 새로운 배열을 할당하여 불변성을 지킴.

## React에서 불변성을 지키는 이유

### 1. 효율적인 상태 업데이트 (얕은 비교 수행)
리액트는 상태값을 업데이트 할 때 얕은 비교를 수행한다.
즉 배열이나 객체의 속성 하나하나를 비교하는 것이 아니라, **이전 참조값과 현재 참조값만을 비교하여** 상태 변화를 감지한다. 
이러한 이유로 배열이나 객체를 업데이트할 때 새로운 참조값을 가진 배열이나 객체를 생성한다.
불변성을 지킴으로써, 리액트는 상태 변화를 감지할 수 있다.

### 2. 사이트 이펙트 방지
외부에 존재하는 원본 데이터를 직접 수정하지 않고, 원본 데이터의 복사본을 만들어서 값을 사용하기에 예상치 못한 오류를 사전에 발생할 수 있다.

## Redux(Reducer)의 불변성
새로운 객체가 아니라 **기존의 state 값을 참조하여 state를 반환한다면, redux(reducer)에서는 기존의 state와의 차이를 인식할 수 없기 때문에** 변경된 state가 적용되지 않고 기존의 state를 유지하게 된다.

따라서 새로운 state값을 반환하는 경우, 새 객체로 갱신하여 반환한다.
```javascript
const initialState = {
	name: 'Kim',
	job: 'student',
	age: null,
}

const reducer = (state = initialState, action) => {
	switch(action.type) {
		case 'UPDATE_AGE':
			return {
				...state,
				age: action.data
			};
		default:
			return state;
	}
}
```

<br/>

---

**Reference**

https://baeharam.netlify.app/posts/architecture/flux-redux

https://2ham-s.tistory.com/336

https://summerr.tistory.com/56#%EB%A6%AC%EB%8D%95%EC%8A%A4%EB%9E%80%3F-1

https://www.tcpschool.com/react/react_redux_intro

https://hsp0418.tistory.com/171
