## DOCTYPE (Document Type)
HTML이 어떤 버전으로 작성되었는지 미리 선언하여, 웹 브라우저가 내용을 올바르게 표시할 수 있게 한다.

## DOCTYPE 정의 (DTD, Document Type Definition)
DOCTYPE 은 HTML 문서에서 `<html>` 태그를 정의하기 전에 가장 먼저 선언된다.
DOCTYPE 을 선언한 경우 표준 모드로 작동, 선언하지 않은 경우 호환 모드(Quricks Mode)로 작동하여 작성한 코드가 의도대로 동작하지 않을 수 있다.

> **웹 브라우저의 레이아웃 엔진**
> 1. 호환 모드 (Quricks Mode) : 기존 방식(하위 버전)으로 제작된 웹사이트를 표현하기 위해, 네비게이터 4(Navigator 4)와 인터넷 익스플로러 5의 비표준 동작들을 `에뮬레이션` 한다.
> 2. 표준 모드 (Full Standards Mode; 표준 모드) : HTML과 CSS에 의해 웹 페이지가 표시된다.
> 3. 유사 표준 모드 (Almost Standards Mode) : 소수의 호환 모드 요소만 지원한다.

HTML에는 다양한 버전이 있으며, 각각의 문서별로 DOCTYPE을 선언하는 방식이 다르다.

```html
<!-- HTML 5-->
<!DOCTYPE html>

<!-- HTML 4.01 -->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

<!-- XTML 1.1 -->
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">
```

[그 외 버전별 선언](https://aboooks.tistory.com/40)

<br/>

## 메타 태그
`<meta>` 는 **웹 페이지에 대한 정보를 제공**하는 기능을 하며, 이러한 정보를 '메타 데이터'라고 한다. 
메타 데이터는 웹 페이지에 나타나지 않고, 검색 엔진이나 웹 크롤러를 통해 수집된다.

## 메타 태그의 종류
### 1. <title /> 
웹 페이지의 제목을 나타내는 요소. 크게 세 가지 상황에서 볼 수 있다:
1. 검색 엔진의 결과로 헤드라인에 적용됨.
2. SNS 공유 시 제목으로 적용됨.
3. 웹 브라우저 상단 탭의 제목으로 적용됨.
```html
<title>title example</title>
```

### 2. \<description />
웹 페이지의 요약된 정보를 나타냄. 
SEO를 위해 같은 사이트더라도 페이지별로 다른 제목과 설명을 추가할 것을 권장함.

```html
<meta name="description" content="description example">
```

### 3. 그 외
- keywords: 검색 엔진에 의해 검색되는 단어 지정
- robots: 검색 로봇 제어
- Date: 제작일
- author: 페이지 작성 제작자 이름
- location: 위치
- copyright: 저작권
- charset: 문자 인코딩 방식
- Generator: 제작 도구
- Distribution: 배포자
- Refresh: 새로고침 주기 및 전환

## 메타 태그 설정

```html
<!-- index.html -->
<head>
	<title>title</title>
	<meta
		name="description"
		content="decription contents"
    > <!-- 닫는 태그는 존재하지 않는다. -->
</head>
```
---
**Reference**

[DOCTYPE이란 도대체 무엇인가?](https://m.blog.naver.com/redtaeung/221929431362)

[HTML DOCTYPE 선언](https://velog.io/@sweetpumpkin/HTML-DOCTYPE-%EC%84%A0%EC%96%B8)

[검색 잘 되는 사이트의 필수조건, 메타(meta) 태그란?](https://yozm.wishket.com/magazine/detail/816/)

[메타 태그를 통한 검색엔진 최적화 (SEO)](https://www.daleseo.com/html-meta-tags-for-seo/)
