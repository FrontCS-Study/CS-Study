# Ajax

## Ajax (Asynchronous JavaScript and XML)

빠르게 동작하는 동적인 웹 사이트를 만들기 위한 개발 기법의 하나.
웹 페이지 전체를 다시 로딩하지 않고도, 웹 페이지의 일부분만 갱신할 수 있다.
즉 Ajax를 이용하면 1) **백그라운드 영역에서 서버와 통신하여**, 2) **그 결과를 웹 페이지의 일부분에만 표시**할 수 있다.

## Ajax 동작 원리

<p align="center">
  <img src="https://www.tcpschool.com/lectures/img_ajax_ajax_application.png" align="center" width="49%">
  <img src="https://www.tcpschool.com/lectures/img_ajax_other_application.png" align="center" width="50%">
  <figcaption align="center"><a herf="https://www.tcpschool.com/ajax/ajax_intro_works">Ajax를 이용한 웹 응용 프로그램과 기존의 웹 응용 프로그램의 동작 원리</a></figcaption>
</p>

1. 사용자에 의한 요청 이벤트가 발생한다.
2. 요청 이벤트가 발생하면, 이벤트 핸들러에 의해 자바스크립트가 호출된다.
3. 자바스크립트는 XMLHttpRequest 객체를 사용하여 서버로 요청을 보낸다.
   - 이 때, 웹 브라우저는 요청을 보내고 나서 서버의 응답을 기다릴 필요 없이 다른 작업을 처리할 수 있다. (비동기) 
4. 서버는 전달받은 XMLHttpRequest 객체를 가지고 Ajax 요청을 처리한다.
5. 서버는 처리한 결과를 HTML, XML 또는 JSON 형태의 데이터로 웹 브라우저에 전달한다.
   - 이 때 전달되는 응답은 새로운 페이지를 전부 보내는 것이 아니라 필요한 데이터만을 전달한다.
6. 서버로부터 전달받은 데이터를 가지고 웹 페이지의 일부분만을 갱신하는 자바스크립트를 호출한다.
7. 결과적으로 웹 페이지의 일부분만이 다시 로딩되어 표시된다.

## Ajax 서버 통신

### XMLHttpRequest

Ajax에서 XMLHttpRequest 객체는 웹 브라우저가 서버와 데이터를 교환할 때 사용한다.

#### XMLHttpRequest 객체 생성

1. XMLHttpRequest 객체를 이용한 방법
   - 익스플로러 7과 그 이상의 버전, 크롬, 파이어폭스, 사파리, 오페라에서는 XMLHttpRequest 객체를 사용한다.

```javascript
var 변수명 = new XMLHttpRequest();
```

1. ActiveXObject 객체를 이용한 방법
   - 익스플로러 5, 6 버전에서는 ActiveXObject 객체를 사용한다.

```javascript
var 변수명 = new ActiveXObject("Microsoft.XMLHTTP");
```

### 서버 요청

Ajax는 XMLHttpRequest 객체를 사용하여 서버와 데이터를 교환한다.
서버에 요청을 보내기 위해선 XMLHttpRequest 인스턴스를 생성하고, `open()` 메소드와 `send()` 메소드를 사용한다.

```javascript
open(전달방식, URL주소, 동기여부);
```

- 전달 방식 : GET, POST 
- URL 주소 : 요청을 처리할 서버의 파일 주소.
- 동기 여부 : 동기식 / 비동기식

```javascript
send(); //GET 방식
send(문자열); // POST 방식
```

#### GET 방식으로 요청

- 주소에 데이터(data)를 추가하여 전달하는 방식. 
- 브라우저에 의해 캐시되어(cached) 저장됨.
- GET 방식은 **쿼리 문자열(query string)에 포함되어 전송되므로, 길이의 제한이 있고 보안상 취약점이 존재함.

```javascript
httpRequest.open("GET", "/examples/media/request_ajax.php?city=Seoul&zipcode=06141", true);
httpRequest.send();
```

#### POST 방식으로 요청

