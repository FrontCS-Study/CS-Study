# Next.js

### 💡 Next.js

Next.js 는 리액트 기반의 서버사이드 렌더링 프레임워크이다. 

Next.js는 **SEO**에 유리하다. 서버사이드 렌더링으로, 모든 페이지에 대한 HTML 마크업이 서버에서 생성된다. 이는 검색엔진에서 해당 페이지를 더 잘 인식하고 색인화하는데 도움이 된다.

Next.js는 **페이지 로딩 속도**를 크게 개선한다. SSR로 페이지가 완전히 로드되기 전에 사용자에게 보여지는 콘텐츠가 있다. 이는 초기 로딩시간을 단축하고 사용자 경험을 향상시킨다.

Next.js는 **개발 생산성**을 높인다. 자동 코드 분할, 환경 변수, TypeScript 지원 등과 같은 기능을 제공한다. 이는 개발자가 빠르게 프로젝트를 구축하고 유지보수하는데 도움이 된다.

Next.js는 **확장성**이 높다. Vercel과 같은 클라우드 플랫폼에서 쉽게 배포할 수 있으며, 서버리스 아키텍처와 함께 사용할 수 있다. 이는 애플리케이션 규모가 커질수록 유용하다.

> React는 프론트엔드 웹 개발에 가장 널리 사용되는 라이브러리 중 하나이다. 반면, Next.js는 서버 사이드 렌더링(SSR) 및 정적 사이트 생성(SSG)과 같은 기능을 제공하는 프레임워크이다.

#### ❔ Next.js 와 React 간의 차이

1. **서버 사이드 렌더링**
   - React는 클라이언트 측에서만 렌더링 되지만, Next.js 는 서버 측에서도 렌더링할 수 있다.
   - SSR은 페이지 로딩 속도를 개선하고 SEO를 개선하는데 도움이 된다.
2. **정적 사이트 생성(SSG)**
   - Next.js는 빌드 시점에 페이지를 렌더링하고 HTML 파일을 생성해 정적 사이트를 생성할 수 있다.
   - SSG는 페이지 로딩 속도를 더욱 빠르게 만들며, CDN가 같은 캐싱 기술을 사용할 수 있다.
3. **파일 시스템 기반 라우팅**
   - Next.js는 파일 시스템 기반 라우팅을 사용한다.
   - 이것은 개박자가 페이지 및 경로를 생성하고 구성하는데 더욱 편리하고 직관적인 방법을 제공한다.
4. **개발 생산성**
   - Next.js는 자동 코드 분할, TypeScript 지원 등과 같은 기능을 제공해 개발 생산성을 높인다.
   - 또한, Next.js는 Vercel과 같은 클라우드 플랫폼에서 쉽게 배포할 수 있다.



### 💡 Next.js의 렌더링 수행 방식

Next.js는 페이지 별로 렌더링 방법을 전환할 수 있다. 사용할 수 있는 방법은 총 4가지이다.

> - 정적 사이트 생성(SSG: Static Site Generation)
> - 클라이언트 사이드 렌더링(CSR: Client Side Rendering)
> - 서버 사이드 렌더링(SSR: Server Side Rendering)
> - 점진적 정적 재생성(ISR: Incremental Static Regeneration)

모든 페이지에서 만약 사전 렌더링이 가능하다면 사전 렌더링을 수행한다.

#### ✔ 정적 사이트 생성 (SSG)

SSG에서는 빌드 시 API등으로부터 데이터를 얻어, 페이지를 그려 정적 파일로 생성한다.

빌드 시에 `getStaticProps`함수가 호출, 함수 내 API 호출 등을 수행하고, 페이지를 그리는데 필요한 props를 반환한다. 화면을 그린 후 해당 결과는 정적 파일의 형태로 빌드 결과로 저장한다. 
페이지 접근이 발생하면 미리 생성한 정적 파일을 클라이언트에 보낸다.
초기 페이지를 그린 이후 클라이언트는 일반 리액트처럼 동적으로 화면을 그릴 수 있다.

SSG는 유저가 접근 시 가지고 있던 정적 파일을 전달하기만 하기 때문에 초기 화면을 그리기까지가 빠르다. 하지만, 가지고 있던 데이터가 마지막 빌드 시점의 데이터이기 때문에 stale 상태이므로 오래된 데이터가 보여질 가능성이 크다. 그러므로 실시간성이 중요한 웹 서비스에는 적절하지 않는 방법이다. 블로그 등의 서비스가 적절해보인다. 또한 성능이 뛰어나기 때문에 Next.js에서도 SSR보다는 SSG를 권장한다.

