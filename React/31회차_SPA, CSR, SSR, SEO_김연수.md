[TOC]

> 💡 **한 줄 개념 요약**
>
> **SPA(Single Page Application)** : 한 개의 페이지로 구성된 애플리케이션  
> **MPA(Multiple Page Application)** : 여러 개의 페이지로 구성된 애플리케이션  
> **CSR(Client Side Rendering)** : 클라이언트 측에서 렌더링을 하는 방식  
> **SSR(Server Side Rendering)** : 서버 측에서 렌더링 하는 방식    
> **SEO(Search Engine Optimization)** : 검색 결과 상단에 위치할 수 있도록 트래픽의 양과 질을 극대화하는 작업

## SPA vs MPA

> SPA는 말 그대로 **하나의 페이지로 구성된 웹**이고, MPA는 **여러개의 페이지로 구성된 웹**이다.

<div align="center">
<img src="https://github.com/FrontCS-Study/CS-Study/assets/83412032/23b08669-0468-4e91-ae28-70a09c8a79c5" alt="spa vs mpa" width="70%">
</div>


### SPA(Single Page Application)

> 하나(Single)의 Page로 구성된 Application

SPA는 **단 하나의 HTML 파일**을 기반으로 **JavaScript를 이용해 동적**으로 화면의 컨텐츠를 바꾸는 방식의 웹 어플리케이션이다.

서버로부터 처음에만 페이지를 받아오고 이후 새로운 페이지 요청이 왔을 때 필요한 데이터만 전달받아서 클라이언트에서 필요한 페이지를 갱신하기 때문에 **CSR**로 렌더링 한다.

### MPA(Multiple Page Application)

> 여러 개(Multiple)의 Page로 구성된 Application

MPA는 **화면 마다 HTML파일이 존재**하고, 사용자가 그 화면을 **요청할 때마다**, 웹 서버가 필요한 **데이터와 HTML로 파싱**해서 보여주는 전통적인 웹페이지 구성 방식의 웹 어플리케이션이다.

사용자의 클릭과 같이 요청이 있을 때마다 서버로부터 이미 렌더링 된 정적 리소스를 받아오기 때문에 **SSR**로 렌더링 한다.

<br>

## CSR vs SSR

> 일반적으로 **SPA**에서는 렌더링 방식을 **CSR**을, **MPA**에서는 렌더링 방식으로 **SSR**을 사용하게 된다.
>
> 이러한 특징 때문에 SPA == CSR, MPA == SSR이라는 오해가 생기지만,   
> 페이지가 몇 개냐(SPA vs MPA), 렌더링을 어디서 하느냐(CSR vs SSR)에 따라 나뉘는 개념이다!

### CSR(Client Side Rendering)

CSR은 Client Side Rendering의 약자로, **클라이언트 측에서 렌더링** 하는 방식이다.

최초에 한번 서버에서 전체 페이지를 로딩하여 보여주고 이후에는 사용자의 요청이 올 때마다 리소스를 서버에서 제공한 후 클라이언트가 해석하고 렌더링을 하는 방식이다.

#### CSR 동작 과정

1\. 유저가 웹사이트에 방문하면, 브라우저가 서버에 콘텐츠를 요청한다.  
2\. 이에 서버는 빈 뼈대만 있는 HTML을 응답으로 보내준다.  
3\. 브라우저가 연결된 JavaScript 링크를 통해 서버로부터 다시 JavaScript 파일을 다운로드한다.  
4\. JavaScript를 통해 동적으로 페이지를 만들어 브라우저에 띄워준다.  

#### 장점

- **속도 및 응답시간** : 전체 페이지를 다시 렌더링하지 않고 변경되는 부분만 갱신하므로 ''화면 깜박임"이 없고, 클라이언트 측에서 연산, 라우팅 등을 모두 직접 처리하기 때문에 반응속도가 빠르다.

- **개발 간소화**
  서버 없이도 개발을 시작할 수 있다. 서버에서 페이지를 렌더링하기 위해 코드를 작성할 필요가 없다.  
  header와 footer와 같이 중복되는 내용은 고정으로 두고 안에 컨텐츠만 업데이트해 로드할 수 있다.

#### 단점

- **초기 구동 속도**
  최초 접근시 웹 애플리케이션에 필요한 모든 정적 리소스를 한번에 다운로드하기 때문에 초기 구동 속도가 상대적으로 느리다.

  > 💡 **CSR 초기로딩 속도를 개선하기** 
  >
  > code splitting 분리를 통해 JS번들 크기를 줄여서 초기 DOM생성속도를 줄이면 초기로딩 속도를 개선할 수 있다.  
  >
  > 모듈화 했던 파일들을 하나로 묶는 것을 번들링이라고 하는데, 번들링된 JS파일은 너무 커서 다운받는데 오랜 시간이 걸릴 수 있다. JS파일을 다운받는데 걸리는 시간동안 화면의 기능(버튼 이벤트 등) 작동하지 않기 때문에 하나로 번들링된 JS 파일을 페이지 별로 필요한 JS 파일로 분리한 것이 code splitting이다.

