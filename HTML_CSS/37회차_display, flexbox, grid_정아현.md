# display

> 💡display 속성?
>
> display 속성은 해당 HTML 요소가 웹 브라우저에 언제 어떻게 보이는가를 결정하는 요소이다. 디폴드는 inline이며, 모든 elements에 적용 가능하다.

display 속성은 요소 자신에 적용되는 것은 외부 디스플레이 타입(outer display type)(inline, block등) 고 자식 요소들의 레이아웃에 영향을 주는 내부 디스플레이 타입(inner display type)(flex 등)으로 나뉜다.

### 0️⃣ display : none

요소를 **렌더링하지 않도록** 설정하고 visibility 속성을 hidden으로 설정한 것과는 달리 **영역 또한 차지하지 않는다.** 크기 자체가 없어지는 것이다. 따라서 HTML 문서 레이아웃에 아무런 영향을 주지 않는다. 

감추어진 요소는 사용자 액션을 받을 수 없기 때문에, 사용자 액션에 따른 이벤트도 발생하지 않는다.

```html
<style>
.display-none{ display: none }
.invisible{ visibility: hidden }
</style>

<div class="display-none">1</div>
<div>2</div>

<div class="invisible">3</div>
<div>4</div>
```

**출력결과**

2



4

### 1️⃣ display : block;

일반적으로 설정하지 않아도 **div가 갖게되는 기본값**이다. (태그에 따라 기본값이 다를 수 있음)

기본적으로 **width가 자신의 컨테이너의 100%**가 되게끔 한다. 즉, 가로 한 줄을 다 차지 한다. 또한 항상 줄 바꿈이 되어 새로운 라인에서 시작한다. 문서에서 문단을 표시할 때, 한 문단이 끝난 뒤에 나타나는 요소는 항상 다름 줄에 표시되는 것과 비슷한 맥락이다.

**`<div>`, `<p>`, `<h>`, `<li>`, `<form>`** 등이 해당한다.

### 2️⃣ display : inline;

**컨텐츠를 딱 감쌀정도의 크기**만을 갖게된다. block 태그와 달리 줄바꿈이 되지 않고, 반드시 컨텐츠를 감싸게 되고 **크기를 변화시킬 수 없다**. width와 height, 여백을 지정할 수 없다는 얘기다. 문서에서 볼드, 이탤릭, 색상, 밑줄 등 **글자나 문장에 효과를 주기 위해 존재하는 단위**라고 할 수 있다.

**`<span>`, `<a>`, `<b>`, `<i>`, `<img>`** 등이 해당한다.

### 3️⃣ display : inline-block;

inline과 block의 특성을 합쳐놓은 속성이다. 기본적으로는 inline의 속성을 지니고 있지만, 임의로 크기를 바꿔줄 수 있다. inline처럼 줄바꿈은 되지 않지만, block처럼 크기나 여백을 지정할 수는 있는 것이다.

### 4️⃣ display : flex;

이름처럼 flexible한 특징을 갖고 있어 많은 경우에 자동으로 자식 요소들의 너비를 분배해 레이아웃을 맞춰주기 때문에 사용하기 쉽고 편리하다.

가변 레이아웃이나 반응형 웹에서 웹 브라우저의 너비에 따라 자동으로 레이아웃의 너비가 조절되도록 할 수 있기 때문에 쉽고 빠른 레이아웃 구현이라는 요구 조건에 잘 부합하는 속성이다.

기본적으로 block 속성을 가진다. flex 디스플레이 속성을 적용하면 자식 요소들은 flex 컨테이너의 구성요소가 되어  인라인 요소들이 배치된다. 자식 요소가 인라인으로 컨텐츠 영역만큼만 자리를 차지하기 때문에, 크기 속성을 부여해 영역을 해울 수 있다. 인라인 요소이기 때문에 전체 너비보다 자식 요소들의 너비 합이 더 넓으면 비율에 따라 너비 값이 자동으로 정해진다.

### 5️⃣ display : grid;

격자형 그리드 구조를 만들 수 있는 속성값이다.

바둑판 모양의 행과 열을 가진 구조를 추가 속성 선언만으로 간편하게 만들 수 있고, 셀(들)을 병합해서 영역의 구획을 나눌 수도 있다. 행과 열 사이의 여백도 설정할 수 있기 때문에 격자형 구조를 구현하는데 최적의 속성이다.

기본적으로 block 속성을 가진다.

`grid-template-rows`는 가로에 배치할 셀들의 비율이나 크기를 지정하는 속성이고, `grid-template-columns`는 세로로 배치할 셀들의 비율이나 크기를 나타낸다.

