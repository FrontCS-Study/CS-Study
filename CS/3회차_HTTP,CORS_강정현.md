## HTTP (HyperText Transfer Protocol)

HTML 문서와 같은 리소스를 가져올 수 있도록 하는, 클라이언트와 서버 간 통신을 위한 통신 규칙.

사용자가 웹 사이트를 방문 &rarr;  브라우저가 웹 서버에 HTTP 요청 전송 &rarr; 웹 서버가 HTTP 응답.

<br/>

### HTTP 프로토콜 특징

**비연결성 (Connectionless)**

클라이언트와 서버가 연결을 맺은 후에 응답을 마치면 기존의 연결을 끊는 특성.

기존의 연결을 유지하지 않기 때문에 클라이언트 연결을 위한 리소스를 줄일 수 있음.

클라이언트의 반복적인 요청에도 매번 연결/해제의 과정을 수행해야 하기 때문에 이에 따른 오버헤드가 발생할 수 있음.

**무상태성 (Stateless)**

비연결성으로 인해 이전에 어떤 상태를 가지고 있는지 알 수 없음.

이를 보완하기 위해 <u>쿠키, 세션, 토큰</u>  등 사용.

<br/>

### HTTP 버전

**HTTP/1.1**

**HTTP/2.0**

<br/>

### URL

**Uniform Resource Locators**

서버에 자원을 요청하기 위해 입력하는 주소. 

#### URL의 구조

