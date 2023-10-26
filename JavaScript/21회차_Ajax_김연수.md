[TOC]

## Ajax

AJAX(Asynchronous Javascript And Xml) 란, JavaScript의 라이브러리 중 하나이며 **비동기식 자바스크립트와 xml** 의 약자이다.

브라우저가 가지고있는 XMLHttpRequest 객체를 이용해서 전체 페이지를 새로 고치지 않고도 페이지의 일부 갱신을 위한 데이터를 로드하는 기법으로, 쉽게 말하면 **자바스크립트를 통해서 서버에 데이터를 비동기 방식으로 요청하는 것**이다.  
<br>

### 기존 방식과의 차이

이전의 웹 페이지는 html 태그로 시작해서 html 태그로 끝나는 **완전한 HTML을 서버로부터 전송 받아 웹 페이지 전체를 처음부터 다시 렌더링하는 방식으로 동작했다.**

Ajax의 등장으로 서버로부터 웹페이지의 변경에 필요한 데이터만 비동기 방식으로 전송 받아 웹페이지를 변경할 필요가 없는 부분까지 다시 렌더링하지 않고, **변경할 필요가 있는 부분만 한정적으로 렌더링하는 방식이 가능**해졌다.

> 1. 변경할 부분을 갱신하는데 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않는다.
> 2. 변경할 필요가 없는 부분은 다시 렌더링하지 않는다. 따라서 화면이 순간적으로 깜박이는 현상이 발생하지 않는다.
> 3. 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 이후 블로킹이 발생하지 않는다.  

<br>

### Ajax의 장점

- **속도 향상**

  페이지 전체를 갱신하지 않고 일부만 갱신할 수 있게 되기 때문에 속도가 향상된다.  

- **동적인 페이지 디자인 가능** 

  페이지 리로딩 없이 실시간으로 유저의 행동에 따라 변화하는 유기적인 페이지를 만들 수 있다.  

- **서버의 부담이 줄어든다.** 

  서버는 매번 같은 HTML페이지를 렌더링 할 필요가 없어지므로 부담이 줄어들게 된다.  

- **사용자의 편의성이 증가** 

  비동기 방식으로 데이터를 주고 받기 때문에 사용자는 응답을 기다리는 동안 다른 작업을 할 수 있다.

### Ajax의 단점

- **진행 파악이 힘들다.**  
  XMLHttpRequest는 진행 과정에 대해 사용자에게 어떤 정보나 단서를 주지 않는다. (요청이 완료되기 전에 사용자가 페이지를 떠나거나 다른 요청을 보낼 경우 오작동을 초래할 수 있다.)

- **연속적으로 데이터를 요청하면 서버에 부하가 증가**  
  연속적으로 데이터를 요청한다면 서버에 부하가 증가 할 수 있다.

- **히스토리 관리가 되지 않는다.**  
  페이지의 일부만 갱신하기 때문에 페이지 단위로 저장되는 히스토리가 남지 않는다. 때문에 사용자가 뒤로가기 버튼 등을 통해 히스토리를 되돌릴 수 없다.  
  <br>

### Ajax로 HTTP 요청 방법

Ajax로 HTTP 요청을 보내기 위해서는 XMLHttpRequest,  fetch(), 외부 라이브러리 방식이 있다.

#### 1. XMLHttpRequest(XHR) 방식

 Web API인 XMLHttpRequest는 자바스크립트에서 HTTP 요청을 위해 사용하는 객체로, 최근에는 잘 쓰지 않는 방법이다.

사용법은 XMLHttpRequest 객체를 생성하고, 특정 구문, 메서드를 활용하여 요청을 보낸다.

```js
const xhr = new XMLHttpRequest();		// XMLHttpRequest 객체 생성

xhr.open("GET","url주소");		// HTTP 요청 
xhr.setRequestHeader("content-type", "application/json");	// HTTP 요청 헤더 설정

// load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다.
xhr.onload = () => {
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
    // {userId: 1, id: 1, title: "delectus aut autem", completed: false}
  } else {
    console.error("Error", xhr.status, xhr.statusText);
  }
};

xhr.send()  // HTTP 요청을 보낸다
```



#### 2. fetch 방식

원래 자바스크립트는 XMLHttpRequest만을 사용하여 HTTP 요청을 해결하였지만, 최신 자바스크립트에서는 fetch() 메서드가 추가되었다.