- **SEO(검색엔진 최적화) 불리**
  맨 처음 HTML이 비어 있어, 자바스크립트를 읽지 못하는 검색엔진에 대해서 크롤링이 되지 않아 색인할 콘텐츠가 존재하지 않게 되기 때문에 SEO에 불리하다. 

  > 💡 **CSR에서 SEO는 안되는걸까?** 
  >
  > 구글의 크롤러 봇은 자바스크립트를 실행하여 CSR 웹 크롤링도 가능하지만, 아직 완벽한 단계가 아니기 때문에 구글 측에서도 SSR을 권장한다.  
  > 네이버의 검색엔진도 SPA 수집 및 색인은 가능하지만 리소스가 많이 들어가는 작업이라 SSR을 권장하고 있다.

<br>

### SSR(Server Side Rendering)

SSR은 Server Side Rendering의 약자로, **서버 측에서 렌더링** 하는 방식이다.

사용자가 웹페이지에 접근할 때 서버에서 페이지에 대한 요청을 하면 서버에서는 html,view와 같은 리소스들을 렌더링하여 사용자에게 반환한다.

#### SSR 동작 과정

1\. 유저가 웹사이트에 방문하면, 브라우저가 서버에 콘텐츠를 요청한다.  
2\. 서버는 페이지에 필요한 데이터를 즉시 얻어와 모두 삽입하고, CSS까지 모두 적용해 렌더링 준비를 마친 HTML과 JavaScript코드를 브라우저에 응답으로 전달한다. 
3\. 브라우저에서는 JavaScript코드를 다운로드하고 HTML에 JavaScript로직을 연결한다. 

#### 장점

- **빠른 초기 로딩** : SSR은 클라이언트에서 요청한 페이지의 html을 다운로드 하기 때문에 CSR보다 초기 진입시 로딩이 빠르다.

- **SEO(검색엔진 최적화) 유리**

  크롤러 봇이 HTML을 무리 없이 읽을 수 있기 때문이다. 크롤러는 페이지를 인덱싱하기 위해 페이지에 관한 많은 정보가 필요한데, SSR을 활용하여 미리 페이지를 빌드하면 크롤러에게 많은 정보를 줄 수 있다. 이렇게 하면 검색엔진이 얻을 수 있는 유기적 트래픽의 양이 자동으로 향상된다.

#### 단점

- **페이지 이동 시 느린 속도**
  사용자가 새로운 페이지를 이동하면 전체 페이지를 다시 렌더링하기 때문에 "화면 깜박임"이 발생한다. 또한 HTML, CSS, JS 와 같은 리소스들이 새로 고쳐져서 속도에 영향을 받는다.  

- **TTV와 TTI 사이 간극 존재**

  TTV(Time to View)는 보여지는 시점, TTI(Time to Interact) 는 인터렉션 사용자와의 통신이 가능해지는 시점이다.  
  CSR은 사용자에게 보여짐과 동시에 모든 html과 js를 불러온 상태이기 때문에, TTV과 됨과 동시에 TTI 모든 동적인 행동을 할 수 있다. 
  SSR은 html을 js보다 먼저 받아오기 때문에 완성된 html로 화면은 확인 되지만 js다운로드가 늦어진다면 기능이 먹통일 경우가 발생할 수 있다.

<br>

### SSG(Static Site Generation)

SSG는 Static Site Generation의 약자로, Static Rendering 이라고도 불리는 방식이다.  

클라이언트에서 필요한 페이지들을 **사전에 미리 준비해뒀다가 요청을 받으면 이미 완성된 파일을 단순히 반환**하여 브라우저에서 뷰를 보여지게 된다.

> 💡 **SSR과 SSG의 차이점**
>
> 서버에서 HTML을 보내준다는 차원에서는 SSR과 유사하나, 요청할 때 즉시만드느냐 혹은 미리 만들어 놓느냐에 따라 차이가 있다.
>
> SSR은 요청 시 서버에서 즉시 HTML을 만들어 응답하기에 데이터가 달라지거나 자주 바뀌어서 미리 만들어 두기 어려운 페이지에 적합하다.  
> SSG는 페이지들을 서버에 미리 만들어 둔 후에 요청 시 해당 페이지를 응답하는 것이므로 바뀔 일이 거의 없어 캐싱해 두면 좋은 페이지에 사용된다.

<br>

## 렌더링 방식 선택 방법(CSR, SSR, SSG, Universal Rendering)