![URL의 구조](https://joshua1988.github.io/images/posts/web/http/url-structure.png)

**프로토콜** http / https

**호스트 주소** 도메인 주소 / IP 주소

**포트 번호** 호스트 내 실행되고 있는 프로세스를 구분하기 위해 사용하는 번호

**경로**서버의 로직으로 가는 영역

**쿼리** 추가적인 데이터



<br/>

## HTTPS

**HyperText Transfer Protocol Secure**

HTTP 의 확장 버전 또는 더 안전한 버전. 브라우저와 서버가 데이터를 전송하기 전에 안전하고 암호화된 연결을 설정함.

<br/>

### HTTPS의 역할

1. 암호화를 통해 클라이언트가 정보를 서버와 안전하게 주고 받도록 함. 제3자 클라이언트가 서버에 보내는 정보들을 못 보게 함.
2. 접속한 사이트가 신뢰할만한 사이트인지 알려주는 역할.
3. 통신 내용의 악의적인 변경 방지.

<br/>

### HTTPS 의 작동 방식

HTT 는 암호화되지 않은 데이터를 전송하기 때문에, 브라우저에서 전송된 정보를 제3자가 가로채고 읽을 수 있음. 

이를 방지하기 위해 통신에 보안 계층을 추가한 HTTPS 로 확장. HTTPS 는 HTTP 요청 및 응답을 **SSL/TLS 기술**에 결합.

1. `https://` URL 형식으로 HTTPS 웹 사이트 방문, 브라우저는 서버의 <u>SSL 인증서를 요청</u>하여 사이트의 신뢰성 검증 시도.
2. 서버는 공개키 (Public Key) 가 포함된 SSL 인증서를 전송.
3. 브라우저에서 인증되면, 브라우저가 공개키를 사용하여 <u>랜덤 대칭 암호화 키 (Random Symmetric Encryption Key)</u>를 비롯한 URL, http 데이터들을 암호화하여 전송.
4. 서버는 비공개키를 이용하여 메시지를 해독. 그 다음 세션 키를 암호화하고 브라우저에 승인 메시지 전송.
5. 브라우저와 서버 모두 동일한 세션 키를 사용하여 메시지 교환.

<br/>

### 대칭키와 공개키(비대칭키)

#### 대칭키

암호화에 사용되는 키 와 복호화에 사용되는 키 가 동일한 암호화 기법.

암호화된 메시지를 상대가 복호화하기 위해선 대칭키의 공유가 있어야 하는데, 이 과정에서 대칭키가 유출되면 누구나 복호화를 할 수 있으므로, 암호화와 복호화에 필요한 키의 공유가 어려움. 



<img width="500" alt="스크린샷 2023-07-11 오전 4 10 10" src="https://github.com/FrontCS-Study/CS-Study/assets/85155789/f6e2e378-8da9-4257-9983-e6fca1073d24">



#### 공개키(비대칭키)

대칭키의 단점을 보완한 암호화 방식.

두 개의 키를 쌍으로 가지면서, A 키를 사용해 암호화를 하면 B 키로 복호화 할 수 있고, B 키로 암호화를 하면 A 키로 복호화 할 수 있음.

A 키를 통해 암호화한 내용은 A 키를 이용해 복호화하는 것은 불가능하고, A 키와 쌍을 이루는 B 키를 이용해야만 복호화가 가능하다.

<img width="500" alt="스크린샷 2023-07-11 오전 4 19 19" src="https://github.com/FrontCS-Study/CS-Study/assets/85155789/3a038e04-f042-4669-b9dc-a0913bba038f">

두 개의 키 중 하나를 비공개키(Private Key, 개인키/비밀키) 로 하여 서버가 가지고, 나머지를 공개키로 지정하여 대중에게 공개한다.

클라이언트는 공개키를 사용하여 비밀번호 같은 정보를 암호화하여 서버로 보내면, 서버는 자신이 보관하고 있던 비공개키를 이용하여 내용을 복호화 한다.

공개키가 유출된다고 해도 공개키로는 암호화만 가능하고 복호화는 서버가 가지고 있는 비공개키로만 가능하기 때문에 정보가 안전하다.

<img width="800" alt="스크린샷 2023-07-11 오전 4 31 55" src="https://github.com/FrontCS-Study/CS-Study/assets/85155789/a2802367-d988-4e5f-83d8-82b727766897">



---

<br/>

## CORS

### 동일 출처 정책 (SOP)

**Same-Origin Policy**

어떤 **출처**에서 불러온 문서나 스크립트가 다른 **출처**에서 가져온 리소스와 상호작용하는 것을 제한하는 보안 방식.

잠재적으로 해로울 수 있는 문서를 분리함으로써 공격 받을 수 있는 경로를 줄인다.

<br/>

#### 출처의 정의

두 URL 의 프로토콜, 호스트, 포트가 모두 같아야 동일한 출처임.

**http://store.company.com/dir/page.html 와 출처 비교**

| URL                                             | 결과 | 이유                               |
| ----------------------------------------------- | ---- | ---------------------------------- |
| http://store.company.com/dir2/other.html        | 성공 | 경로만 다름                        |
| http://store.company.com/dir/inner/another.html | 성공 | 경로만 다름                        |
| https://store.company.com/secure.html           | 실패 | 프로토콜 다름                      |
| http://store.company.com:81/dir/etc.html        | 실패 | 포트 다름 (http:// 는 80이 기본값) |
| http://news.company.com/dir/other.html          | 실패 | 호스트 다름                        |

<br/>

### 교차 출처 리소스 공유 (CORS)

**Cross-Origin Resource Sharing**

추가 HTTP 헤더를 사용하여, 한 출처에서 실행 중인 웹이 다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제.

리소스가 자신의 출처 (프로토콜 + 호스트 + 포트) 와 다를 때 교차 출처 HTTP  요청을 실행한다.

> https://domain-a.com 의 JavaScript 코드가 https://domain-b.com/data.json 을 요청하는 경우

Same-Origin Policy 의 문제점을 해결하기 위해 CORS 를 이용하여, <u>출처가 다른 도메인에서의 접근 요청이라도 서버 단에서 접근 권한을 허용</u>할 수 있다. 

<br/>

#### CORS 동작 방식

**기본 흐름**

1. 리소스 요청 시 HTTP 요청 헤더의 Origin 필드에 요청을 보내는 출처를 담아 보낸다.
2. 서버가 응답할 때, 응답 헤더의 `Access-Control-Allow-Origin` 이라는 값에 "이 리소스를 접근하는 것이 허용된 출처" 를 내려준다.
3. 응답을 받은 브라우저는 보냈던 요청의 Origin 과 서버가 보내준 응답의 `Access-Control-Allow-Origin` 을 비교, 유효한 응답인지를 결정한다.



CORS 접근 제어 시나리오: Preflight Request, Simple Request, Credentialed Request

<br/>

#### CORS 해결 방법

CORS 정책 위반으로 인한 문제가 발생했을 경우 해결할 수 있는 방법

<br/>

**Access-Control-Allow-Origin 세팅하기**

정석대로 `Access-Control-Allow-Origin ` 헤더에 알맞은 값을 세팅.

**Webpack Dev Server로 리버스 프록싱하기**

CORS 를 가장 많이 마주치는 환경은 로컬에서 프론트엔드 개발하는 경우인데, 이 때 `Access-Control-Allow-Origin ` 헤더에 ` localhost:3000` 같은 범용적인 출처를 넣는 경우는 드물다.

프론트엔드 개발자는 대부분 웹팩과 `webpack-dev-server` 를 사용하여 개발 환경을 구축하는데, 이 라이브러리가 제공하는 프록시 기능을 사용한다.

```JavaScript
module.exports = {
  devServer: {
    proxy: {
      '/api': {
        target: 'https://api.evan.com',
        changeOrigin: true,
        pathRewrite: { '^/api': '' },
      },
    }
  }
}
```

로컬 환경에서 /api 로 시작하는 URL 로 보내는 요청에 대해 브라우저는 `localhost:3000/api` 로 요청을 보낸 것으로 알고 있지만, 사실 웹팩이 `https://api.evan.com` 으로 요청을 프록싱함. 즉 프록싱을 통한 CORS 우회. 

단, 로컬 개발 환경에서만 가능.





---

**Reference**

[HTTP와 HTTPS의 차이점은 무엇인가요?](https://aws.amazon.com/ko/compare/the-difference-between-https-and-http/)

[HTTPS와 SSL인증서 & 대칭키, 공개키(비대칭키)](https://velog.io/@dong5854/HTTPS%EC%99%80-SSL%EC%9D%B8%EC%A6%9D%EC%84%9C-%EB%8C%80%EC%B9%AD%ED%82%A4-%EA%B3%B5%EA%B0%9C%ED%82%A4%EB%B9%84%EB%8C%80%EC%B9%AD%ED%82%A4)

[URL 구조 이해하기](https://www.grabbing.me/URL-018cdd1bb4b541fab6246569244fcf93)

[교차 출처 리소스 공유 (CORS)](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)

[동일 출처 정책](https://developer.mozilla.org/ko/docs/Web/Security/Same-origin_policy)

[프런트엔드 개발자가 알아야하는 HTTP 프로토콜 Part 1](https://joshua1988.github.io/web-development/http-part1/)

[CORS는 왜 이렇게 우리를 힘들게 하는걸까?](https://evan-moon.github.io/2020/05/21/about-cors/)
