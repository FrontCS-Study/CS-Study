[TOC]

# CSS 레이아웃

레이아웃이란 웹 페이지에 포함될 요소들의 공간적 위치를 배열 시키는 자리 배치를 의미한다. 

CSS레이아웃에 사용되는 기술은 다음과 같다.

- normal flow
- display 속성
- flexbox
- grid
- float
- positioning
- 테이블 레이아웃(table layout)
- 다단 레이아웃(multiple-column layout)

## normal flow

normal flow란 보통 흐름을 뜻하며, 페이지 레이아웃을 제어하지 않았을 경우 브라우저가 요소들을 기본 값으로 배치하는 것을 의미한다.

```html
<h1>Basic document flow</h1>
<p>
  I am a basic block level element. My adjacent block level elements sit on new
  lines below me.
</p>
```

## display 속성

CSS **`display`** 속성은 플로우 레이아웃 내에서 요소가 블록 레벨과 인라인 요소 중 어느 것으로 참여해야 하는지, 그리고 요소 자신의 내부 레이아웃은 플로우, 플렉스, 그리드 등의 종류 중 어느 것이어야 하는지 설정한다.

기본 유형 종류 : { display : (block, inline, inline-block, none, table, flex, grid 등) }

- `none` : 요소가 페이지에 보이지 않고, 공간을 차지하지도 않는다.
  - visibility 속성을 hidden으로 설정에도 요소가 보이지 않지만, 해당 공간이 존재한다는 차이점이 있다.

- `block` : 요소를 블록 박스화 시켜, 항상 새 줄에서 시작하여 사용 가능한 화면 너비를 차지한다.
  - width, height, margin, padding 프로퍼티 지정이 가능하다.
  - block요소 내에 inline 레벨 요소를 포함 할 수 있다.
  - 기본값 태그 : **div태그, p태그, h태그, ul태그, li태그, form태그** 등
- `inline` : 인라인 박스, block과 달리 줄바꿈이 되지 않고, 새 줄에서 시작되지 않으며, content 너비만큼 가로폭을 차지한다.
  - width, height, margin-top, margin-bottom 프로퍼티를 지정할 수 없다. 상, 하 여백은 line-height로 지정한다.
  - inline 레벨 요소 내에 block 레벨 요소를 포함할 수 없다.
  - 기본값 태그 : **span태그, a태그, img태그, button태그** 등

- `inline-block` : block과 inline의 중간 형태
  - 요소 자체는 inline요소처럼 한 줄에 표현되면서, block 요소처럼 크기나 여백을 모두 지정할 수 있다.

- `flex` : 콘텐츠를 **flexbox 모델**에 따라 배치한다. 아이템들을 가로 방향 혹은 세로 방향으로 1차원 배치할 수 있는 방식이다. 
- `grid` : 콘텐츠를 **Grid 모델**에 따라 배치한다. Flex와는 다르게 2차원으로 배치하는 방식으로 column과 row의 비율이나 크기를 지정한다.
- `table` : 레이아웃을 table로 표현할 수 있는 속성이다. HTML \<table> 요소처럼 동작한다. 

## Flexbox

<div align="center">
    <img src="https://studiomeal.com/wp-content/uploads/2020/01/01-1.jpg" width="40%" alt="flex와 gird 레이아웃"/>
</div>

flexbox는 **CSS 가변 상자 레이아웃(CSS Flexible Box Layout)** 모듈의 줄임말로, 사용자 인터페이스 디자인과 **1차원 레이아웃**에 최적화된 CSS 모듈이다.

flexbox의 적용을 원하는 요소들의 부모 요소에 **display: flex** 속성을 적용하고 나면 모든 자식이 flex 속성을 지닌다.

flex의 속성은 컨테이너에 적용되는 속성, 아이템에 적용되는 속성으로 나뉜다.

