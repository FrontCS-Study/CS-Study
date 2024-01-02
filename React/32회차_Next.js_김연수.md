[TOC]

## Next.js

### Hydrate 의미

Hydrate는 Server Side 단에서 렌더링 된 정적 페이지와 번들링된 JS파일을 클라이언트에게 보낸 뒤, 클라이언트 단에서 **HTML 코드와 React인 JS코드를 서로 매칭 시키는 과정**을 말한다.

Next js에서 새롭게 만든 것이 아닌 기존 리액트에서 지원하고 있는 함수다. 리액트에서 CSR이 아닌 SSR을 사용할 때 사용된다.

Next js로 제작된 페이지는 미리 렌더링 된 정적 HTML을 내려 받은 후 최소한의 JS코드들을 DOM에 붙여 렌더링 함으로써 브라우저에서 새로 **Paint과정을 거치지 않고 렌더링**한다. 때문에 사용자에게 빠르게 화면을 보여줄 수 있다.

#### React의 웹 페이지 구성 원리

리액트의 웹 페이지는 CSR기반으로 처음 프로젝트를 세팅하면 단순 뼈대만 있는 HTML document와 JS 파일들을 클라이언트로 모두 보낸 뒤, 클라이언트 단에서 JS 코드들을 통해 웹 화면을 렌더링 하며 페이지를 그리게 된다.  
웹 페이지 렌더링을 한 뒤에도 페이지 내에서 동작하는 모든 이벤트 또한 자바스크립트로 인해 일어나게 된다.

#### Next.js의 웹 페이지 구성 원리

Next.js는 클라이언트에게 웹 페이지를 보내기 전에 Server Side 단에서 미리 웹 페이지를 **Pre-Rendering** 한다. 그리고 Pre-Redering으로 인해 생성된 HTML document를 클라이언트에게 전송한다. (이때,  클라이언트 화면은 단순히 웹 화면만 보여주는 HTML일 뿐이고, 자바스크립트 요소들이 하나도 없는 상태이다.) 

그리고 바로 리액트가 번들링 된 자바스크립트 코드들을 클라이언트에게 전송한다. 

그 다음, 이 자바스크립트 코드들이 이전에 보내진 HTML DOM 요소 위에서 한번 더 렌더링을 하면서, 각자 자기 자리를 찾아가며 매칭이 된다. 이 과정을 **Hydrate**라고 부른다.

> 🔍 **Server에서 한번 렌더링하고 Client에서도 한번 더 렌더링 하면 비효율적인 렌더링 방식 일까?**
>
> 서버 단에서 빠르게 Pre-Rendering하고 **유저에게 빠른 웹 페이지로 응답할 수 있다는 것에 더욱 큰 이점을 가져갈 수 있다.** 심지어 이 Pre-Rendering 한 Document는 모든 자바스크립트 요소들이 빠진 굉장히 가벼운 상태이므로 클라이언트에게 빠른 로딩이 가능하다. 이는 같은 화면에 대해 두 번 렌더링이 일어난다는 단점을 보완하고도 남는다.
>
> 더 나아가서 클라이언트 단에서 자바스크립트가 렌더링을 할 때, 단지 각 DOM 요소에 자바스크립트 속성을 매칭 시키기 위한 목적이므로 실제 웹 페이지를 다시 그리는 과정까지는 하지 않는다. (Paint 함수 호출되지 않음)

### *ReactDOM.render()*  vs _ReactDOM.hydrate()_

Hydrate는 Next.js에 종속된 동작이 아니라 *ReactDOM* 함수이다.

리액트는 클라이언트 렌더링만 있어 ReactDOM.render() 함수를 사용하여 유저에게 보여줄 HTML, CSS 그리고 자바스크립트 모두 `render()` 함수를 이용해 생성하여, React가 어떤 DOM을 렌더하는지 알려준다.

반면에 Next.js는 서버에서 보여줄 HTML 컨텐츠를 가져오기 때문에재차 `render()` 함수로 HTML을 생성하여 DOM을 그리는 일은 비효율적이다. 이는 렌더링을 통해 새로운 웹 페이지를 구성할 DOM을 생성하는 것이 아니라, 기존 DOM Tree에서 해당되는 DOM 요소를 찾아 HTML에 유저가 상호작용할 수 있는 자바스크립트 속성(이벤트 리스너 등)들만  연결하는 것이다.

<br>

### Next.js 기본 설정 파일

#### App Router vs Pages Router

기존 Next.js는 `/page` 폴더에 모든 웹 사이트의 페이지 컴포넌트를 처리했다.

최근 Next.js 13버전부터 부터 프로젝트 생성시 App Router를 기본으로 설정되게 변경되었다.  
(단, 모든 설정 No 선택경우 기존 pages방식으로 세팅됨 (_Typescript나, src/폴더 사용 등..._)) 

공식문서에서 App Routers는 `/app` 폴더 내에 페이지 컴포넌트 설정이 가능하다.

<div align="center">
<img src="https://www.notion.so/image/https%3A%2F%2Ffile.notion.so%2Ff%2Ff%2F41cba087-9d68-4afd-b3f1-bc7d4e28e11b%2Fb74d8faa-8465-4cee-88be-467744988a3b%2FUntitled.png%3Fid%3Dcc45a2fb-cdc0-4315-b9ed-4d95af6c2a79%26table%3Dblock%26spaceId%3D41cba087-9d68-4afd-b3f1-bc7d4e28e11b%26expirationTimestamp%3D1704268800000%26signature%3DkcrriMc2U8gbbPZNziH6RqA9QN6Q5rIlU-mz-olbLDI?table=block&id=cc45a2fb-cdc0-4315-b9ed-4d95af6c2a79&cache=v2" width="80%"/>
</div>

아직까지는 pages router 처리 방식을 사용하는 블로그들이 많은듯...?

<br>

---

**참조**

[리액트의 hydration이란?](https://simsimjae.tistory.com/389)

[Next.js의 Hydrate란? - 이뇽의세상 - 티스토리](https://helloinyong.tistory.com/315)

[Next.js의 렌더링 과정(Hydrate) 알아보기](https://www.howdy-mj.me/next/hydrate)

[Next.js 13부터의 App Router와 기존 Page Router의 차이 비교](https://www.jadru.com/diffrent-approuter-and-pagerouter)
