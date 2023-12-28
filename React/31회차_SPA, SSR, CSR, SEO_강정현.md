## SPA
### 멀티 페이지 애플리케이션(Multiple Page Application, MPA)
하나의 애플리케이션이 여러 개의 웹 페이지로 구성되어 있다.
클라이언트(웹 브라우저)가 새로운 페이지를 요청할 때마다 서버로부터 정적 리소스(static resource)를 내려받아 매번 전체 페이지를 리렌더링(re-rendering)하게 된다.

규모가 커지고 사용자와의 상호작용이 많아짐에 따라, 속도 저하 등의 문제가 발생하게 됨.
이러한 문제를 해결하기 위해 SPA가 등장함.

### 싱글 페이지 애플리케이션(Single Page Application, SPA)
**단 한 개의 페이지만으로 구성**되며, 애플리케이션에 필요한 모든 정적 리소스를 최초 단 한 번만 다운로드하여 렌더링하고, 이후 **새로운 페이지에 대한 요청이 있을 때에 페이지 갱신에 필요한 데이터만을 내려받아 리렌더링** 한다. 

사용자에게 보여주는 페이지는 한 페이지만, 사용자가 원하는 해당 페이지와 현재 사용자 브라우저의 주소 상태에 따라 다양한 화면을 보여준다. = `Routing(라우팅)`

<br/>

