# DOM



## DOM(Document Object Model)

> 문서 객체 모델(The Document Object Model, 이하 DOM)은 HTML, XML 문서의 프로그래밍 **interface**이다. DOM은 문서의 **구조화된 표현(structured representation)을 제공**하며 프로그래밍 언어가 **DOM 구조에 접근할 수 있는 방법**을 제공하여 그들이 **문서 구조, 스타일, 내용 등을 변경**할 수 있게 돕는다. DOM 은 **구조화된 nodes와 property와 method**를 갖고 있는 **objects**로 문서를 표현한다. 이들은 웹페이지를 스크립트 또는 프로그래밍 언어들에서 사용될 수 있게 연결시켜주는 역할을 담당한다.
>
> 출처: MDN




### DOM 이 생겨난 이유?

##### 태초에 웹은 문서를 공유하기 위해 만들어졌다.

웹은 문서를 공유하기 위해 만들어졌고, 해당 문서를 만들기 위해서 HTML이라는 문서에 마크업이라는 언어를 만들고 문서를 만들 수 있게 되었다.

이후 서버에서 프로그래밍을 통해 HTML을 제작할 수 있는 PHP와 같은 서버언어가 등장했고, 이를 통해 컨텐츠가 포함된 HTML을 동적으로 생성할 수 있게 되었다.

이 개념을 **브라우저가 새로고침을 하지 않고 서버의 개입없이 브라우저 내부에서 동적으로 HTML을 변경**하고자 자바스크립트가 탄생했다.

-> 브라우저가 먼저 만들어졌고, 서버언어가 만들어진 다음 **자바스크립트는 나중에** 만들어졌다.



##### HTML은 문자열이다

HTML이 좋은 점은 바로 문자열로 이루어져있다는 점이다. 그렇기에 서버 언어에서는 별다른 HTML 전용 API없이도 문자열의 조작을 통해서 얼마든지 HTML을 만들어 낼 수 있었다.

> PHP Code:
>
> ```js
> <?php
> $name = "teo.yu";
> ?>
> <h1>hello, <?=$name?>!</h1>
> ```
>
> HTML Result:
>
> ```js
> <h1>hello, teo.yu!</h1>
> ```

#####  그렇다면 javascript에서도?