- **CSR**

  \- 유저와 상호작용이 많고, 대부분이 고객의 개인정보로 이루어진 서비스

  \- 검색엔진에 노출이 불필요한 경우

- **SSR**

  \- 회사 홈페이지여서 홍보나 상위노출이 필요하고, 누구에게나 항상 같은 내용을 보여주는 서비스

  \- 업데이트가 빈번해 해당 페이지 데이터가 자주 업데이트 되는 경우

- **SSG**

  \- 회사 홈페이지여서 홍보나 상위노출이 필요하고, 누구에게나 항상 같은 내용을 보여주는 서비스

  \- 업데이트를 거의 하지 않는 경우

- **Universal Rendering** (초기 렌더링으로는 CSR + 이후 SSR)

  \- 사용자에 따라 페이지 내용이 달라지고, 화면 깜빡임이 없는 빠른 인터렉션이 중요한 서비스

  \- SEO를 포기할 수 없어 상위노출이 필요한 경우

<br>

## SEO

네이버나 구글에 키워드를 검색하면 상위 노출된 페이지를 먼저 열람하게 된다.  
이때 상위 노출되는 결과는 크게 두 가지로 광고로 노출되는 PPC(Pay-per-click)와 SEO(Search Engine Optimization) 있다.

**SEO**는 광고비 집행 없이 자사의 홈페이지 혹은 콘텐츠를 **검색엔진에서의 자연 상위노출하는 작업**을 의미한다.  

> 🔍 **Google의 SEO 검색 과정(3단계)**
>
>  **1. 크롤링(Crawling):** 검색 엔진이 웹사이트를 방문하여 페이지 내용을 수집하는 과정
>
> - 구글 봇이 인터넷상의 웹페이지를 탐색하고 수집한다.
> - 크롤러가 수집하지 못하는 사이트나 페이지는 검색 결과에 나타나지 않을 수 있다.
>
> **2. 인덱싱(Indexing):** 검색 엔진이 웹페이지를 수집하고 분류하는 과정
>
> - 수집한 페이지의 내용을 분석하여 검색할 때 사용될 수 있는 정보로 변환한다.
> - 검색 엔진의 데이터베이스에 해당 페이지가 수록된다.
>
> **3. 검색 결과 제공(Serving search results)**
>
> - 사용자가 검색어를 입력하면, 구글은 해당 검색어와 관련된 페이지를 데이터베이스에서 찾아서 결과 페이지에 노출한다.
> - 구글은 검색어와 관련성이 높은 페이지와 검색 결과에 나타나는 순서를 결정하는 알고리즘을 이용한다.
>
> 따라서, SEO를 효과적으로 수행하기 위해서는 검색 엔진 크롤러가 우리 사이트의 페이지를 쉽게 찾을 수 있도록 웹사이트를 구성하고, 검색어와 관련된 정보를 포함하는 콘텐츠를 제공하는 것이 중요하다. 또한, 인덱싱이 제대로 이루어지도록 웹페이지의 구조와 내용을 최적화하는 작업이 필요하다.

### SEO를 필요성

- **비용 없이 지속적인 노출 가능**
  - 광고는 노출 속도가 빠르고, 검색 결과 상단에 위치시킬 수 있는 장점이 있지만, 단가 및 유지 비용이 비싸다는 단점이 있다.  
  - 반면에 SEO는 노출 순위를 올리는데 시간이 걸리지만, 비용이 들지 않고 지속적인 노출이 가능하다는 장점이 있다.
- **광고를 클릭하는 비율이 적다**
  - 전체 검색 유저중에서 대부분이 광고보다 자연 검색된 결과를 클릭하기 때문에 소비자가 홈페이지를 쉽게 발견 할 수 있도록 SEO을 통해서 노출 순위를 높이는 작업이 필요하다.

### SEO 최적화 방법

#### 1. Meta Data

Meta Tag는 웹페이지가 담고 있는 컨텐츠가 아닌 웹페이지 자체의 정보를 명시하기 위한 목적으로 사용되는 HTML 태그를 의미한다.  

title, name, content 등의 **meta tag** 를 활용하여 사이트의 제목과 설명 등을 설정할 수 있다.

##### React에서 SEO사용하기

 SSR은 정적인 페이지로 `meta` 태그들이 미리 정의되어서 SEO에 유리하지만, CSR의 경우에는 `meta` 태그 정의에 어려움이 있다.

`react-helmet` 패키지는 동적으로 SEO에 필요한 메타태그들을 쉽게 변경할 수 있게 도와주는 라이브러리이다.

```react
// App.js
import React from 'react';
import { Helmet } from 'react-helmet';

export default function App() {
  return (
    <div className="App">
      <Helmet>
        <title>리액트 헬멧을 이용한 타이틀 변경</title>
      </Helmet>
      Test 헬멧
    </div>
  );
}
```