> flex 속성 참고 : [이번에야말로 CSS Flex를 익혀보자](https://studiomeal.com/archives/197)

## Grid

flexbox와 다르게 grid는 **2차원 레이아웃**을 지원한다.  
즉, flexbox는 행을 변경 및 지정하며, grid는 행과 열을 변경 및 지정할 수 있다. 

Flex와 마찬가지로, Grid는 컨테이너에 **display: grid** 속성을 적용하여 각각의 아이템을 배치한다.

> grid 속성 참고 : [이번에야말로 CSS Grid를 익혀보자](https://studiomeal.com/archives/533)

## float

float를 사용하면 normal flow 속에 해당 요소와 해당 요소를 뒤따르는 블록 수준 요소의 동작이 변경된다. 

float된 요소는 페이지의 흐름의 일부가 되어 주변 콘텐츠의 위치에 영향을 준다.  
(페이지의 흐름에서 제거되는 position: absolute 요소와 다르다.)

**float: (none, left, right, inline-start, inline-end)**

주로 이미지 주변에 텍스트를 감싸기 위해 만들어진 프로퍼티이다.

<div align="center">
    <figure>
      <img src="https://blog.kakaocdn.net/dn/cCFwCD/btrpIzrE5Ul/KveuA89x8JzWLe2Sh0TGx1/img.png" alt="float 레이아웃" width="60%">
      <figcaption><small>이미지 출처:<a href="https://mystudy.tistory.com/30">[CSS] CSS레이아웃(normal flow, display...)</a></small></figcaption>
    </figure>
</div>

## positioning

포지셔닝(positioning)은 normal flow에 요소를 기존의 배치 위치에서 벗어나 다른 위치로 이동시킬 수 있는 기술이다.  
포지셔닝이 레이아웃과 차이점은 레이아웃을 생성하는 것이 아닌 특정 요소의 위치를 관리하고 조정한다는 것이다.

**position : (static, relative, absolute, fixed, sticky)**

포지셔닝은 position 속성을 사용하며, 다음과 같이 5가지 포지션 유형이 있다.

- **정적 포지션(static position)** : 모든 요소에 기본값으로 지정된 속성으로, 요소를 normal flow에 따라 배치한다.

- **상대 포지션(relative position)** : normal flow 상의 요소를 기존 위치에 비례해 요소 위치를 수정 및 배치한다.

  - 이때 요소는 normal flow에 따라 배치된다.

  - right, left, top, bottom 값에 따라 오프셋을 적용한다.

    > 🔍 오프셋이란 기준이 되는 위치로부터 얼마나 떨어져 있는지를 나타내는 값을 의미한다.

- **절대 포지션(absolute position)** : 요소를 normal flow에서 제거한다.

  - 브라우저에서 페이지 레이아웃 시 공간을 배정하지 않는다.

  - 상위 요소의 블록의 기준으로 오프셋을 적용한다.
  - **상위 태그 중 position 속성이 설정되지 않았다면 최상위 태그인 `<body>`를 기준으로 한다.**

- **고정 포지션(fixed position)** : 요소를 normal flow 에서 제거한다.
  - absolute position과는 다르게 뷰포트를 기준으로 오프셋을 적용한다.  
    따라서 페이지가 스크롤될 때 고정된 위치를 지닌다.
- **스티키 포지션(sticky position)** : 정적 포지션과 고정 포지션의 혼합형이다.
  -  스크롤 위치가 임계점에 이르면 position: fixed와 같이 박스를 화면에 고정할 수 있는 속성으로 스크롤 영역 기준으로 배치한다.

## table layout

과거에 웹 페이지 전체 레이아웃을 형성하는데 table요소(tr, th, td태그)가 사용되었다.  
하지만 과도한 마크업, 디버그 문제, 의미론적으로 적절하지 않아 현재는 사용하지 않는다.

[[웹 페이지 레이아웃에서 테이블을 피하는 이유]](https://ko.eyewated.com/%EC%9B%B9-%ED%8E%98%EC%9D%B4%EC%A7%80-%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83%EC%97%90%EC%84%9C-%ED%85%8C%EC%9D%B4%EB%B8%94%EC%9D%84-%ED%94%BC%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0/)

해당 문서에 따르면 테이블 레이아웃을 사용하지 않는 중요한 이유는 다음과 같다.

- 웹 페이지 접근성이 떨어진다.
- 유지관리에 어렵다.
- 유연한 레이아웃을 생성할 수 없다.
- 많은 마크업을 사용하기에 느리게 로드된다.
- 페이지를 섹션에 따라 인쇄를 할 시 레이아웃이 망가진다.

> 🔍 **HTML table요소와 CSS의 display: table 속성의 차이점**
>
> table 요소는 블록 레벨 요소로 간주되며, 새로운 줄에서 시작하고 가로 폭 전체를 차지한다.  
> display: table 속성을 사용한 요소는 기본적으로 인라인 레벨 요소로 간주되어 다른 요소와 동일한 줄에 배치된다. 따라서 레이아웃에 맞게 조정하기 위해 추가적인 CSS가 필요할 수 있다. 
>
> table 요소는 HTML에서 테이블을 생성하는 데 사용되며, 시맨틱 마크업과 테이블에 특화된 속성 및 기능을 제공한다.  
> display: table 속성은 일반적인 HTML 요소를 테이블 형태로 스타일링하기 위해 사용되며, 스타일링의 유연성과 다양한 레이아웃 옵션을 제공한다.

## multiple column layout

muliple column layout은 다단 레이아웃으로 불리며 단 형태로 나타나는 레이아웃이다.



<div align="center">
    <figure>
      <img src="https://blog.kakaocdn.net/dn/etjsm7/btrpKmr21p9/7ZiHRKkzr0WqZWtciP1kA0/img.png" alt="float 레이아웃" width="60%">
      <figcaption><small>이미지 출처:<a href="https://mystudy.tistory.com/30">[CSS] CSS레이아웃(normal flow, display...)</a></small></figcaption>
    </figure>
</div>

다단 레이아웃에 사용되는 속성은 다음과 같다.

**column-count 속성**

- 다단 개수를 지정할 때 사용된다.
- 기본값은 2이며, 속성 값만큼의 다단을 생성한다.

**column-width 속성**

- 속성 값으로 다단의 폭을 설정한다.
- 설정한 폭에 맞게 다단이 생성된다.

```css
/* 3열 레이아웃 적용 예시 */
.container {
  column-count: 3;
}
```

<br>

---

**참고**

[[CSS] CSS 레이아웃 (normal flow, display, flex box, grid, float ...](https://mystudy.tistory.com/30)

[[정보통신기술용어해설] CSS 레이아웃](http://www.ktword.co.kr/test/view/view.php?m_temp1=5703)

[[CSS] display 속성](https://velog.io/@sukong/CSS-display-속성)

[CSS3 Display](https://poiemaweb.com/css3-display)
