# AJAX



### AJAX(Asynchronous Javascript And XML)

AJAX란, JavaScript의 라이브러리 중 하나로, **JavaScript를 사용해 클라이언트가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신해 웹페이지를 동적으로 갱신**하는 프로그래밍 방식을 말한다.

일반적으로 HTTP 프로토콜은 단방향 통신이다. 그렇기 때문에 클라이언트에서 요청을 보내고, 서버쪽에서 응답을 받으면 연결이 끊어지낟. 그래서 화면의 내용을 갱신하기 위해서는 다시 요청을 보내고 응답을 받으며 페이지 전체를 갱신해야 한다. 다만, 이러한 경우에는 엄청난 자원낭비오 시간낭비를 겪게 된다.

Ajax는 Http 페이지 전체가 아닌 일부만 갱신 가능하도록, 브라우저에서 제공하는 Web API이며 비동기 통신인  **XMLHttpRequest 객체를 통해 서버에 요청**한다. 이 경우, **JSON이나 XML형태로 필요한 데이터만 받아 갱신**해 전체 페이지를 새로 고치지 않고도 **페이지의 일부만을 위한 데이터를 로드**할 수 있다. 자원과 시간 절약이 가능하다는 것이다.

Ajax를 사용하면 다음과 같은 이점이 있다.

1️⃣ 변경할 부분을 갱신하는데 필요한 데이터만 서버로부터 전송받으므로 불필요한 데이터 통신이 발생하지 않는다.

2️⃣ 변경할 필요가 없는 부분은 다시 렌더링 되지 않고, 따라서 화면의 깜빡임이 없다.

3️⃣ 클라이언트와 서버가 비동기 통신을 하므로 서버에게 요청을 보낸 후 블로킹(작업 중단)이 되지 않는다. 



### AJAX의 통신과정