## CSR (Client-Side Rendering)
![CSR](https://tech.weperson.com/img/wedev/csr.png)
이미지 출처: [The Benefits of Server Side Rendering Over Client Side Rendering](https://medium.com/walmartglobaltech/the-benefits-of-server-side-rendering-over-client-side-rendering-5d07ff2cefe8)

1. 브라우저가 서버에 요청을 보내면, 서버는 요청에 따른 CSS, JS의 링크만 있는 빈 HTML 파일을 응답한다.
2. HTML 파일에 링크로 연결되어 있던 필요한 추가적인 CSS, JS의 내용을 다운로드 한다.
3. 브라우저가 JavaScript 프레임워크(React 등)을 실행한다. 
	1. 이 단계까지 빈 화면이 그려진다.
4. 필요한 모든 파일을 다운로드 완료하면, 이를 이용해 클라이언트에서 직접 동적으로 HTML을 구성하고, 웹 페이지 화면에 보여준다.

### 장점
1. 초기 로딩 이후에는 서버를 거치지 않고 클라이언트 단에서 동적으로 렌더링하기 때문에 빠른 사용자 경험을 제공할 수 있다.
2. `TTV(Time To View)`와 `TTI(Time To Interaction)` 사이에 간극이 없어 사용자 경험이 좋다.
3. JS및 CSS 파일과 같은 리소스는 한 번만 로드되고 이후 다시 요청되지 않기 때문에, 서버 과부하를 예방할 수 있다.

### 단점
1. 처음 HTML과 자바스크립트 파일이 모두 다운로드 되어야 브라우저에 그려지기 때문에, **초기 로딩 속도가 느리다.**
2. 빈 HTML 파일을 받아온 상태에서 클라이언트 동작을 시작하기 때문에, **SEO에 불리하다.**


<br/>

## SSR (Server-Side Rendering)
![SSR](https://tech.weperson.com/img/wedev/ssr.png)
이미지 출처: [The Benefits of Server Side Rendering Over Client Side Rendering](https://medium.com/walmartglobaltech/the-benefits-of-server-side-rendering-over-client-side-rendering-5d07ff2cefe8)

1. 서버가 "Ready to be rendered", 즉 **즉시 렌더링 가능한 HTML 파일**을 만들어 브라우저에 전송한다.
	1. 필요한 데이터와 CSS까지 적용되어 있는 상태.
2. 클라이언트에 전달되면, 이미 렌더링 준비가 되어있기 때문에 HTML은 즉시 렌더링 된다. **클라이언트가 JS를 다운 받기 전까지, 사이트 자체는 조작 불가능하다.** 만약 이 때 사용자 조작이 있는 경우, 해당 사용자 조작을 기억하고 있는다.
3. 브라우저가 JavaScript 프레임워크(React 등)을 실행한다.
4. JS까지 성공적으로 컴파일 되었기 때문에, 기억하고 있던 사용자 조작이 실행된다.

### 장점
1. 이미 DOM이 모두 구성된 파일을 브라우저가 받아 렌더링하기 때문에, **초기 구동 속도가 빠르다.**
2. 서버가 완전히 렌더링된 페이지를 클라이언트에게 전송하므로, 검색 엔진 크롤러는 직접 완전히 렌더링된 페이지를 통해 정보를 가져갈 수 있어 **SEO에 유리하다.**

### 단점
1. 화면에 변화가 생길 때마다 서버에서 HTML 파일을 보내주고 다시 화면을 그리기 때문에, **화면 전환 시 깜빡임(Blinking) 현상**이 발생할 수 있다.
2. 방문자 수가 많고 사용량이 많을 경우, **서버 과부하가 발생**할 수 있다.
3. `TTV(Time To View)`와 `TTI(Time To Interaction)` 사이에 간극이 있어, 사용자 경험에 부정적인 영향을 줄 수 있다.

<br/>

## 검색 엔진 최적화 (Search Engine Optimization, SEO)
검색엔진 결과로 웹사이트 또는 웹페이지의 상위 노출도를 높이는 작업.
검색엔진이 쉽게 이해할 수 있는 형태로 웹 페이지를 구성하는 과정이다.

### React에서 SEO 설정
#### 1. SSR 사용하기
next.js와 같은 SSR을 이용하여 CSR 환경에서 힘든 SEO 처리를 쉽게 할 수 있다.
1. CSR이 브라우저에서 html, js 파일을 받아 렌더링한다면, **SSR은 브라우저에서 해야할 렌더링을 서버에서 렌더링하여 html 파일을 브라우저에 보낸다.** 
2. API 요청을 서버에서 진행하고, 브라우저가 받은 html 문서에는 SEO 처리에 필요한 정보가 들어있다.
3. 이후 넘어온 데이터를 메타 태그에 담아 SEO 처리를 한다.

#### 2. meta tag 이용하기
title, description과 같은 meta tag를 설정한다.
React는 설정할 수 있는 정적 페이지가 index.html 하나 뿐이므로, `react-helmet` 라이브러리를 사용해서 페이지별 meta tag를 변경할 수 있다.

Open Graph Protocol - Open Graph 프로토콜은 모든 웹 페이지가 소셜 미디어에서 풍부한 컨텐츠를 담도록 해준다. `og : title`, `og : url`, `og : image`, `og : type`이 가장 중요하다.

**helmet 사용 예시**
```jsx
import {Helmet} from "react-helmet"

<Helmet
	title= "Page title"
	meta={[
		{"name": "description", "content": "Description of page"},
		{property: "og:type", content: "article"},
		{property: "og:title", content: "Example title"},
		{property: "og:image", content: "http://example.com/article.jpg"},
		{property: "og:url", content: "http://example.com/example"}
	]}
 />
```

<br/>

---

**Reference**

[React 이해하기](https://www.tcpschool.com/react/intro_understanding)

[CSR/SSR, SPA/MPA, PWA](https://tech.weperson.com/wedev/frontend/csr-ssr-spa-mpa-pwa/#csr-client-side-rendering)

[렌더링(CSR과 SSR)의 개념](https://www.ktpdigitallife.com/%EB%A0%8C%EB%8D%94%EB%A7%81csr%EA%B3%BC-ssr%EC%9D%98-%EA%B0%9C%EB%85%90/)

[[사용자 친화 웹] React에서 검색엔진 최적화(SEO)하는 방법들](https://velog.io/@lhj5924/%EC%82%AC%EC%9A%A9%EC%9E%90-%EC%B9%9C%ED%99%94-%EC%9B%B9-React%EC%97%90%EC%84%9C-%EA%B2%80%EC%83%89%EC%97%94%EC%A7%84-%EC%B5%9C%EC%A0%81%ED%99%94SEO%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95%EB%93%A4)

[React SEO(Search Engine Optimization) 검색 사이트 최적화 컴포넌트](https://sujinlee.me/react-seo/)

[Next.js 환경에서 SSR을 활용하여 SEO 처리하기](https://www.chanstory.dev/blog/post/4)

