# props

> props란?
> 부모 컴포넌트에서 자식 컴포넌트로 전달하는 데이터.
> 자식 컴포넌트에서 props를 변경할 수 없다.

## props 지정하기
`<ComponentName prop1={propValue1} prop2={propValue2} ... />` 형태로, 컴포넌트를 부를 때 함께 지정한다.

```JSX
<Dog name="Ari" age={10} />
<Dog name="Bori" age={7} />
```

- 같은 컴포넌트에 다른 props 값을 주어, 패턴이 반복되는 형태로 컴포넌트의 재사용이 가능하다.
- props에는 boolean(true/false), 숫자, 배열과 같은 다양한 형태의 데이터를 담을 수 있다.
- props에 있는 데이터는 문자열인 경우를 제외하면 모두 중괄호로 값을 감싼다.

## Prop 받아 사용하기
- props는 읽기 전용이므로, 컴포넌트 내부에서 props를 수정할 수 없다.
	- 입력값을 수정하지 않는 함수를 '순수 함수'라고 호칭하며, 모든 React 컴포넌트는 자신의 props를 다룰 때, 순수 함수처럼 동작해야 한다.
- props로 받는 함수형 컴포넌트에 인자를 정의하면, props를 속성으로 가지는 객체 Object가 해당 인자로 전달된다.
- 컴포넌트 내부에서 사용 시, `{ props.propValue1 }`, `{ props.propsValue2 }` 의 형태로 사용한다.

```JSX
function Dog (props) {
	return (
		<div>{props.name}</div>
		<div>{props.age}</div>
	)
}
```

### 클래스형 컴포넌트에서 props 사용하기
클래스형 컴포넌트에서 props를 사용할 때는 `this.props`로 불러와 사용한다.

```JSX
class Dog extends React.Component {
	render() {
		const { name, age } = this.props;
		return <div>{name}</div>;
	}
}
```
<br/>

# state

> state란?
> 컴포넌트 내부의 동적 데이터.

## 클래스 컴포넌트에서 state 사용
- 객체 형식의 `this.state`를 통해 state 객체의 초기값을 설정하고 조회한다.
- state 값을 변경할 경우, `this.setState`를 사용한다.

```JSX
class ClassExample extends React.Component {
	constructor (props) {
		super(props);
		// state 초기값 설정
		this.state = {
			count: 0,
		};
	}
	render() {
		const { count } = this.state; // state 조회
		return (
			<div>
				<p>You clicked {count} times.</p>
				<button onClick={()=>{
					this.setState({ // this.setState를 통해 state 업데이트
						count: count + 1 
					});
				}}>
					Click Me
				</button>
			</div>
		)
	}
}
```

## setState()의 특징
### 1. 직접 state를 수정할 수 없다.
```JSX
this.state.comment = 'Hello'; // 컴포넌트를 다시 렌더링하지 않음.
```
```JSX
this.setState({comment: 'Hello'}); // setState()를 이용해 수정.
```

###  2. state 업데이트는 비동기적일 수 있다.
`this.props`와 `this.state`가 비동기적으로 업데이트될 수 있기 때문에, 다음 state를 계산할 때 해당 값에 의존해서는 안 된다.

- 잘못된 코드
```JSX
this.setState({
	counter: this.state.counter + this.props.increment,
});
```

객체보다는, 함수를 인자를 사용하는 다른 형태의 `setState()`를 사용한다.
- 이전 state 를 첫 번째 인자로 받고, 업데이트가 적용된 시점의 props를 두 번째 인자로 받는다.

- 사용 코드
```JSX
this.setState((state, props) => ({
	counter: state.counter + props.increment
}));
```

### 3. state 업데이트는 병합된다.
- `setState()`를 호출할 때, React는 제공한 객체를 현재 state로 병합한다.
- state는 다양한 독립적인 변수를 포함할 수 있으며, 별도의 `setState()` 호출로 각 변수를 독립적으로 업데이트할 수 있다.
```JSX
constructor(props) {
	super(props);
	this.state = {
		posts: [],
		comments: []
	};
}

componentDidMount() {
	fetchPosts().then(response => {
		this.setState({
			posts: response.posts
		});
	});
	fetchComments().then(response => {
		this.setState({
			comments: response.comments
		});
	});
}
```


---
Reference

[[React] Component와 props, state](https://velog.io/@soyi47/React-Component-props-state)

[State and Lifecycle](https://ko.legacy.reactjs.org/docs/state-and-lifecycle.html)
