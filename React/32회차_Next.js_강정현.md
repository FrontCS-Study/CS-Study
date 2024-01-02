## Next.js
Next.js는 리액트를 위해 만든 오픈소스 자바스크립트 웹 프레임워크로, 리액트에는 없는 SSR, SSG, ISR(Incremental Static Regeneration)과 같은 기능을 제공한다.

<br/>

## Next.js의 렌더링 순서
### 1. Pre-Rendering
서버에서 각 페이지의 html 파일을 미리 생성하는 것.
![Pre-Rendering](https://nextjs.org/static/images/learn/data-fetching/pre-rendering.png)
(이미지 출처: https://nextjs.org/static/images/learn/data-fetching/pre-rendering.png)

1. **Initial Load**: 특정 페이지의 컴포넌트를 Pre-Rendering하여, 미리 서버에서 생성한 html을 전달하여 사용자에게 화면을 보여준다.
2. **Hydration**: html에 JS 코드를 주입하여 동적인 화면으로 바뀐다.

#### Pre-Rendering 의 종류
1. SSG (Static Site Generation)
**모든 페이지에 대한 html 문서를 빌드할 때 생성**하고, 요청이 올 때마다 만들어 둔 html 문서를 재활용하는 방식.
SSR 방식이면서 빠르게 클라이언트에게 응답할 수 있으나, 사용자와 상호작용 해야 하는 부분, 즉 매 요청마다 페이지에 대한 내용이 변경되어야 하는 경우라면 적합하지 않을 수 있다.

2. SSR (Server Side Rendering)
**매 요청마다 페이지에 대한 html 문서를 렌더링**하여 클라이언트에게 전달하는 방식.
`getServerSideProps` 함수를 정의하면, 해당 페이지는 항상 매 요청마다 html 문서를 Pre-Rendering 함.


>최초 렌더링 이후 클라이언트 사이드에서 페이지 변경 시, 서버에서 Pre-Rendering 과정이 일어나는가?

`useRouter`를 사용해 `router.push`로 페이지를 변경한 경우, CSR 방식으로 렌더링되어 서버 측의 Pre-Rendering 과정은 일어나지 않는다.
서버 측에서 Pre-Rendering이 진행되는 경우:
- JavaScript 에서 `location.href` 로 페이지를 이동했을 경우
- html에서 a태그의 `href`로 페이지를 이동했을 경우
- 브라우저 주소창에 url을 입력하여 해당 페이지에 접근했을 경우
- 새로고침 한 경우

<br/>

### 2. 클라이언트로 Pre-Rendering한 html 문서 전달
Next.js가 요청 온 브라우저에게 **Pre-Rendering한 html 문서를** 응답한다.

<br/>

### 3. 클라이언트에서 Rendering 시작 (Hydration)
브라우저(클라이언트)는 서버에서 받은 html 문서를 화면에 렌더링한다.
그리고 Next.js 서버에서 바로 JS 코드를 클라이언트에게 전송한다.

클라이언트에서는 `ReactDOM.render()` 함수를 이용해 React Element를 렌더링하고, 
`ReactDOM.hydrate()` 함수를 이용해 DOM 요소에 이벤트 리스너를 적용하고 렌더링을 진행한다.


<br/>

---
**Reference**

[Next.js 의 렌더링 과정에 대하여](https://funveloper.tistory.com/164)

[Next.js에서 꼭 알아야하는 Pre-rendering](https://eun-jee.com/post/front-end/nextjs/03-prerendering)

[Next.js의 렌더링 과정(Hydrate) 알아보기](https://www.howdy-mj.me/next/hydrate)

[Next.js의 Hydrate란?](https://helloinyong.tistory.com/315)