### 6️⃣ display : table;

table 태그와 유사한 레이아웃을 CSS로 구현할 수 있도록 해주는 속성이다.

모던 웹에 적합하지 않는 테이블 태그를 대신해 일반 태그로 테이블과 같은 격자형 배치를 가능하도록 한다. 
고정된 행과 열 구조를 가지는 테이블과 달리 반응형으로 행과 열 구조를 변경할 수 있기 때문에 조금 더 유연한 격자형 구조를 만들 수 있다.

table 속성과 함께 사용할 수 있는 전용 속성 개수가 적어서 다른 레이아웃 속성들의 도움을 받아야 테이블 태그의 속성과 같은 수준의 레이아웃을 만들 수 있다.

기본적으로 block 속성을 가진다.

display : table 			// table 요소처럼 표현하기
display : table-row 	// tr 요소처럼 표현하기
display : table-column 	// col 요소처럼 표현하기
display : table-cell 	// td 요소처럼 표현하기
display : table-caption 	// caption요소처럼 표현하기



# Flexbox, Grid

### 💡Flexbox?

일명 flexbox라 불리는 Flexible Box module은 flexbox 인터페이스 내의 아이템 간 공간 배분과 강력한 정렬 기능을 제공하기 위한 1차원 레이아웃 모델로 설계되었다. 

flexbox는 행과 열 형태로 항목 무리를 배치하는 일차원 레이아웃 메서드이다. 항목은 부족한 공간에 맞추기 위해 축소되거나 여분의 공간을 채우기 위해 변형된다.

flexbox를 사용하면 유동적으로 각 요소들을 배치할 수 있고, 웹사이트의 요소들을 수평적으로 배치할 수 있다. 웹 사이트에 요소들을 배치하면 기본적으로 위에서 아래로 쌓이는 방식이지만 flexbox를 사용하면 손쉽게 수평 배치가 가능하다.

flexbox는 크게 두 가지 타입에 대해 적용되는데, 

![flexbox](https://user-images.githubusercontent.com/35194820/149148924-896b8ef8-a080-44f3-a0f5-6ffbeecff0a3.png)

> container: item을 담고 있는 박스
>
> item: container에 포함되어 있는 요소들

- container에 지정되어 있는 속성들

  ![container](https://user-images.githubusercontent.com/35194820/149149134-b47f8d8b-0efa-4f8d-ba44-0fd0593e3ffe.png)

  - display
  - flex-direction
  - flex-wrap
  - flex-flow
  - justify-content
  - align-items
  - align-content

- item에 지정되어 있는 속성들

  ![item](https://user-images.githubusercontent.com/35194820/149149626-04c2031e-a932-47d5-a56d-3ffcf9d89f7c.png)

  - order
  - flex-grow
  - flex-shirink
  - flex
  - align-self

### 💡Grid

CSS 그리드 레이아웃(Grid Layout)은 페이지를 여러 주요 영역으로 나누거나, 크기와 위치 및 문서 계층 구조 관점에서 HTML 기본 요소로 작성된 콘트롤 간의 관계를 정의하는데 아주 탁월하다.

테이블과 마찬가지로 그리드 레이아웃은 세로 열과 가로 행을 기준으로 요소를 정렬할 수 있다. 하지만, 테이블과 달리 CSS 그리드는 다양한 레이아웃을 훨씬 더 쉽게 구현할 수 있다. 예를 들어, 그리드 컨테이너 속 자식 요소를, 마치 CSS로 일일이 위치를 지정해 준 것처럼, 실제로 겹치게 층을 지면서 자리를 잡도록 각 요소의 위치를 지정해 줄 수도 있다.





## Reference

- [[tistory, 개발자 아저씨들 힘을모아] [CSS] display란? / display 속성 / display 종류](https://programming119.tistory.com/97)

- [[TCP SCHOOL.com] 디스플레이](https://tcpschool.com/css/css_position_display)

- [[apost.dev] CSS 레이아웃 속성 기초2 - 디스플레이(Display) 속성을 이해하자.](https://apost.dev/1054/)

- [[velog, sukong.log] [CSS] display  속성](https://velog.io/@sukong/CSS-display-%EC%86%8D%EC%84%B1)

- [[mdn] Flexbox](https://developer.mozilla.org/ko/docs/Learn/CSS/CSS_layout/Flexbox)
- [[Origogi.github.io][CSS] Flexbox 정리](https://origogi.github.io/html/CSS-Flexbox/)

- [[mdn] CSS 그리드 레이아웃](https://developer.mozilla.org/ko/docs/Web/CSS/CSS_grid_layout)