[TOC]

## DOM

문서 객체 모델(DOM, Document Object Model)은 **브라우저가 HTML을** **읽고 조작할 수 있도록** 요소들를 **구조적**으로 표현한 것이다.  

즉, 자바스크립트 같은 언어가 쉽게 웹 페이지에 접근하여 조작할 수 있게 연결시켜주는 역할을 담당한다.

> 자바스크립트는 DOM을 조작할 수 있는 프로그래밍 언어 중 하나일 뿐이지 자바스크립트가 DOM인 것은 아니다!



### DOM 구성 요소

DOM은 모든 요소들과의 관계를 부자 관계로 표현한 트리 구조로 구성되어 있다.  
트리 구조로 구축이 되기 때문에, HTML 문서는 최종적으로 하나의 최상위 노드(root 노드)에서 시작해 자식 노드들을 가지며, 아래로만 뻗어나가는 구조로 만들어진다.

<div align="center">
<img src="https://github.com/FrontCS-Study/CS-Study/assets/83412032/faa2d4de-aa96-40e5-b21f-9a3aa0c64c31" alt="DOM 구성요소" width="60%" />
</div>

document 노드가 최상위 노드가 되고, 밑으로 element 노드가 오며, 이어 text 노드와 attribute 노드가 오는 계층적인 구조이다.  

이러한 노드 타입에는 총 12개가 있다. 이 중에서 중요한 노드 타입은 다음과 같이 4가지다.

#### 1. document node (문서 노드)

DOM Tree에서 최상위 루트 노드이자, document 객체를 가리킨다. HTML 문서 전체를 나타내는 노드이기도 하다.  
window 객체의 document 프로퍼티로 바인딩(연결)이 되어 있어 **`window.document`** , **`document`** 로 참조해 사용할 수 있다.   
HTML 문서에 이 문서 노드는 오로지 1개만 존재한다.

#### 2. element node (요소 노드)

모든 HTML 요소(body, h2, div, span 등)는 요소 노드이다.  
속성 노드를 가질 수 있는 유일한 노드로서, 부모-자식 관계를 가지게 되기 때문에 계층적 구조를 이룰 수 있다.

#### 3. attribute node (속성 노드)

`<input>`태그 안에 name, value 등의 속성을 사용 할 수 있는데 이러한 속성을 가르키는 노드를 속성 노드라고 한다.   
모든 HTML 요소의 속성은 속성 노드이며, 요소 노드에 대한 정보를 가지고 있다.   
그렇기 때문에 부모 노드가 아닌 해당 노드와 연결(바인딩) 되어 있다.

#### 4. text node (텍스트 노드)

텍스트 노드는 요소 노드의 자식이며 자신의 자식 노드를 가질 수 없기 때문에 DOM Tree에 가장 마지막에 위치하는 자식 노드이다.

`<span>안녕</span>`에서 텍스트 노드는 '안녕'이다.

<br>

## 가상 DOM

DOM Tree가 수정될 때마다 랜더 트리가 계속 실시간으로 갱신되어 성능에 좋지 않다.  
즉, 화면에서 10개의 수정 사항이 발생하면 수정할 때마다 새로운 랜더 트리가 10번 수정되면서 새롭게 만들어지는 단점이 있다.

**Virtual DOM** 은 브라우저 내부에서 사용되는 개념으로, DOM의 상태를 메모리에 넣어두고 변경이 있을 때만 업데이트한다.   
즉, **화면에 그려지지 않고 아직 메모리에 있는 상태**를 말한다.

Virtual DOM은 DOM의 변경 사항이 일어날 때마다, Virtual DOM에 해당하는 데이터를 업데이트하고, 이전 버전의 Virtual DOM과 새 버전의 Virtual DOM을 비교하여 변경된 부분만 실제 DOM에 업데이트하는 방식으로 작동한다. 결과적으로 브라우저는 한번만 렌더링을 하게 됨으로써 **불필요한 렌더링 횟수**를 줄일 수 있다.

가상 돔을 활용한 대표적인 프론트엔드 프레임워크로 React, Vue, Angular이다. 

> 💡 **DOM과 Virtual DOM**
>
> DOM은 HTML 문서를 파싱해서 문서의 객체 모델을 생성하는 역할,  
> Virtual DOM은 실제 DOM의 가벼운 복사본으로서 렌더링 성능을 최적화하는 역할을 한다.
>
> |                              |                       DOM                        |                         Virtual DOM                          |
> | :--------------------------: | :----------------------------------------------: | :----------------------------------------------------------: |
> |           업데이트           |                      느리다                      |                            빠르다                            |
> |      HTML 업데이트 방식      |            HTML을 직접적으로 업데이트            |             HTML을 직접적으로 업데이트 하지 않음             |
> | 새로운 element 업데이트 방식 | 새로운 element가 업데이트된 경우 새로운 DOM 생성 | 새로운 element가 업데이트 된 경우 새로운 가상 DOM 생성 후 이전 DOM과 비교 후 차이점 DOM만 업데이트 |
> |            메모리            |                메모리 낭비가 심함                |                      메모리 낭비가 덜함                      |

<br>

---

**출처**

[문서 객체 모델 DOM 과 자바스크립트 JavaScriptㅣ생성 방식 및 접근 방법](https://www.codestates.com/blog/content/dom-javascript)

[[Javascript] Window, DOM, BOM](https://cbw1030.tistory.com/46)

[Virtual DOM, 가상 돔 이란?](https://doqtqu.tistory.com/316)

[돔(dom)과 가상 돔(virtual dom)](https://velog.io/@kcj_dev96/%EB%8F%94dom%EA%B3%BC%EA%B0%80%EC%83%81-%EB%8F%94virtual-dom)