#### ✔ 클라이언트 사이드 렌더링 (CSR)

이는 기본적인 리액트로 프로젝트를 만들었을 때 사용하게 되는 일반적인 방법이다. 서버로부터 빈 HTML 파일을 받아와 해당 DOM에 비동기로 가져온 데이터를 추가한 뒤 그려 저장한다.

그렇기에 유저는 데이터가 오기까지 초기 로딩 혹은 빈 페이지를 보게 될 가능성이 커 초기화면을 그리는 것이 중요하지 않다. 그러므로 실시간성이 중요한 페이지에 적합하다.

#### ✔ 서버 사이드 렌더링 (SSR)

유저가 접근했을 때마다 `getServerSideProps`를 호출하고, 그 결과 props를 기반으로 한 페이지를 서버 측에서 그린 후 클라이언트에 전달한다.

SSG와 다르게 매 접근시마다 서버에서 데이터를 가져와 화면을 그리기 때문에 오래된 데이터를 보여주거나 SEO에 대한 유효성을 기대할 수 있지만, 그만큼 클라이언트에서 처리가 많아지기 때문에 다른 방법에 비해 지연이나 부하가 생길 수 있다.

최신 가격이 표시되는 제품 페이지 등, 항상 최신 데이터를 보여주고자 하는 서비스에 적합하다.

#### ✔ 점진적 정적 재생성(ISR)

ISR은 SSG의 응용아리 할 수 있는 렌더링 방식이다. 사전에 생성해둔 페이지를 전달하면서, 동시에 접근이 발생함에 따라 페이지를 다시 생성해 새로운 페이지를 전송할 수 있다.
페이지에 접근이 발생했을 때 SSG처럼 기존에 생성해둔 정적 파일을 보내주지만 이 때의 데이터에 유효 기간을 설정할 수 있고 만약 유효기간이 지난 시점에서의 접근이 발생한다면 백그라운드에서 다시 `getStaticProps`를 호출해 화면을 그리고 서버에 저장된 페이지의 데이터를 생신한다.

SSR과 달리 요청 시에 서버에서 처리를 하지 않기 때문에 SSG와 마찬가지로 초기 지연을 짧게할 수 있고 어느정도 최신 데이터를 기반으로 하는 페이지를 초기에 그리는 것이 가능하다.
SSG와 SSR의 중간과 같은 렌더링 방법이다.



### 💡 사전 렌더링

사전 렌더링(Pre-Rendering)의 원리는 간단하다.

![processOfPreRendering](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FkRa9x%2FbtrShH7Rg6c%2FmkRdXNkY9TjpKGwo2ZcrY0%2Fimg.png)

1. 뼈대만 있는 HTML이 아닌, 서버로부터 말 그대로 **사전에 렌더링된 HTML 파일**을 받아와 **화면을 먼저 출력**하고,
2. **JS 파일을 받아오는 과정**(hydrating)을 거치게 된다.

![csr](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fene7X0%2FbtrSeNHwdmk%2FBTCIJBhRJ1b3qhlR1UxATK%2Fimg.png)

위는 리액트, 즉 CSR 방식으로 렌더된 화면이다.
body 태그안에 root id를 가진 div 태그 하나만 존재한다는 것을 볼 수 있는데, 이 점은 CSR이 SEO에 불리한 이유이기도 하다. 이것이 바로 뼈대만 있는 HTML 파일을 의미한다.

![preRendering](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F6SCDy%2FbtrScaJY5eY%2FISzN7W1BKl7WJ9s0CRj1MK%2Fimg.png)

위는 Next.js 앱을 실행시킨 뒤 살펴본 소스코드 이다.

**뼈대 HTML만 있는 것이 아닌, 화면을 보여주는 여러가지 태그들이 소스코드에 포함**되어 있는 것을 볼 수 있다. 이 점이 **사전 렌더링 방식을 사용하게 될 경우, SEO가 용이**한 이유이다.

이렇게 화면에 보여줄 내용들이 포함되어 있는 HTML 파일을 사전 렌더링 된 HTML 파일이라고 부른다.
물론 사전 렌더링 된 HTML역시 단순한 HTML 파일이기 때문에 단순히 이 파일만으로는 유저와의 상호작용(버튼 클릭, 데이터 요청, etc..) 이 불가능하다.