- 서버로 전송하고자 하는 데이터를 HTTP 헤더에 포함하여 전송함.
- `setRequestHeader()` 메소드를 이용해 먼저 헤더를 작성한 후에, `send()` 메소드로 데이터를 전송함.

```javascript
httpRequest.open("POST", "/examples/media/request_ajax.php", true);
httpRequest.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
httpRequest.send("city=Seoul&zipcode=06141")
```

### 서버 응답

Ajax에서 서버로부터의 응답을 하기 위해 사용하는 `XMLHttpRequest` 객체의 프로퍼티:

- readyState 프로퍼티
- status 프로퍼티
- onreadystatechange 프로퍼티

#### readyState 프로퍼티

XMLHttpRequest 객체의 현재 상태를 나타냄.

1. UNSENT (숫자 0) : XMLHttpRequest 객체가 생성됨.
2. OPENED (숫자 1) : `open()` 메소드가 성공적으로 실행됨.
3. HEADERS_RECEIVED (숫자 2) : 모든 요청에 대한 응답이 도착함.
4. LOADING (숫자 3) : 요청한 데이터를 처리 중임.
5. DONE (숫자 4) : 요청한 데이터의 처리가 완료되어 응답할 준비가 완료됨.

#### status 프로퍼티

서버의 문서 상태를 나타냄.

- 200 : 서버에 문서가 존재함.
- 404 : 서버에 문서가 존재하지 않음.

#### onreadystatechange 프로퍼티

XMLHttpRequest 객체의 **readyState 프로퍼티 값이 변할 때마다** 자동으로 호출되는 함수를 설정함.

```javascript
switch (httpRequest.readyState) {
    case XMLHttpRequest.UNSET:
        currentState += "현재 XMLHttpRequest 객체의 상태는 UNSET 입니다.<br>";
        break;
    case XMLHttpRequest.OPENED:
        currentState += "현재 XMLHttpRequest 객체의 상태는 OPENED 입니다.<br>";
        break;
    case XMLHttpRequest.HEADERS_RECIEVED:
        currentState += "현재 XMLHttpRequest 객체의 상태는 HEADERS_RECEIVED 입니다.<br>";
        break;
    case XMLHttpRequest.LOADING:
        currentState += "현재 XMLHttpRequest 객체의 상태는 LOADING 입니다.<br>";
        break;
    case XMLHttpRequest.DONE:
        currentState += "현재 XMLHttpRequest 객체의 상태는 DONE 입니다.<br>";
        break;
}

document.getElementById("status").innerHTML = currentState;
if (httpRequest.readyState == XMLHttpRequest.DONE && httpRequest.status == 200 ) {
    document.getElementById("text").innerHTML = httpRequest.responseText;
}
```

## Ajax의 한계

1. Ajax는 클라이언트가 서버에 데이터를 요청하는 클라이언트 풀링 방식을 사용하므로, 서버 푸시 방식의 실시간 서비스는 만들 수 없다.
2. Ajax로는 바이너리 데이터를 보내거나 받을 수 없다.
3. Ajax 스크립트가 포함된 서버가 아닌 다른 서버로 Ajax 요청을 보낼 수는 없다.
4. 클라이언트 PC로 Ajax 요청을 보낼 수는 없다.

# Fetch API

기존의 XHR(XMLHttpRequest) 객체를 이용한 Ajax의 가독성 문제를 해결하기 위해 등장하였으며, ES6에서 표준으로 사용함.
반환값으로 **Promise** 를 가진다. 

```javascript
fetch(resource, init)
	.then(callback)
	.catch(callback)
```

- resource : 요청 주소, URL, 경로
- init (optional) : 설정 객체

```javascript
const init = {
	method: "POST",
	body: JSON.stringify(data),
	headers: {
		"Content-Type" : "application/json",
	},
	credientials: "same-origin"
}
```

```javascript
fetch('/src/data.json')
	.then(response => { // 첫번째 then
		if (response.status === 200) {
			return response.json();
		} else {
			console.log(response.statsText);
		}
	})
	.then(jsonData => { // 두번째 then
		console.log(jsonData);
	})
	.catch(err => {
		console.log(err);
	})
```

