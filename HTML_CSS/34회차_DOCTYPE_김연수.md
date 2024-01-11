[TOC]

## DOCTYPE

DOCTYPE은 document type의 약어로, 웹 문서가 어떠한 형식으로 작성되었는지 HTML 최상단에 **문서의 형식을 선언하는 코드**이다.

DOCTYPE 선언의 형식은 HTML 태그는 아니지만, HTML에 가장 먼저 선언되는 것으로 `<!DOCTYPE html>` 와 같이 사용한다.  

HTML5, XHTML, HTML의 세가지 문서 유형이 존재하며, 기술한 유형에 따라 마크업 문서의 요소와 속성등을 처리하는 기준이 되며 유효성 검사에 이용된다.  `<!DOCTYPE html>`와 같은 단순한 스타일은 HTML5부터 사용된다.

- HTML5 버전 사용 예시 

  ```html
  <!DOCTYPE html>
  <html>
      ...
  </html>
  ```

> 🔍 **DOCTYPE 종류가 다양한 이유**
>
> 초기 HTML은 단순 텍스트위주의 표현을 위하였지만, 점차 발전을 통해 다양한 요소와 디자인 태그들도 생겼고 IE에 독주로 특정 브라우저에서만 볼 수 있는 기술들이 생겨났다.
>
> 하지만 점점 다양한 플랫폼 다양한 브라우저 사용이 늘어나면서 웹 표준이 필요하게 되었고,  
> 웹 표준을 만들기 위해 HTML4.01 XHTML의 등장의 과정을 겪고 현재 HTML5가 탄생했다.  
>
> 현재 HTML5는 다양한 브라우저에서 다 호환이 되고 HTML과 CSS를 분리하면서 기존 HTML의 디자인 태그를 삭제했다.  
> 하지만 그 전 버전은 한번에 모든 웹 페이지들이 코드를 바꿀 수 없기 때문에 발전 과정에 있어서  HTML 4.01이나 XHTML1.0이 남아있다.

### 사용 목적

**크로스 브라우징**을 위해 사용한다.

각 브라우저의 엔진이 렌더링 모드를 결정할 때 문서 첫 부분의 DOCTYPE을 참조하게 된다.  

DOCTYPE을 선언하지 않으면 브라우저가 해당 문서의 형식을 알 수 없게 된다.  
이 경우, 브라우저는 **표준 모드**가 아닌 **쿼크 모드**로 렌더링 모드를 변경하게 되는데, 쿼크 모드는 같은 내용의 HTML 문서라 할지라도 각 브라우저의 환경에 따라 제작자가 의도한 레이아웃(태그의 구조, CSS 등)이 깨지게 되어 사용자에게 정상적인 상태의 문서를 보여주지 못하게 된다.(크로스 브라우징 이슈 발생)

따라서 DOCTYPE으로 모든 브라우저들이 표준 모드로 HTML 문서를 동일하게 인식할 수 있도록 하고, 문서 간의 호환성을 높이기 위해 반드시 선언해야 한다.

> 🔍  **쿼크 모드와 표준 모드**
>
> - **쿼크 모드(Quirks mode)**
>
>   오래된 웹 브라우저를 위해 디자인된 웹 페이지의 하위 호환성을 유지하기 위해 표준모드를 대신하여 쓰이는 렌더링 모드. 오래된 웹 페이지들이 최신 버전의 브라우저에서 깨져보이지 않으려는 목적이다.
>
> - **표준 모드(Standard mode)**
>
>   W3C등의 표준을 준수하는 렌더링 모드.

>  💡 **크로스 브라우징(Cross Browing)**
>
>  크로스 브라우징이란, 브라우저 간 호환성 보장하는 작업을 말한다.  
>  즉, 웹 페이지 제작 시 모든 브라우저에서 깨지지 않고 의도한 대로 올바르게(호환성) 나오게 하는 작업이다.
>
>  크로스 브라우징을 고려하지 않으면 HTML, CSS, JS 등 코드가 원하는대로 작동이 안될 수 있다.  
>  HTML, CSS, JS 작성 시 W3C의 웹 규격에 맞는 표준 웹 기술을 적용해 모든 브라우저, 기기에서 사이트가 의도된 대로 보여지게 해야한다.
>
>  DOCTYPE을 선언으로 표준 모드를 유지하는 것은 크로스 브라우징 방법 중 하나이다.