![htmljs](https://velog.velcdn.com/images%2Fteo%2Fpost%2Fde0d1112-f35f-404d-bc41-309663728c89%2Fimage.png)

출처: https://imyeonn.github.io/blog/web/86/

서버에서 그랬던 것처럼 화면에 그대로 문자열을 출력하듯이 HTML을 출력해버린다면?

`document.write()`라는 간단한 api를 만들어 서버에서 동작하듯이 만들어 보았다.

이 방식은 맨 처음 동적으로 문서를 만드는 것까지는 좋았으나, 이후 새롭게 문서를 변경하기 위해서 몇 가지 문제점이 생긴다.

##### HTML 문서가 화면에 그려지기까지

HTML은 결국 사람이 편리하게 다룰 수 있는 문자열이지 컴퓨터가 알아듣고 사용하기에 좋은 언어는 아니다. 컴퓨터가 이를 해석하고 그릴 수 있게 하기 위해 HTML을 분석하고 정리해서 화면에 그릴 수 있는 구조를 만들어야 한다. 이 과정은 확실히 적은 비용은 아니다.

![tokenization](https://velog.velcdn.com/images%2Fteo%2Fpost%2F4bd9515d-517d-4b9c-b551-02708bacb187%2Fimage.png)

![domtree](https://velog.velcdn.com/images%2Fteo%2Fpost%2F8b025a12-c16f-484a-ba8b-1389b0d53497%2Fimage.png)

출처: https://yceffort.kr/2021/11/jorney-from-tags-to-dom

이렇게 화면에 그릴 수 있는 구조를 이미 만들어둔 상태에서 일부 변경된 화면을 만들기 위해 처음부터 다시 변경된 HTML을 분석하여 다시 전부 구조를 만들어 적용하는 방식은 비효율적이다.

그래서 최초 HTML을 통해 한 번 구조를 만들었다면 다음 번 변경은 HTML의 조작이 아닌 이미 만들어진 구조에서 직접 일부 수정을 하는 방식을 통해 훨씬 더 적은 비용으로 화면을 변경할 수 있을 것이다.

##### 그래서 필요한 것이 DOM!

![htmltodom](https://velog.velcdn.com/images%2Fteo%2Fpost%2F9badf1fa-6d4c-4e09-9ee6-5aad257e7c91%2Fimage.png)

출처: https://simplesnippets.tech/what-is-document-object-modeldom-how-js-interacts-with-dom/

> HTML을 동적으로 보다 효율적으로 변경하기 위해서 HTML **문서(Document)**를 자바스크립트가 이해할 수 있는 **객체(Object)**의 형태로 **모델링(Modeling)**하여 자바스크립트에서 조작을 할 수 있도록 만든 **interface**가 바로 이 **DOM(Document Object Model)**이다.

##### Interface? DOM과 Javascript의 관계

> #### DOM 과 자바스크립트
>
> 이 문서의 대부분의 예제와 같이, 위에서 사용된 예제는 JavaScript이다. 위의 예제는 자바스크립트로 작성되었지만 문서(document) 와 문서의 요소(element) 에 **접근**하기 위해 DOM 이 사용되었다. **DOM 은 프로그래밍 언어는 아니지만** DOM 이 없다면 자바스크립트 언어는 웹 페이지 또는 XML 페이지 및 요소들과 관련된 모델이나 개념들에 대한 정보를 갖지 못하게 된다. 문서의 모든 element - 전체 문서, 헤드, 문서 안의 table, table header, table cell 안의 text - 는 문서를 위한 document object model 의 한 부분이다. 때문에, 이러한 요소들을 **DOM 과 자바스크립트와 같은 스크립팅 언어**를 통해 접근하고 조작할 수 있는 것이다.
>
> 초창기에는 자바스크립트와 DOM 가 밀접하게 연결되어 있었지만, 나중에는 각각 분리되어 발전해왔다. 페이지 콘텐츠(the page content)는 DOM 에 저장되고 자바스크립트를 통해 접근하거나 조작할 수 있다. 이것을 방정식으로 표현하면 아래와 같다:
>
> **API (web or XML page) = DOM + JS (scripting language)**
>
> **DOM 은 프로그래밍 언어와 독립적으로 디자인되었다.** 때문에 문서의 구조적인 표현은 단일 API 를 통해 이용가능하다. 이 문서에서는 자바스크립트를 주로 사용하였지만, DOM 의 구현은 어떠한 언어에서도 가능하다. 아래는 파이썬을 사용한 예제이다:
>
> 출처: [MDN](https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/Introduction)

이미 브라우저는 자바스크립트가 만들어지기 전에 이미 HTML을 파싱하고 화면으로 표현을 할 수 있는 내부 구조를 가지고 있었다. 그리고 나서 javascript가 만들어졌다. 즉, 이 내부 구조가 javascript로 만들어 진 것이 아니라는 뜻이다.

그저 이러한 구조를 javascript를 통해서 조작을 할 수 있는 api를 제공하는 형식으로 만들어지게 되었다.

이후 모든 브라우저에서의 동작은 같은 형태로 만들어야 한다는 웹표준이라는 것을 제정하게 되면서 **어떠한 언어로든 이 구조에 접근을 할 수 있는 api명세를 통일할 필요**가 생겼고, DOM을 정의했다.

그래서 엄밀히 말하면 **DOM은 API 중에서 공통 인터페이스 부분만을 의미**한다.

> API(web or XML page) = DOM + JS (scripting language)



### DOM의 구조

사실상 DOM의 본질은 다음과 같다.

> ✔ DOM은 웹 페이지의 객체 지향 표현이며, **자바스크립트와 같은 스크립팅 언어를 이용해 DOM을 수정**할 수 있다.
>
> ❗ DOM = HTML을 수정하는 법을 컴퓨터가 알아듣게 작성하는 방법
>
> 즉, 자바스크립트는 브라우저가 읽고 어떤 작업을 할 수 있는 언어이고, DOM은 바로 이 작업이 이루어지는 장소 인 것이다.
>
> 우리가 **"Javascript로 하는 것"**이라고 생각하는 것은 정확히는 **"DOM API"**이다.

DOM은 일단 HTML을 표현하고 조작하는데 목적이 있기 때문에 **HTML의 구조**를 그대로 닮아있게 된다. 

![structureofhtml](https://velog.velcdn.com/images%2Fteo%2Fpost%2F35e73082-fce2-4047-850d-3ca7c39d366f%2Fimage.png)

HTML에는 Tag를 가지는 Element와 Text로 나뉘고 엘리먼트는 여러개의 Attribute를 가진다.

DOM에서 이러한 각각의 요소를 Node라고 부르며 HTML과 마찬가지로 Element Node, Attribute Node, Text Node로 부르게 된다. 그 밖의 주석이나 문서와 같은 Node도 존재한다.



##### DOM Tree

![domtree](https://velog.velcdn.com/images%2Fteo%2Fpost%2Fd098a02b-d07b-4870-a93c-c74f874cb8be%2Fimage.png)

HTML의 엘리먼트는 동일한 엘리먼트를 포함할 수 있는 구조가 된다. 이러한 구조를 Tree라 부르기에, HTML이 여러개의 Element와 Node로 구성되어 있는 것을 DOM Tree라고 부른다.

![domtreeasfile](https://velog.velcdn.com/images%2Fteo%2Fpost%2Fbc6c95fd-7a72-43d0-82fc-e226a937ce67%2Fimage.png)



## Virtual-DOM

DOM은 `HTML Elements`, `Attributes`, `CSS styles`, `Events`, `Methods` 등을 제어할 수 있는 표준 인터페이스를 제공한다. 다시 정의하면 **웹페이지를 구성하는 요소를 구조화해서 나타낸 객체이고, 이 객체를 이용해서 웹페이지 구성요소를 제어할 수 있다**. 

그렇다면 Virtual DOM은 왜 Virtual DOM일까?

### Virtual DOM?

> Virtual DOM은 React, Vue, 및 Elm과 같은 선언적 웹 프레임워크에서 사용되는 DOM의 경량 JavaScript표현이다. Virtual DOM을 업데이트 하는 것은 화면에 아무것도 렌더링 할 필요가 없기 때문에 실제 DOM을 업데이트 하는 것보다 비교적 빠르다.

### Virtual  DOM은 어떤 문제를 해결하기 위해 나온 기술인가?

- DOM 조작에 의한 렌더링이 비효율적인 문제
- SPA 특징으로 DOM 복잡도 증가에 따른 최적화 및 유지보수가 더 어려워지는 문제

⭐결론적으로 **DOM을 반복적으로 직접 조작하면 그만큼 브라우저가 렌더링을 자주 하게 되고, 그만큼 PC자원을 많이 소모하게 되는 문제를 해결하기 위한 기술**이다.

예를 들어

```JS
document.getElementById('hello').innerHTML = 'hello world!';
document.body.style.background = 'black';
```

위와 같이 자바스크립트로 DOM에 접근하여 내용을 바꾸거나 스타일을 바꿀 수 있다. 예전의 jQuery와 같은 라이브러리도 위와 같이 DOM의 메서드를 이용해 DOM을 바꿀 수 있었는데, DOM이 변경될 때마다 브라우저 엔진은 렌더링을 다시 한다.

현대의 웹처럼 변경이 많고, 변경할 대상도 많을 시 DOM을 계속해서 변경하는 비효율적인 작업이 생긴다. 정확히는 DOM을 변경하는 문제가 아니고 렌더링을 여러번 하는 게 문제,,

### Virtual DOM은 이 문제를 어떻게 해결했나?(동작 원리)

Virtual DOM은 DOM과 유사한 역할을 담당하는 객체이다. 즉, 변경 사항을 DOM에 직접 수정하는 게 아니라, 중간단계로 Virtual DOM을 통해 DOM을 수정하게 한다.

실질적인 방법은 Virtual DOM에 변경 내역을 한번에 모으고 실제 DOM과 변경된 Virtual DOM의 차이를 판단한 후 , 변경된 부분만 찾아 변경하고, 그에 따른 렌더링을 한 번만 하는 것으로 해결했다.

예를 들어,

- 만약 어떤 게시판에서 `<ul>` 태그 안의 `<li>`를 변경하는데 DOM에 접근하여 `<li>`태그를 10번 을 바꾸지 않고 Virtual DOM을 통해 `<li>`태그10개를 변경하고 Virtual DOM과 실제 DOM을 비교하여 변경된 부분을 1번만에 변경하여 렌더링도 1번만 일어나게 하는 것 이다

### Virtual DOM의 주의사항과 한계

- 0.1초마다 화면에 데이터가 변경이 된다면 Virtual DOM으로 0.5초씩 모아가지고 렌더링을 적게할 수 있나? 
  - -> 안된다. 동시에 변경하는 것에 한해서만 렌더링된다.

- React나 Vue등을 이용해서 Virtual DOM을 쓰면 무조건 빠른가? 
  - -> 아니다. 똑같이 최적화를 해야한다. (슬라이드를 옮기거나 무한 스크롤등의 움직임이 있을 때는 Virtual DOM을 이용해서 반복 렌더링을 하지 않도록 해줘야 한다.)

- Virtual DOM은 메모리에 존재한다. DOM에 준하는 무거운 객체(Virtual DOM)가 메모리에 있기 때문에 메모리의 사용이 많이 늘어날 수 밖에 없다.

- Virtual DOM을 조작하는 것도 엄청나게 많은 컴포넌트를 조작하게 된다면 오버헤드가 생기기 마련이다. Virtual DOM 제어가 DOM 직접 제어에 비해 상대적으로 비용이 적게 들 뿐이다.



## Reference

[DOM은 다들 어떤식으로 공부하셨나요?](https://velog.io/@teo/dom)

[DOM (Document object Model) 완벽 정복하기](https://geniee.tistory.com/32)

[기술면접4탄-dom](https://ksmfou98.github.io/Interview/%EA%B8%B0%EC%88%A0%20%EB%A9%B4%EC%A0%91%204%ED%83%84/)