- 첫 번재 then에서 `response.json()`을 바로 출력하지 않고, 다음 then으로 리턴하여 넘겨준 것은 `response.json()`은 실제 값이 아닌 Promise를 가지고 있기 때문이다.
  - Response 객체의 body 값을 추출해내기 위해서는 타입에 따라 아래와 같은 메소드를 사용해야 함.
    - arrayBuffer()
    - blob()
    - json()
    - text()
    - formData()
  - 위 메소드들은 모두 Promise를 반환하고, 이 Promise가 resolve 되어 다음 then에서는 실제 값을 다룰 수 있게 된다.

# JSON

JavaScript Object Notation
JavaScript 객체 문법으로 구조화된 데이터를 표현하기 위한 문자 기반의 표준 포맷.
일반적으로 웹 어플리케이션에서 데이터를 전송할 때 사용한다. (서버 - 클라이언트 데이터 전송)

## JSON 메소드

- JSON.stringify : 객체를 JSON으로 인코딩함.
- JSON.parse : JSON으로 인코딩된 객체를 다시 객체로 디코딩함.

### JSON.stringify

```javascript
let json = JSON.stringify(value[, replace, space]);
```

- value : 인코딩 하려는 값
- replacer : JSON으로 인코딩 하길 원하는 프로퍼티가 담긴 배열. 또는 매핑 함수 (`function(key, value)`)
- space : 서식 변경 목적으로 사용할 공백 문자 수

변환된 문자열은 *JSON으로 인코딩된(JSON-encoded), 직렬화 처리된(serialized), 문자열로 변환된(stringified), 결집된(marshalled) 객체*라고 부른다.

- 문자열은 큰따옴표로 감싼다. JSON에선 작은따옴표나 백틱을 사용할 수 없다.
- 객체 프로퍼티 이름은 큰따옴표로 감싼다.

```javascript
let student = {
	name: 'John',
	age: 30,
	isAdmin: false,
	course: ['html', 'css', 'js'],
	wife: null
};
let json = JSON.stringify(student);
console.log(typeof json); // string
console.log(json);
/* 결과:
{"name":"John","age":30,"isAdmin":false,"courses":["html","css","js"],"wife":null}
*/
```

JSON은 데이터 교환을 목적으로 만들어진, 언어에 종속되지 않는 포맷이기 때문에 자바스크립트 특유의 객체 프로퍼티는 `JSON.stringify`가 처리할 수 없다.
`JSON.stringify` 호출 시 무시되는 프로퍼티:

- 함수 프로퍼티 (메소드)
- 심볼형 프로퍼티 (키가 심볼인 프로퍼티)
- 값이 `undefined`인 프로퍼티

```javascript
let user = {
	sayHi() { // 무시
		alert("Hello"); 
	},
	[Symbol("id")]: 123, // 무시
	something: undefined // 무시
}
console.log(JSON.stringify(user)); // {} (빈 객체 출력)
```

### JSON.parse

```javascript
let value = JSON.parse(str, [reviver]);
```

- str : JSON 형식의 문자열
- reviver : 모든 (key, value) 쌍을 대상으로 호출되는 `function(key, value)` 형태의 함수로 값을 변경함.

```javascript
let numbers = "[0, 1, 2, 3]"; // 문자열로 변환된 배열
numbers = JSON.parse(numbers);
console.log(numbers[1]) // 1

let userData = '{ "name": "John", "age":35 }'
let user = JSON.parse(userData);
console.log(user.age); // 35
```

---

**Reference**

[Ajax 동작 원리](https://www.tcpschool.com/ajax/ajax_intro_works)

[AJAX, XMLHttpRequest와 Fetch 살펴보기](https://junhobaik.github.io/ajax-xhr-fetch/)

[정말 멋진 Fetch API!](http://hacks.mozilla.or.kr/2015/05/this-api-is-so-fetching/)

[📥 XMLHttpRequest&fetch&axios](https://velog.io/@kcj_dev96/fetch)

[JSON으로 작업하기](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Objects/JSON)

[JSON과 메서드](https://ko.javascript.info/json)
