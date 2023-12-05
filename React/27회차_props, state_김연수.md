[TOC]

## props와 state

React에서 컴포넌트가 데이터를 다루는 방법으로 `props`, `state` 그리고 `context`가 있다.

- **props** : 부모 컴포넌트에서 자식 컴포넌트로 전달되는 데이터, 자식 컴포넌트에서는 변경할 수 없다.
- **state** : 컴포넌트 **내부**에서 관리하는 데이터, 컴포넌트 안에서 데이터를 변경할 수 있다.   
  			 즉, State는 한 컴포넌트의 상태(State)를 나타낸다.

### Props

React 에서 한 컴포넌트에서 다른 컴포넌트로 데이터를 전송할 때, 사용하는 특수 객체가 props 이다.

Props는 **단방향**으로 데이터를 전송한다는 특징이 있다.  
자식에서 부모로, 동일한 레벨의 Component로 Props를 전달하는 것은 불가능하다.

#### props 사용 방법

props 로 데이터를 전달하는 방법은 우선 컴포넌트에 props를 정의하고 값을 할당해야 한다.

Props 에 숫자, Boolean, 배열 등 다양한 형태의 데이터를 전달할 수 있는데, 문자를 제외하고는 `{}`로 감싸서 데이터를 전달해야 한다.

```react
function App() {
  return (
    <div>
     	<Dog name="Ari" age={10} />
		<Dog name="Bori" age={7} />
    </div>
  );
}
```

컴포넌트 내부에서 사용 시, 객체에 존재하는 값을 가져오듯 점 연산자(.)를 사용하여 원하는 props를 꺼내 쓸 수 있고, 이를 중괄호로 감싸 `{ [인자 이름].[props 이름] }` 형태로 사용한다.

```react
// 함수 컴포넌트에서 props 사용
function Dog(props) {
  return <h1>{props.name}, {props.age}</h1>;
}

// 클래스 컴포넌트에서 props 사용
class Dog extends React.Component {
  render() {
    return (
      <h1>{this.props.name}, {this.props.age}</h1>;
    )
  }
}
```

React 는 `{name: 'Ari', age: '10'}` 를 Props로 하여 Dog Component를 호출하고, Dog Component 는 `<h1>Ari, 10</h1>` 컴포넌트를 반환한다.

props를 받을 때, **구조 분해 할당 **을 사용하여 아래와 같이 점 연산자 사용을 줄일 수 있다.

```react
// 함수형 컴포넌트에서 객체 인자를 구조 분해 할당
function Dog({ name, age }) {
	return {
		<div>{name}, {age}</div>
	}
}

// 클래스 컴포넌트에서 구조 분해 할당으로 props 사용
class Dog extends React.Component {
	render() {
		const { name, age } = this.props;	
		return <div>{name}</div>;
    }
}
```

<br>

#### props는 읽기 전용 객체

Props의 중요한 특징 중 하나는 `읽기 전용의 객체`이다.

props는 **읽기 전용**이므로 **컴포넌트의 내부에서 props를 수정해서는 안 된다.** 입력 값을 수정하지 않는 함수를 순수 함수라고 호칭하며, 모든 React 컴포넌트는 자신의 props를 다룰 때, 순수 함수처럼 동작해야 한다.

그러나 UI 는 동적이며, 사용자 액션, 네트워크 응답 및 다른 요소에 의해 출력이 변화될 수 있다. 그리하여 React는 `state` 를 통해 위의 규칙을 위반하지 않고 동적으로 시간에 따라 UI를 변화시킨다.

#### defaultProps, propTypes

defaultProps 프로퍼티를 할당하여 **props 의 초기값 **을 정의할 수 있으며, `prop-types` 를 통하여 **props의 타입을 확인** 할 수 있다.

클래스형 컴포넌트에서 propTypes나 defaultProps를 사용할 때는, 클래스 내부에서도 지정할 수 있다.

```react
// 컴포넌트 props 초기값 지정
Dog.defaultProps = {,
	name: '이름',
    age: 0,
}
// 컴포넌트 props 타입 확인
Dog.propsTypes = {,
	name: PropTypes.string.isRequired,
    age: PropTypes.number,
}

// 클래스형 컴포넌트에서 props 사용하기
class Dog extends React.Component {
	static defaultProps = { ... };	// 컴포넌트 props 초기값 지정
	static propTypes = { ... };	// 컴포넌트 props 타입 확인
	render() { ... }
}
```

<br>

### State

state 는 컴포넌트 **내부**에서 관리하며, 상태에 따라 변하는 **동적 데이터** 이다.  
state 는 props와 다르게 자동으로 생성되지 않아 명시적으로 state를 기술 해야 한다.

state를 사용하는 방식에는 컴포넌트의 종류에 따라 2가지가 있다. 

• 클래스형 컴포넌트 : 컴포넌트 자체가 state를 지니는 방식으로 사용   
• 함수형 컴포넌트 : useState라는 함수, Hook을 통해 사용

#### 클래스형 컴포넌트에서 State 사용

```react
// 1.
class Greeting extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      name: 'min'
    };
  }
}

// 2.
class Greeting extends React.Component {
  state = {
    name: 'min'
  }
}
```

#### 함수형 컴포넌트에서 State 사용

함수형 Component 에서는 Hook의 `useState` 을 사용하여 state 를 관리할 수 있다.   
`useState` 는 상태 유지 값과 그 값을 갱신하는 함수를 반환한다.

```react
function Greeting() {
  // useState 의 첫 번째 값은 initialState, 두 번째는 Setter 함수
  const [name] = useState('min');
}
```

##### setState 함수

state 값을 변경할 때는 setState 함수를 사용한다. 그리고 setState 를 사용하여 **state 가 변경되면 컴포넌트는 리렌더링** 된다. 

setState 는 **비동기적**으로 이루어진다. 그러므로, setState 호출 직후 새로운 값이 바로 this.state 에 반영되지 않는다. 

이때, callback 을 사용하여 업데이트 된 state를 사용할 수 있다.

<br>

---

**참고**

[[React] Component와 props, state](https://velog.io/@soyi47/React-Component-props-state)

[[React] React 의 Props, State](https://minjung-jeon.github.io/React-props-state/)

[[React] Props와 State - React에서 데이터를 다루는 주요 개념인 ...](https://deku.posstree.com/ko/react/props-state/)
