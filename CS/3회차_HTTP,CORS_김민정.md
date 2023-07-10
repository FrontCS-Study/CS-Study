## URL

### 정의

Uniform Resource Locators

서버에 자원을 요청하기 위해 입력하는 영문 주소

숫자보다 기억하기 쉽다

URL로 되어있는 HTTP 요청을 DNS(Domain Name System)를 통해 host에 해당하는 실제 IP 주소로 변환하여 서버에 요청을 보낸다

### 활용

HTTP 요청과 응답에 활용된다

- HTTP Request

  `URL + 요청 메서드`

- HTTP Response

  `상태 코드 + 응답 Body`



# HTTP

## 정의

Hypertext Transfer Protocol

World Wide Web을 위한 데이터 통신의 기초이자 웹 사이트를 이용하는 데 쓰는 프로토콜

- 웹을 기준으로 브라우저와 서버 간에 데이터를 주고 받기 위한 방식



## 특징

### 비상태성

클라이언트와 서버의 연결이 끝나면 상태 정보가 유지되지 않는다

데이터 요청이 서로 독립적이다 (== 이전 데이터 요청과 다음 데이터 요청이 서로 관련이 없다)

---> 다수의 요청 처리 및 서버의 부하를 줄일 수 있는 성능 상의 이점

TCP/IP 통신 위에서 동작

기본 포트는 80번



## HTTP/1.0

한 연결당 하나의 요청 처리 --> RTT 증가

서버로부터 파일을 가져올 때마다 TCP의 3-way 핸드셰이크를 계속해야 되기 때문

> RTT: 패킷이 목적지에 도달하고 나서 다시 출발지로 돌아오기까지 걸리는 시간 / 패킷 왕복 시간

### RTT 증가 해결

1. 이미지 스플리팅
2. 코드 압축
3. 이미지 Base 64 인코딩



## HTTP/1.1

한 번 `TCP 초기화`를 한 이후에 `keep-alive`라는 옵션으로 여러 개의 파일 송수신

다수의 리소스(이미지, css 파일, script 파일)을 처리하려면 개수에 비례해 대기 시간이 길어진다

### HOL Blocking

네트워크에서 같은 큐에 있는 패킷이 첫 번째 패킷에 의해 지연될 때 발생하는 성능 저하 현상

### 무거운 헤더 구조

쿠키 등 많은 메타데이터가 들어가 있다

압축이 되지 않음



## HTTP/2

HTTP/1.x보다 지연 시간을 줄이고 응답 시간을 더 빠르게 할 수 있다

### 멀티플렉싱

여러 개의 스트림을 사용하여 송수신

![image](https://github.com/mindot7/TIL/assets/97648258/86a8fe45-5969-4048-9f25-77e0c9b3485e)

--> 여러 요청과 응답을 통해 HOL Blocking 해결

### 헤더 압축

HTTP/1.x의 무거운 헤더를 해결하기 위해 허프만 코딩 압축 알고리즘을 사용해 HPACK 압축

### 서버 푸시

클라이언트 요청 없이 서버가 바로 리소스를 푸시할 수 있다

> 클라이언트가 html 파일을 요청하면 서버에서 html파일과 css파일을 같이 푸시



## HTTPS

HTTP/2는 HTTPS 위에서 동작

### 정의

애플리케이션 계층과 전송 계층 사이에 신뢰 계층인 SSL/TLS 계층을 넣은 `신뢰할 수 있는 HTTP 요청`

통신의 암호화

### SSL/TLS

TLS이 최신버전

#### 정의

전송 계층에서 보안을 제공하는 프로토콜

## SEO

SEO: Search Engine Optimization (검색엔진 최적화)

- 구글의 SSL인증서 강조

  HTTPS 서비스를 하는 사이트가 그렇지 않은 사이트보다 SEO 순위가 높을 것이다

### 구축 방법

1. CA에서 구매한 인증키를 기반으로 구축
2. 서버 앞단의 HTTPS를 제공하는 로드밸런서를 둔다
3. 서버 앞단에 HTTPS를 제공하는 CDN을 둔다



# CORS

## 정의

Cross-Origin Resource Sharing

서버에서 다른 Origin에 대한 요청을 허용하는 정책

서버가 웹 브라우저에서 리소스를 로드할 때 다른 오리진을 통해 로드하지 못하게 하는 HTTP 헤더 기반 메커니즘 (SOP)

> 오리진
>
> - 프로토콜과 호스트 이름, 포트의 조합

Origin을 기준으로 다른 Origin에 API를 요청하면

--> 응답을 보내는 출처가 다른 출처여도 예상된 출처라면 요청에 대한 응답을 보낸다

## 해결

FE 개발 시 FE 서버를 만들어서 BE 서버와 통신할 때 마주치는 에러

1. FE에서 `프록시 서버`를 만들어 해결한다

2. 개발 환경 프록시 설정

```shell
프론트 WithCredentials: true
서버 Access-Control-Allow-Credentials: true
```





*참고자료*

면접을위한CS전공지식노트/길벗/주홍철

https://joshua1988.github.io/web-development/http-part1/#%EB%93%A4%EC%96%B4%EA%B0%80%EB%A9%B0

https://github.com/junh0328/prepare_frontend_interview/blob/main/cs.md#CORS