fetch() 메서드는 브라우저에 내장되어 전역에서 사용 가능하고, MLHttpRequest 객체보다 사용법이 간단하면서 Promise를 지원하기 때문에 비동기 처리를 위한 콜백 패턴의 단점에서 자유롭다.

fetch 함수는 비교적 최근에 추가된 Web API로서 인터넷 익스플로러를 제외한 대부분의 모던 브라우저에서 제공한다.  

fetch 함수에는 HTTP 요청을 전송할 URL과 HTTP 요청 메서드, HTTP 요청 헤더, 페이로드(전송되는 데이터) 등을 설정한 객체를 전달한다.

```js
const promise = fetch(url [, options])
```

fetch 함수는 HTTP 응답을 나타내는 Response 객체를 래핑한 Promise 객체를 반환한다.

```js
fetch("url주소").then((결과) =>{
       	//결과 처리하는곳
    }).catch(() => {
      	//에러 발생시 코드
	})
}
```



#### 3. 외부 라이브러리 방식

\- jQuery를 사용한다. → $.ajax();

\- react나 vue 환경에서 axios 설치 사용  
<br>

> 🔍 **XMLHttpRequest와 fetch 메서드의 차이**
>
> 두 메서드 모두 브라우저에 내장된 WebAPI로 Ajax 통신이 가능하다.
>
> 하지만 XMLHttpRequest(XHR)는 프로미스를 사용하고 싶다면 직접 따로 구현해야되는 반면에, fetch 메서드는 Promise를 기반으로 이미 구성되어 있기 때문에 더 간편하게 사용 가능하다.
>
> 또한, XHR은 입력, 출력, 그리고 상태 모두를 하나의 객체로 관리해야하고, XHR의 상태 값은 이벤트를 통해 추적해야 했다.

<br>

## JSON

JavaScript Object Notation라는 의미의 약자로, Javascript에서 객체를 만들 때 사용하는 표현식을 의미한다.  
JSON 표현식은 사람과 기계 모두 이해하기 쉬우며 용량이 작아서, 최근에는 JSON이 XML을 대체해서 데이터 전송 등에 많이 사용한다.

JSON은 데이터 포맷일 뿐이며 어떠한 통신 방법도, 프로그래밍 문법도 아닌 단순히 데이터를 표시하는 표현 방법일 뿐이다. 



### json 형식

자바스크립트 객체와 마찬가지로 **key / value가 존재**할 수 있으며 **객체, 배열 등의 표기를 사용**할 수 있다.

JSON형식에서는 **null**, **number**, **string**, **array**, **object**, **boolean**을 사용할 수 있다.

```js
{
  "email": "hello@gmail.com",	  // name-value형식
  "hobby": ["puzzles","swimming"] // 리스트 형식
}
```

> 🔍 **XML vs JSON**
>
> XML은 데이터 값 양쪽으로 태그가 있지만, JSON은 태그로 표현하지 않고 중괄호({}) 같은 형식으로 하고, 값을 ','로 나열한다.



### json이 제공하는 정적 프로토타입 메서드

- **JSON.parse**( JSON 형식의 문자열 ) : JSON 형식의 텍스트를 자바스크립트 객체로 변환한다.
- **JSON.stringify**( JSON 형식의 문자열로 변환할 값 ) : 자바스크립트 객체를 JSON 텍스트로 변환한다.

<br>

---

**참고**

[[TCP SCHOOL] 2) Ajax 기초](https://www.tcpschool.com/ajax/ajax_intro_basic)

[AJAX(Asynchronous Javascript And XML)란?](https://velog.io/@jungnoeun/Ajax)

[JSON이란 무엇인가?](https://velog.io/@surim014/JSON%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)

[Ajax로 HTTP 요청을 보내기 위해서 사용할 수 있는 방법](https://velog.io/@godud2604/Ajax로-HTTP-요청을-보내기-위해서-사용할-수-있는-방법)

[[JavaScript] Ajax 사용법 알아보기](https://velog.io/@jji3205/서블릿....너는대체)

[[Javascript] 자바스크립트 HTTP 통신 (XMLHttpRequest vs fetch() vs axios ...)](https://joseph0926.tistory.com/31)