일단, 렌더링 된 화면을 유저에게 보내준 후, Next.js는 JS 파일을 받아오고 실행시켜 유저와 상호작용이 가능한 상태가 된다. 그리고 우리는 이렇게 유저와 상호작용이 사능한 상태가 되는 것은 hydration이라고 부른다.

Next.js에서는 이런 **사전 렌더링 방식**으로 다음과 같은 **2가지 방식**을 제공하고 있으며, 두가지 방식을 섞어서 사용도 가능하다.

1. **Static Generation (정적생성)**
   **추천되는 방식**으로 build타임(npx build)에 HTML파일을 미리 생성해 놓고, 이를 요청이 있을 때마다 재사용 하는 방식이다. 빌드 시, **미리 생성된 HTML파일을 CDN에 캐시**해놓고, 유저의 요청이 있을 때마다 재사용이 가능해 **성능적으로 매우 유리**하다.
2. **Server-side Rendering(서버 사이드 렌더링)**
   요청이 있을 때마다, 필요한 HTML파일을 생성하는 방식이다.

참고로, 위에 언급한 것처럼 Next.js에서는 가능하다면 **정적생성방식을 추천**하고 있다. 
하지만, development환경에서 실행하는 경우(npm run dev), 항상 SSR방식으로 동작하게 된다. 



### 💡 하이드레이션

> Hydrate는 Server Side 단에서 렌더링 된 정적 페이지와 번들링 된 JS파일을 클라이언트에게 보낸 뒤, 클라이언트 단에서 **HTML 코드와 React인 JS 코드를 서로 매칭시키는 과정**을 말한다.

![process of hydrate](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FRZ89B%2FbtrbENofpfp%2FfBn3QJncluRPEBQNpeXjE1%2Fimg.png)

Hydrate는 SSR에서 사용되는 개념이다. SSR의 경우 pre-rendering을 통해 완성된 HTML을 클라이언트에게 전달한다. 이렇게 **서버에서 렌더링된 정적 페이지**를 클라이언트에게 보내고, **react는 번들링된 JavaScript 코드를 클라이언트에게** 보낸다. 클라이언트는 전달받은 **HTML과 JS코드를 매칭하는 Hydrate를 수행**한다. Hydrate란 전송받은 JavaScript들이 이전에 보내진 HTML DOM 요소 위에서 **한 번 더 렌더링** 하게 되면서 각각 자기 자리를 찾아가며 매칭되는 것이다. Hydrate 후에는 클릭과 같은 이벤트나 모듈들이 적용되어 사용자 조작이 가능해진다.

즉, Hydrate는 클라이언트 측 JavaScript가 정적 호스팅 또는 서버 측 렌더링을 통해 전달되는 정적 HTML요소에 이벤트 핸들러를 첨부해 동적 웹 페이지로 변환하는 기술이다. 

SSR 덕분에 사용자는 UI를 먼저 볼 수 있고, Hydrate 덕분에 JS 코드가 매칭되어 추후에 사용자 조작이 가능한 것이다.

#### 🔍 React 의 Hydrate

사실 Hydrate는 Next.js 에 종속된 동작이 아니라 ReactDOM 함수이다.

```react
const element = <h1>Hello!</h1>;
ReactDOM.render(element, container, [callback])
```

element: 화면에 그려질 react의 element
container: 리액트 element를 container DOM에 연결
callback: 렌더링 후에 반환되는 값을 돌려주는 콜백 함수

즉, ReactDOM.render는 두번째 파라미터인 지정된 DOM 요소에 하위로 주입해 렌더링한다.

```react
const element = <h1>Hello!</h1>;
ReactDOM.hydrate(element, container, [callback])
```

렌더링을 통해 새로운 웹페이지를 구성하지 않고, 기존 DOM Tree에서 해당되는 DOM요소를 찾아 정해진 자바스크립트 속성들만 적용시키는 Hydrate를 수행한다.