<br>

## Meta Tag

메타 태그는 해당 문서에 대한 정보인 **메타데이터를 정의**할 때 사용한다.  
다시 말해 웹페이지가 담고 있는 컨텐츠가 아닌 웹페이지 자체의 정보를 명시하기 위한 목적으로 사용되는 HTML 태그를 말한다. 

> **메타 데이터(Meta Data)** : 웹사이트의 경우, 웹페이지에 대한 정보 제공 목적으로 사이트 설명, 키워드 등을 뜻한다.

메타 태그로 제공된 정보는 **브라우저와 검색 엔진을 사용**할 수 있게된다.

이러한 `<meta>`는 HEAD 태그 사이에 작성하며 **반드시 BODY 태그 앞쪽에 위치**해야 한다.

### 메타 태그 속성

메타태그 속성에는 http-equiv, name, content 3가지 속성이 있다.  

name 속성이나 http-equiv 속성이 명시되었다면 반드시 content 속성도 함께 명시되어야 하며, 반대로 두 속성이 명시되지 않았다면 content 속성 또한 명시될 수 없다.

#### http-equiv = "항목명"

• 웹 브라우저 서버에 명령을 내리는 속성   
• name 속성을 대신해 사용 가능  
• html 문서가 응답 헤더와 함께 웹 서버로 부터 웹 브라우저에 전송되었을 때만 의미를 가짐  
• content 속성의 정보 / 값을 위한 HTTP header를 제공  
• html4에서는 문자 설정을 할때 사용했지만 html5에서는 문자 설정 방법이 더욱 간단해짐

• 사용 예시  
html5에서는 `<meta>` 요소의 scheme 속성을 더 이상 지원하지 않으며, 문자셋(charset)을 손쉽게 정의할 수 있는 charset 속성이 새롭게 추가되었다.

```html
html4.01 : <meta http-equiv = "content-type" content ="text/html; charcet = UTF-8">
html5 : <meta charset = "UTF-8">
```

#### content = "정보 값"

• meta 정보의 내용을 지정  
• name 속성과 http-equiv와 연관된 값을 줌.

#### name = "정보 이름"

• 몇 개의 meta 정보의 이름을 정할 수 있으며 지정하지 않으면 http-equiv 와 같은 기능

### 메타 태그 종류 & 사용법

[메타(meta) 태그 종류 & 사용법](https://inpa.tistory.com/entry/HTML-📚-meta-태그-정리)

### 메타 태그로 검색엔진 최적화(SEO)

네이버(Naver), 구글(Google)과 같은 포털사이트의 **검색엔진 봇은 메타태그를 수집**하여 사용자에게 필요한 정보를 제공한다.

다음은 SEO에 중요한 메타 태그들이다.

#### Meta `title`

웹페이지의 고유 제목, 검색엔진이 웹페이지를 읽을 때 가장 먼저 읽는다.

```html
<meta name="title" content="This is Page Title">
```

#### Meta `description`

검색 결과에 표시되는 문자 지정

```html
<meta name="description" content="다양한 정보와 유용한 컨텐츠를 만나 보세요">
```

#### Meta `robots`

검색 로봇 제어

아래와 같이 하면, 페이지를 색인화 하고, 링크 연결이 된다.

```html
<meta name="robots" content="index, follow">
```

#### Meta `refresh redirect`

refresh : 입력한 주소로 5초 후 이동

```tsx
<meta http-equiv="refresh" content="5;url="https://example.com/">
```

#### Meta `charset`

문자 코드의 종류 설정

```html
<meta charset="utf-8">
```

#### Meta `viewport`

모바일 반응 페이지

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

<br>

---

**참고**

[[HTML]  선언 의미 및 현재는 어떻게 사용하는가?](https://ssd0908.tistory.com/entry/HTML-DOCTYPE-선언-의미-및-현재는-어떻게-사용하는가)

[[HTML] Doctype 이란 ? - Kenna - 티스토리](https://kenna-hwa.tistory.com/97)

[[TCPSchool] HTML \<meta> 태그](https://tcpschool.com/html-tags/meta)



[메타 태그 (Meta Tag)로 검색엔진 최적화하기 (SEO) 🔍 👀 👍](https://velog.io/@devjeenie/메타-태그-Meta-Tag로-검색엔진-최적화하기-SEO)

