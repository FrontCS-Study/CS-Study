# DOCTYPE

HTML에서 DOCTYPE은 모든 문서의 최상단에서 찾을 수 있는 필수 서문이다.

### 💡 !DOCTYPE 이란?

document type의 약어로, **웹 문서가 어떤 형식으로 작성되었는지 문서 형식을 선언**하는 것이다. 브라우저에게 HTML의 버전 및 웹 브라우저 내용을 잘 출력할 수 있도록 도와주는 역할을 하여 호환성을 높일 수 잏다.

DOCTYPE은 HTML 파일의 최상단에 위치해야 한다.

```html
<!-- HTML5 -->
<!DOCTYPE html>
```

### 🔍 !DOCTYPE을 쓰지 않을 경우 어떻게 될까?

웹 브라우저는 문서 형식 선언이 없으면 쿼크 모드(quirks mode)로 렌더링해서 각 브라우저마다 다른 형태의 결과물을 보여준다. 이것을 방지하기 위해 문서 형식 선언을 하며 이로 인해 HTML 문서를 표준 모드로 렌더링 할 수 있게 된다.

> **❔ 쿼크 모드(Quirks mode)?**
>
> 오래된 웹 브라우저를 위해 디자인된 웹 페이지의 하위 호환성을 유지하기 위해 표준모드를 대신해 쓰이는 렌더링 모드. 오래된 웹 페이지들이 최신 버전의 브라우저에서 깨져 보이지 않으려는 목적이다.
>
> **❔ 표준 모드(Standard mode)?**
>
> W3C등의 표준을 준수하는 렌더링 모드.





# META TAG

### 💡 메타 태그란

메타태그는 브라우저와 검색 엔진을 사용할 수 있도록 웹 문서의 정보를 포함하고 있다.

하이퍼텍스트(Hyper text) 생성언어 HTML 문서의 맨 위쪽에 위치하는 태그(tag)로 HEAD 태그 사이 또는 뒤에 있어도 되지만 반드시 BODY 태그 앞쪽에 위치해야 한다.

메타태그는 웹 페이지의 요약이므로 상당히 중요하다.



### 📌 메타태그 속성

**1️⃣ http_equiv = "항목명"** 

- 웹브라우저 서버에 명령을 내리는 속성
- name 속성을 대신해 사용할 수 있다.
- html 문서가 응답 헤더와 함께 웹 서버로부터 웹 브라우저에 전송되었을때만 의미를 갖는다.
- content 속성의 정보 /  값을 위한 HTTP header를 제공한다.
- html4에서는 문자 설정을 할 때 사용했다. 하지만 html5에서는 문자 설정 방법이 더 간단해졌다. (아래 예시)
  - html4.01 : < meta http-equiv = "content-type" content ="text/html; charcet = UTF-8">
    html5 : < meta charset = "UTF-8">

**2️⃣ name = "정보 이름"**

- 몇 개의 meta 정보의 이름을 정할 수 있으며 지정하지 않으면 http-equiv와 같은 기능을 한다.

**3️⃣ content = "정보값"** 

- meta 정보의 내용을 지정한다.
- name 속성과 http-equiv와 연관된 값을 준다.

**4️⃣ charset = "인코딩 언어"**

- 페이지의 문자 인코딩을 선언한다. 값으로는 utf-8이 들어간다.

- 인코딩 하는 방식을 선언하는 이유는 문자 깨짐 현상 등 심각한 오류가 나타날 수 있기에 이를 방지하기 위해 가장 기본이 되는 것이 바로 인코딩 선언이다.

  > 웹페이지에서 문자 깨짐 현상 발견 시 조치 방법
  >
  > - HTML meta 의 charset 미설정 또는 다른 인코딩 선언
  > - 서버 언어(java, asj, jsp, php 등) 가 사용된 경우 인코딩 선언의 오류
  > - 데이터 베이스의 인코딩 설정의 오류
  > - 텍스트 에디터의 문서 저장 시 기본 인코딩 charset 오류



### 🔍 메타태그 종류

1️⃣ **Keywords (검색엔진에 의해 검색되는 단어 지정)**

```html
<meta name="keyword" content="HTML, meta, tag, element, reference">
```