![기존 웹 응용 프로그램의 동작 원리](https://www.tcpschool.com/lectures/img_ajax_other_application.png)

![ajax를 이용한 웹 응용 프로그램의 동작 원리](https://www.tcpschool.com/lectures/img_ajax_ajax_application.png)

1. 사용자에 의한 요청 이벤트가 발생한다
2. 요청 이벤트가 발생하면 이벤트 핸들러에 의해 자바스크립트가 호출된다.
3. 자바스크립트는 XMLHttpRequest 객체를 사용해 서버로 요청을 보낸다.
4. 서버는 전달받은 XMLHttpRequest 객체를 가지고 요청을 처리한다.
5. 서버는 처리한 결과를 HTML, XML 또는 JSON 형태의 응답 데이터를 생성하고, 웹 브라우저에 전달한다.
6. 이때 전달되는 응답은 새로운 페이지를 전부 보내는 것이 아니라 필요한 데이터만을 전달한다.
7. 서버로부터 전달받은 데이터를 가지고 웹 페이지의 일부분만을 갱신하는 자바스크립트를 호출한다. 결과적으로 웹 페이지의 일부분만이 다시 로딩되어 표시된다.



### XMLHttpRequest 


XMLHttpRequest 객체는 웹 브라우저가 서버와 데이터를 교환할 때 사용된다. 웹 브라우저가 백그라운드에서 계속해서 서버와 통신할 수 있는 것은 바로 이 객체를 사용하기 때문이다.

```js
// 1. XMLHttpRequest객체 생성
var httpRequest = new XMLHttpRequest(); 

// 2. onreadystatechange 등록
httpRequest.onreadystatechange = function() {
	// XMLXttpRequest 객체의 현재 상태와 서버 상의 문서 존재 유무를 확인
    if (httpRequest.readyState == XMLHttpRequest.DONE && httpRequest.status == 200 ) {
    	console.log(httpRequest.responseText); // 서버에 요청하여 응답으로 받은 데이터를 문자열로 반환
    }
};

// 3. GET 방식으로 요청을 보내면서 데이터를 동시에 전달함
httpRequest.open("GET", "서버URL", true);
httpRequest.send();
```



---

#### ✅ XMLHttpRequest 요청 보내기

1️⃣ XMLHttpRequest 객체 생성

```js
var httpRequest = new XMLHttpRequest();
```

2️⃣  `onreadystatechange`등록

> `onreadystatechange`로 서버로부터 응답이 오게 되어 XMLHttpRequest 객체의 값이 변하게 되면 이를 감지해 자동으로 호출되는 함수를 설정한다. 함수를 등록하게 되면 서버에 요청한 데이터가 존재하고, 서버로부터 응답이 도착하는 순간을 특정할 수 있게 된다.

- ```js
  // 2. onreadystatechange 등록
  httpRequest.onreadystatechange = function() {
  
  };
  ```

3️⃣ 서버로 보낼 Ajax 요청의 형식을 설정

> `httpRequest.open(전달방식, URL주소, 동기여부)` 메소드를 등록한다.

- 전달방식은 요청을 전달할 방식으로  GET 방식과 POST 방식 중 하나를 선택할 수 있다.

- URL 주소는 요청을 처리할 서버의 파일 주소를 전달한다.

- 동기 여부는 요청을 동기식으로 전달할 지 비동기식으로 전달할 지를 알려주는 것. (true: 비동기 / false: 동기)

  ```js
  httpRequest.open("GET", "서버URL", true);
  ```

4️⃣ 작성된 Ajax 요청을 서버로 전달

> `httpRequest.send()` 메서드를 통해 서버로 객체를 전달한다. 이때 `send()` 메서드 매개변수를 쓰냐 안쓰냐에 따라 GET / POST 방식으로 다르게 보내게 된다.

- `send()` : GET 방식

- `send(문자열)` : POST 방식

  ```js
  httpRequest.send();
  ```

5️⃣ POST 방식 서버 요청

> Ajax에서는 서버에 POST 방식의 요청을 보내기 위해서 다음과 같이 작성한다. 이때 서버로 전송하고자 하는 데이터는 HTTP 헤더에 포함되어 전송된다. 따라서 `setRequestHeader()` 메소드를 이용하여 먼저 헤더를 작성한 후에, `send()` 메소드로 데이터를 전송하면 된다.

- ```js
  var httpRequest = new XMLHttpRequest(); // 객체 생성
  
  httpRequest.onreadystatechange = function() {
      if (httpRequest.readyState == XMLHttpRequest.DONE && httpRequest.status == 200 ) {
          console.log(httpRequest.responseText);
      }
  };
  
  // POST 방식의 요청은 데이터를 Http 헤더에 포함시켜 전송함.
  httpRequest.open("POST", "/examples/media/request_ajax.php", true);
  httpRequest.setRequestHeader("Content-Type", "application/x-www-form-urlencoded"); // 보낼 데이터 헤더를 지정
  httpRequest.send("city=Seoul&zipcode=06141"); // 보낼 데이터 지정
  ```



### Fetch

자바스크립트의 Ajax 요청방식에는 정통적으로 `XMLHttpRequest()` 객체를 생성하여 요청하는 방법이 있다. 초창기의 비동기 서버 요청방식이며 성능에 문제는 없지만, 문법이 난해하고 가독성도 좋지 않다. 

이벤트 기반인 XMLHttpRequest과는 달리, fetch API는 **Promise 기반**으로 구성되어 있어 **비동기 처리 프로그래밍 방식**에 잘 맞는 형태이다. 그래서 then이나 catch와 같은 체이닝으로 작성할 수 있다는 장점이 있다. 또한 fetch API는 **JS 기본 기능**이기 때문에, JQuery와 같이 CDN과 같은 다른 작업을 하지 않아도 바로 사용할 수 있다.

```js
fetch('ajax_intro_data.txt')
    .then( response => response.text() )
    .then( text => { document.getElementById("#t").innerHTML = text; } )

/* 'fetch('서버주소')' 는 웹 브라우저에게 '이 서버주소로 요청해줘' 라는 의미이고, 
뒤에 .then이 붙으면 '요청 끝나고나서 이 할일을 해줘!' 라는 것이다. */
```



#### ✅ fetch 문법 사용법

```js
fetch("https://jsonplaceholder.typicode.com/posts", option)
   .then(res => res.text())
   .then(text => console.log(text));
```

1️⃣ fetch 에는 기본적으로 첫 번째 인자에 요청할 url이 들어간다.

2️⃣ 기본적으로 http 메소드 중 GET으로 동작한다.

3️⃣ fetch를 통해 Ajax를 호출 시 해당 주소에 요청을 보낸 다음, 응답 객체(Promise object Response)를 받는다.

4️⃣ 그러면 첫 번째 `then`에서 그 응답을 받게 되고, `res.text()` 메서드로 파싱한 text 값을 리턴한다.

5️⃣ 그러면 그 다음 `then`에서 리턴받은 text 값을 받고, 원하는 처리를 할 수 있게 된다.



##### ✅ Fetch - async / await 문법

fetch의 리턴값 response는 Promise객체이다. 당연히 `async`/`await` 문법으로 보다 가독성 높게 코딩할 수 있다.

```js
// fetch 사용 시
fetch("https://jsonplaceholder.typicode.com/posts", option)
.then(res => res.text())
.then(text => console.log(text));
```

```js
// async/await 사용 시
(async () => {
  let res = await fetch("https://jsonplaceholder.typicode.com/posts", option);
  let text = await res.text();
  console.log(text);
})() //await은 async함수내에서만 쓸수 있으니, 익명 async 바로 실행함수를 통해 활용합니다.
```



# JSON

### JSON(JavaScript Object Notation) 이란?

JSON은 자바스크립트의 객체 표현식과 유사한 방식으로 데이터를 주고 받는 방법이다. 

JSON은 객체를 정의하지 않는다. 즉, 자바스크립트 **객체가 아니라 객체 표현식으로 데이터를 표현**한다. 따라서 다른 도메인에서도 요청을 보낼 수 있다.

```js
var test = {
    "name" : "coco",
    "age" : 26,
    "address : "서울시"
}
```

위와 같은 객체 형식의 표현식으로 쓸 수 있다. test라는 객체는 **key : value 형식의 표현식**이다.

key = "name " : value = "coco" 으로 데이터가 들어간다.

이런 JSON 형식으로 데이터를 전송하면 Bean에서 key의 명칭과 같은 Bean안에 있는 변수를 자동으로 매핑한다.






## Reference

[🌐 XMLHttpRequest 으로 AJAX 요청하기](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-AJAX-%EC%84%9C%EB%B2%84-%EC%9A%94%EC%B2%AD-%EB%B0%8F-%EC%9D%91%EB%8B%B5-XMLHttpRequest-%EB%B0%A9%EC%8B%9D)

[🌐 Fetch API 으로 AJAX 요청하기](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-AJAX-%EC%84%9C%EB%B2%84-%EC%9A%94%EC%B2%AD-%EB%B0%8F-%EC%9D%91%EB%8B%B5-fetch-api-%EB%B0%A9%EC%8B%9D)

[AJAX란 무섯인가 ?](https://99geo.tistory.com/65)

[Group Study, 모던 자바스크립트 Deep Dive - 43 Ajax](https://daniel-park.tistory.com/97?category=776410)

[Ajax 동작 원리](https://www.tcpschool.com/ajax/ajax_intro_works)

[JSON & AJAX 간단히 알아보기](https://dev-coco.tistory.com/92)