> **React의 웹 페이지 구성 원리**
>
> React는 JS파일만을 이용해 웨 화면을 구성하는 원리를 가지고 있다. 그래서 실제 HTML 코드는 안에 내용이 하나도 없는 상태이다. (Client Side Rendering이 SEO에 적합하지 않은 이유이기도 하다.)
> 단순 뼈대만 있는 HTML document와 JS파일들을 클라이언트로 모두 보낸 뒤, 클라이언트 단에서 JS코드들을 통해 웹 화면을 렌더링하며 페이지를 그리게 된다. 웹 페이지 렌더링을 한 뒤에도 페이지 내에서 동작하는 모든 이벤트 또한 자바스크립트로 인해 일어나게 된다.
>
> ```REA
> // src/index.js
> 
> import React from "react";
> import ReactDOM from "react-dom";
> import App from './src/App';
> 
> ReactDOM.render(<App />, document.getElementById("root"));
> ```
>
> 위 코드처럼 public/index.html에는 아무 내용 없는 기본 뼈대만 있고, 나머지는 src/index.js의 자바스크립트 코드에서 모든 화면을 렌더링 한 뒤 HTML DOM요소 중 root라는 아이디를 가진 엘리먼트를 찾아서 하위로 주입을 하게 된다.

> **Next.js의 웹 페이지 구성 원리**
>
> Next.js는 클라이언트에세 웹 페이지를 보내기 전에 Server Side 단에서 미리 웹 페이지를 Pre-Rendering한다. 그리고 Pre-Rendering으로 인해 생성된 HTML document를 클라이엉ㄴ트에게 전송한다. 
> 중요한 것이 아래 내용인데,
> 현재 클라이언트가 받은 웹 페이지는 단순히 웹 화면만 보여주는 HTML일 뿐이고, 자바스크립트 요소들이 하나도 없는 상태이다. 이는 웹 화면을 보여주고 있지만, 특정 JS 모듈 뿐 아니라 단순 클릭과 같은 이벤트리스너들이 각 웹 페이지의 DOM 요소에 하나도 적용되어 있지 않은 상태임을 말한다.
> Next.js Server에서는 Pre-Rendering된 웹 페이지를 클라이언트에게 보내고 나서, 바로 리액트가 번들링된 자바스크립트 코드들을 클라이언트에게 전송하다.
> 그리고 이 JS 코드들이 이전에 보내진 HTML DOM요소 위에서 한 번 더 렌더링을 하면서, 각자 자기 자리를 찾아가며 매칭이 된다. 
> 이 과정을 **Hydrate**라고 부른다.
> 이것은 마리 자바스크립트 코드들이 DOM 요소 위에 물을 채우듯 필요로 하던 요소들을 채운다 하여 Hydrate(수화) 라는 용어를 쓴다고 한다.

#### 🔍 Serer에서 렌더링 후 Client에서도 한 번 더 렌더링 하면 비효율적이지 않을까?

어쩌면 두 번 렌더링 하는 것을 비효율적으로 보일 수 있다.

그러나 서버 단에서 빠르게 Pre-Rendering하고 유저에게 빠른 웹 페이지로 응답할 수 있다는 것에 더욱 큰 이점을 가져갈 수 있다. 심지어 이 Pre-Rendering 한 Document는 모든 자바스크립트 요소들이 빠진 굉장히 가벼운 상태이므로 클라이언트에게 빠른 로딩이 가능하다.
이는 같은 화면에 대해 두 번 렌더링이 일어난다는 단점을 보완하고도 남는다.

더 나아가 클라이언트 단에서 자바스크립트가 렌더링을 할 때, 단지 각 DOM 요소에 자바스크립트 속성을 매칭 시키기 위한 목적이므로 실제 웹 페이지를 다시 그리는 과정까지는 하지 않는다.(Paint함수 호출 하지 않음.)





## Reference

- [Brunch, Next.js 배우기 1(React에서 Next.js)](https://brunch.co.kr/@jennyjang93/32)

- [Velog, Hydrate(Next, React 18)](https://velog.io/@leehyunho2001/Hydrate)
- [Tistory, Next.js의 Hydrate란?](https://helloinyong.tistory.com/315#:~:text=Next.js%EC%9D%98%20%EC%9B%B9%20%ED%8E%98%EC%9D%B4%EC%A7%80,%EB%A5%BC%20%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8%EC%97%90%EA%B2%8C%20%EC%A0%84%EC%86%A1%ED%95%9C%EB%8B%A4)
- [Velog, Next.js의 렌더링 방법](https://velog.io/@jvn4dev/Next.js%EC%9D%98-%EB%A0%8C%EB%8D%94%EB%A7%81-%EB%B0%A9%EB%B2%95)
- [Tistory, [Next.js] CSR과 비교하여 pre-rendering 쉽게 이해하기](https://marklee1117.tistory.com/99)