> 🔍 **주요 검색 엔진·사이트에서의 Meta Keyword 인식 저하**
>
> Meta Keyword는아래와 같이 사용하며 쉼표로 여러개를 지정할 수 있다.
>
> ```html
> <meta name="keywords" content="키워드 1, 키워드 2, 키워드 3">
> ```
>
> 과거에 Meta Keyword는 SEO 최적화 과정에서 빠질 수 없는 요소였었지만 현재는 스팸성으로 전락해버렸다.
>
> 웹 사이트를 상단에 노출시키기 위해 메타 키워드를 마구잡이로 지정해버리게 되면서 검색엔진이 유저가 찾고자 하는 정보를 제공하지 못하게 되었기 때문이다.

#### 2. pre-render (SSR / SSG)

React + Next.js를 Vue + Nuxt.js를 사용하면 SPA에서도 SSR 렌더링이 가능하다.

#### 3. robots.txt 색인 방지

`robots.txt` 파일은 웹사이트의 루트 디렉토리에 위치하는 텍스트 파일로, 검색 엔진이 사이트의 어떤 부분을 크롤링하거나 혹은 인덱싱하면 안 되는지를 지정할 수 있다.

이 파일은 "User-agent" 지시어로 검색 엔진 봇에게 경로 접근을 허용하거나 차단하는 규칙을 설정한다.  
이처럼 `robots.txt`은 검색엔진에 중요한 역할을 하므로 색인 생성을 방지하는 설정이 없는지 주의해야한다.

React 에서는 public 폴더 내 존재한다.

#### 4. 이미지 최적화

구글에서는 텍스트는 물론 이미지 기반의 검색도 지원한다.  
그렇기 때문에 이미지를 관리할 때는 여러가지 원칙에 따라 설정해야, 검색 최적화가 된다.

이미지를 CSS 스타일이 아닌 HTML Img 태그로 설정해야한다는 점이다.  
크롤러는 HTML Img 태그로 이미지를 탐색하므로 CSS로 이미지를 등록하면 색인 생성이 안된다.

```react
<!-- 나쁜 예시 ❌ -->
<div style="background-image:url(cat.jpg)">먼치킨 고양이</div>

<!-- 좋은 예시 ⭕️ -->
<img src="cat.jpg" alt="먼치킨 고양이" />
```

<br>

### SEO 측정하는 법

크롬에서 개발자 도구(F12)를 열고 **Lighthouse**에서 **검색엔진 최적화(SEO)** 을 선택하고 **페이지 로드 분석**을 실시한다.

그 결과 해당 페이지에 대한 SEO 점수를 알려준다. 

**[통과한 검사]** 목록에서는 어떤 점이 SEO 점수에 긍정적인 영향을 줬는지 알려준다.  
 왜 점수가 낮는지는 **[콘텐츠 권장사항]** 에서 확인할 수 있다.

이처럼 **Lighthouse** 페이지 분석으로 SEO에 영향을 주는 항목을 확인하고 개선할 수 있다.

<br>

---

**참고**

(이미지 출처) [SPA vs MPA 개념, 장단점](https://velog.io/@yejine2/SPASingle-Page-Application-VS-MPA)

[MPA, SPA 차이, 장단점 - 코딩 일지 - 티스토리](https://green-grapes.tistory.com/entry/MPA-SPA-차이-장단점)

[렌더링 삼형제 CSR, SSR, SSG 이해하기](https://velog.io/@ka0son/렌더링-삼형제-CSR-SSR-SSG-이해하기)

[이제는 알아야겠다! CSR과 SSR의 차이점과 장단점 (SPA, MPA, SSG, Universal Rendering 까지)](https://dev-ellachoi.tistory.com/28)

[왜 CSR(Client Side Rendering)에서 SEO가 단점일까? - Minsoftk](https://minsoftk.tistory.com/68)

[SEO의 정의와 관련 개념, 필요성에 대하여](https://brunch.co.kr/@mobiinside/1470)

[검색엔진 최적화(SEO)란? | 요즘IT - 위시켓](https://yozm.wishket.com/magazine/detail/1540/)

[SPA에서 서버사이드 렌더링을 구축하지 않고 SEO 최적화하기](https://byseop.netlify.app/csr-seo/)

[[사용자 친화 웹] React에서 검색엔진 최적화(SEO)하는 방법들](https://velog.io/@lhj5924/사용자-친화-웹-React에서-검색엔진-최적화SEO하는-방법들)

[구글 검색 엔진과 SEO을 알아보자!(with. SEO 측정 방법)](https://mong-blog.tistory.com/entry/구글-검색-엔진과-SEO을-알아보자with-SEO-측정-방법)