**2️⃣ Description (검색 결과에 표시되는 문자 지정)**

```html
<meta name="description" content="HTML meta tag page">
```

**3️⃣ robots (검색 로봇 제어)**

```html
<meta name="robots" content="noindex, nofollow" />
```

- noinedx : 검색 결과에 이 페이지를 표시하지 않는다(수집 대상에서 제외).
- nofollow: 이 페이지의 링크를 따라가지 않는다 ( 그 페이지를 포함해 링크가 걸린 곳을 수집 대상으로 하지 않음).
- noarchive: 검색 결과에 저장된 페이지 링크를 표시하지 않는다.
- All(기본값): 색인 생성이나 게재에 대한 제한이 없다. 기본값이므로 명시적으로 표시해도 효과는 없다. 'index, follow'를 지정한 것과 같음
- None: 'noindex, nofollow'와 같음
- index: 그 페이지를 수집 대상으로 한다.
- follow: 그 페이지를 포함해 링크가 걸린 곳을 수집 대상으로 한다.

```html
<meta name="robots" content="noindex, nofollow" />
: 구글에서 자신의 사이트가 색인화 되거나 링크 연결이 되지 않도록 설정할 수 있음.
	주로 사이트가 개발중일때, 자신의 사이트 중 일부 페이지가 구글에 노출되길 원치 않을 때 사용.
```

**4️⃣ Date (날짜 지정)**

```html
<meta name="Date" content="2023-06-05T07:45:37+09:00" />
```

content '+09:00'은 GMT(그리니치 표준시)로 부터의 시차

한국은 '+09:00', 미국 동부는 '-05:00', 등 나라, 지역에 따라 결정

**5️⃣ Content-Script-Type (웹페이지에 쓰인 언어 지정)**

```html
<meta http-equiv="Content-Script-Type" content="Text/javascript" />
```

6️⃣  **X-UA-Compatible (브라우저 호환성)**

```html
<meta http-equiv="X-UA-Compatible" content="IE-edge" />
```

7️⃣ **Title, Author, Publisher, Other Agent, Generator, Reply-To, Email, Filename, Location, Ditribution, Copyright, ...**

```html
<!-- 제목 -->
<meta http-equiv="Title" content="DOCTYPE" />
<!-- 제작자 -->
<meta http-equiv="Author" content="아현" />
<!-- 제작사 -->
<meta http-equiv="publisher" content="인파_" />
<!-- 웹 책임자 -->
<meta http-equiv="Other Agent" content="민정" />
<!-- 제작 도구 -->
<meta http-equiv="Generator" content="Visual Studio Code" />
<!-- 메일 주소 -->
<meta http-equiv="Reply-To" content="test@gmail.com" />
<meta http-equiv="Email" content ="test@gmail.com" />
<!-- 파일 이름 -->
<meta http-equiv="Filename" content="index.html" />
<!-- 위치 -->
<meta http-equiv="Location" content="위치" />
<meta name=”geo.region” content=”US-MN” />
<meta name=”geo.placename” content=”Minneapolis” />
<meta name=”geo.position” content=”44.980257;-93.270034″ />
<meta name=”ICBM” content=”44.980257, -93.270034″ />
<!-- 배포자 -->
<meta http-equiv="Distribution" content="name" />
<!-- 저작권 -->
<meta http-equiv="Copyright" content="아현" />
```

8️⃣ **Refresh (새로고침)**

```html
<!-- 60초 마다 새로고침한다! -->
<meta http-equiv="refresh" content="60" />

<!-- 입력한 주소로 5초 후 이동 -->
<meta http-equiv="refresh" content="5; url = 주소" />
```

9️⃣ **viewport (모바일 반응 페이지)**

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```







##  Reference

- [tistory, <!DOCTYPE html> 의 의미는 무엇일까?]()

- [tistory, [HTML 기초] DOCTYPE이란?](https://jwss.tistory.com/2)

- [tistory, 🏷️ 메타(meta) 태그 종류 & 사용법](https://inpa.tistory.com/entry/HTML-%F0%9F%93%9A-meta-%ED%83%9C%EA%B7%B8-%EC%A0%95%EB%A6%AC)
- [tistory, 메타(meta)태그 정리](https://webclub.tistory.com/354)