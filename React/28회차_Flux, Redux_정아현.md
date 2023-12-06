# Flux

#### 🙄 나오게 된 이유

애플리케이션을 구현해 본 대부분의 개발자는 MVC라는 용어를 접해보았을 것이다. MVC 패턴이란 Model에 데이터를 저장하고, Controller를 사용해 Model의 데이터를 관리하는 소프트웨어 디자인 패턴의 하나이다. 이러한 MVC 패턴에서는 사용자가 View를 통해 데이터를 입력하면 View에서도 Model을 업데이트 할 수 있기 때무에 데이터가 양방향으로 전달될 수 있다.

하지만 이렇게 구현된 프로젝트는 규모가 커질수록 수많은 Model과 View가 생성되고, 이때 Model의 상태에 변화가 생길 경우 Model과 View 사이에 엄청난 양의 데이터가 양방향으로 전달되게 된다. 이로 인해 데이터의 흐름을 예측하기가 점점 힘들어지고, 수많은 버그를 발생시키는 원인이 된다.

이러한 문제 해결을 위해 2014년 페이스북(현 메타)에서는 Flux 패턴이라는 새로운 아키텍처를 제안했다.

![mvc patten](https://www.tcpschool.com/lectures/img_react_mvc-pattern.JPG)

### 💡 Flux 패턴

![flux patten](https://www.tcpschool.com/lectures/img_react_flux-pattern.JPG)

Flux 패턴은 사용자 입력을 기반으로 Action을 생성하고, 이를 Dispatcher에 전달해 Store의 데이터를 변경한 뒤 View에 반영하는 단방향의 데이터 흐름을 가지는 소프트웨어 아키텍처이다. Flux 패턴으로 구현된 프로젝트는 데이터가 단방향으로만 전달되기 때문에 데이터의 흐름을 파악하기가 용이하고, 그 결과를 쉽게 예측할 수 있다는 장점을 갖는다.

✔ **Action**

여기서 액션은 특별한 계층이라고 하기는 어려워 보인다. 이 구조에서 이 Action은 정확히 Action Creator라고 생각하면 된다. 예를 들어 클릭 같은 이벤트가 발생했을 때 그 이벤트가 발생했음을 Action 정보를 담고 있는 객체를 만들어 내 Dispatcher에 전달하는 역할을 한다.

✔ **Dispatcher**

Dispatcher는 들어오는 Action 객체 정보를 받아 실제로 어떤 행동을 할 지 결정하는 곳이다. Action 객체를 나누어 처리하고 미리 정해둔 Action 객체의 type을 구분해 미리 작성해 둔 명령들을 처리한다.
흔히 쓰이는 표현으로 Dispatcher는 중앙 허브 역할을 하고 있다고 한다. Dispatcher에서는 주로 Store에 있는 정보에 접근해 수정하는 명령이 많다. 이 때 상태 관리와 관련한 문제가 있을 수 있다. Store에 있는 상태 사이의 의존성이 있을 경우 이를 관리하는 것도 Dispatcher가 하는 일이다.

✔ **Store**

데이터, 상태를 담고 있다. React에서 우리는 이 Store를 Dispatcher와 연결해 Store에 접근할 수 있도록 callback명령을 제공할 수 있다.
또한 여기서 가지고 있는 상태에 View가 접근하고, 상태가 변경되면 View에서도 이를 반영한다.

✔ **View**

View는 일단 우리가 알고 있는 그 View의 역할을 한다. Store에서 어떤 이벤트(변경 등)가 발생하면 View는 변경된 점을 가져오고 이를 바탕으로 화면을 다시 렌더링한다. 
React에서 View는 특이하게 언제나 보여주는 역할만 하지는 않는다. Controller-View라는 존재가 있을 수 있는데, 이는 상위에서 하위 View들에게 어던 상태나, Dispatcher등을 제공하기 위해 하위 View를 감싸는 형태로 존재한다. 그런 방식으로 감싸면 굳이 하위 View로 계속 넘겨주는 불편함을 겪을 필요가 없어져 적절히 활용하면 도움이 된다.

> 이러한 Flux 패턴에 Reducer를 결합해 만든 것이 Redux이다.





# Redux

#### 🙄 나오게 된 이유

애플리케이션의 규모와 복잡도가 증가할수록 컴포넌트끼리 state를 공유해야 할 경우 수많은 컴포넌트를 복잡하게 거쳐야만 그 값을 전달할 수 있게 되었다. 또한, 필요에 따라 state를 비동기적으로 변경해야 할 경우도 많아지게 되었다. 따라서 state를 보다 효과적으로 관리할 수 있는 방법의 중요성이 대두되었으며, 이렇게 등장한 여러 상태 관리 라이브러리 중 하나가 바로 Redux이다.

### 💡Redux

Redux는 현재 많은 개발자들이 상태관리를 위해 즐겨 사용하고 있는 자바스크립트 상태 관리 라이브러리이다. Redux를 사용하면 state 관리 로직을 컴포넌트와 분리시킴으로써 효율적으로 관리할 수 있게 되며, 컴포넌트끼리 state를 공유할 때 여러 컴포넌트를 거쳐 prop를 복잡하게 전달(Props Drilling)하지 않아도 손쉽게 값을 전달할 수 있게 된다.

![Redux](https://www.tcpschool.com/lectures/img_react_redux-concept.JPG)

#### 🔍 Redux 동작 원리

Redux는 Store라는 전역 상태 저장소를 통해 state와 Reducer를 저장한다.
Redux는 애플리케이션 하나 당 하나의 Store만을 생성하며, 유용한 몇 가지 내장 함수를 같이 제공한다. 각 컴포넌트들은 이러한 내장 함수들을 사용해 Store의 데이터에 접근하고 변경을 요청할 수 있다.

Store를 변경하기 위한 로직을 저장하는 곳이 바로 Reducer이다.
Reducer는 Flux 패턴에는 없는 Redux만의 고유한 개념으로 현재 state와 Action(액션 type과 전송할 데이터(payload)로 이루어진 객체 형태)을 인수로 전달 받아 Store에 접근하고, 전달 받은 Action을 참고해 새로운 state를 만들어 반환한다. 
**상태를 변화시킬 때 반드시 이전 객체와는 다른 새로운 state 객체를 반환해야 한다.** 이렇게 되면 데이터(상태)가 변화하기 이전과 이후의 객체는 서로 다른 객체가 된다.
따라서 이전 상태 객체는 유지하였으므로 **불변성**을 지킨 것이며, Redux는 데이터가 변경되었다는 사실을 shallow equality checking을 통해 상태가 변화했음을 알 수 있다.

이러한 Reducer를 원하는 조건에 따라 호출하는 것이 바로 Action 이다. state에 어떠한 변화가 필요할 때 Action을 발생시켜(Dispatch) Reducer에 전달한다.

마지막으로 Dispatch와 함께 Store의 내장 함수 중 하나인 subscribe를 통해 Action이 Dispatch 될 때 마다 인수로 전달한 함수를 호출하도록 설정할 수 있다.

![Redux동작원리](https://www.tcpschool.com/lectures/img_react_redux-operating-principle.JPG)

Redux에서 state를 관리하는 작업의 흐름은 다음과 같이 진행된다.

1. 사용자가 UI를 통해 컴포넌트 내에 존재하는 이벤트를 호출한다.
2. 이벤트와 연결된 액션 생성 함수(Action Creator)가 호출된다.
3. 액션 생성 함수에서 생성된 Action이 호출된다.
4. 호출된 Action이 Reducer로 전달(Dispatch)된다.
5. Reducer에서 Dispatch된 Action에 따라 state값을 변경한다.
6. 변경된 state가 렌더링 되어 UI에 표시된다.



#### 📌 Redux의 세 가지 원칙

1. **하나의 애플리케이션 안에는 하나의 Store만 사용해야 한다.**

   (프로그램의 전역 상태는 단일 저장소 내의 트리에 저장된다.) 이렇게 함으로써 서버로부터 가져온 state가 직렬화(serialization)된 채 전달될 수 있으며, 클라이언트 측에서는 이를 추가적인 코드 없이도 곧바로 사용할 수 있다. 또한, 디버깅도 용이해진다.

2. **state는 읽기 전용이다.** 

   state를 변화시키는 유일한 방법은 Action을 전달하는 것 뿐이다. 이를 통해 모든 state 변화를 중앙에서 관리할 수 있으며, state 변경에 대한 추적이 용이해진다.

3. **state의 변화를 일으키는 Reducer는 순수 함수로 작성되어야 한다.**

   Reducer는 현재 state와 Action을 전달 받아 다음 state를 반환하는 순수 함수이다. 즉, 이전 state를 변경하는 것이 아니라 변화를 일으킨 새로운 state 객체를 생성해 반환하는 것이다. 똑같은 파라미터로 호출된 Reducer 함수는 언제나 똑같은 결과값을 반환해야 한다.
   `new Date()`를 사용하거나 랜덤 숫자를 생성하거나 혹은 네트워크에 요청을 하는 등의 실행할 때마다 다른 결과 값이 나오는 작업은 결코 순수하지 않은 작업이므로, Reducer 함수의 밖에서 처리해줘야 한다. 이를 위해, Redux middleware를 사용하곤 한다.



### ❓ Redux의 불변성

> 불변성 : 메모리 영역의 값을 변경할 수 없는 것

**⭐ Redux는 Shallow equality 검사로 상태의 변화를 감지한다.**

서로 다른 객체 (a, b)인 경우 단순히 `a === b`인지만 체크해도 다른지 알 수 있고, 이것을 **shallow equality**라고 한다. 반대로 **deep equality** 검사는, 두 객체(a, b) 내부 프로퍼티까지 모두 같은지 일일히 체크 하는 것을 맗한다. 따라서 두 가지 방식 중 성능적으로 당연히 shallow equality가 우위일 수 밖에 없다. 
그러므로 Reducer에서는 상태를 변화시킬 때 이전 상태 객체의 값을 변경하는 것이 아니라 아예 새로운 객체를 반환하는 것이다. 그리고 여기서 이전 상태 객체는 변경하지 않았으므로 **불변성**을 지켰다고 볼 수 있다.







## Reference

- [Redux 개요](https://www.tcpschool.com/react/react_redux_intro)

- [Web: React Flux 패턴](https://medium.com/hcleedev/web-react-flux-%ED%8C%A8%ED%84%B4-88d6caa13b5b)

- [Flux | 사용자 인터페이스를 만들기 위한 어플리케이션 아키텍쳐 (haruair.github.io)](https://haruair.github.io/flux/docs/overview.html)

- [[redux] 리덕스(Redux)란 무엇일까?: (1) 상태(state) 관리와 불변성](https://chanhuiseok.github.io/posts/redux-1/)