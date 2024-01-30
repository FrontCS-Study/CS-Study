
## display 속성

### 1. none
- 요소를 렌더링하지 않도록 설정한다.
- `visibility` 속성을 `hidden`으로 설정한 것과 달리, 영역도 차지하지 않는다.

### 2. block
- 기본적으로 가로 영역을 모두 채우며, block 요소 다음에 등장하는 태그는 줄바꿈이 된 것처럼 보인다.
- `width`, `height` 속성을 지정할 수 있으며, block 요소 뒤에 등장하는 태그가 그 이전 block 요소에 배치될 수 있어도 항상 다음 줄에 렌더링 된다.
- **div, p, h, li 태그가 해당된다.**

### 3. inline
- block 과 달리 줄바꿈이 되지 않고, `width`와 `height`를 지정할 수 없다. 또한 **`margin`과 `padding` 속성은 좌우 간격만 반영이 되고, 상하 간격은 반영되지 않는다.**
- display 속성 값을 inline -> block으로 변경했더라도, 변경된 요소는 내부에 다른 요소를 포함할 수 없다. - 처음부터 display 속성이 block인 요소만이 내부에 다른 요소를 포함할 수 있다.
- **span, b, i, a 태그가 해당된다.**
- inline 요소는 height 값을 가지지 않기 때문에, layout 배치 시 여백이 요소의 크기로 적용되지 못한다.

![span태그에 padding을 줬을 때](https://i.imgur.com/aA1F7UY.png)

![padding 준 span 위아래로 div 태그가 있을 때](https://i.imgur.com/g87xniJ.png)

### 4. inline-block
- inline-block으로 설정된 요소 자체는 inline처럼 동작하나, 해당 요소 내부에서는 block 요소처럼 동작한다.
- 줄바꿈이 되지 않지만, 크기(`width`, `height`, `margin`)를 지정할 수 있다. 크기를 지정하지 않을 경우, inline과 같이 컨텐츠만큼 영역이 잡힌다.
- IE7 이하에서 사용할 수 없다.

<br>

## display: flex
```html
<div class="container">
	<div class="item">flex item</div>
	<div class="item">flex item</div>
	<div class="item">flex item</div>
</div>
```
부모 div.container: **Flex Container**
자식 div.item: **Flex Item**

### 1. Flex Container에 적용되는 속성
#### 1. display: flex
```css
.container {
	display: flex;
}
```
Flex Item들은 가로 방향으로 배치되고, 자신이 가진 컨텐츠의 width 만큼만 차지한다. height는 컨테이너의 높이만큼 늘어난다.

#### 2. flex-direction
아이템이 배치되는 축의 방향을 결정하는 속성.
```css
.container {
	flex-direction: row; /* 아이템이 행(가로) 방향으로 배치된다. */
	flex-direction: column; /* 아이템이 행(가로) 역순 방향으로 배치된다. */
	flex-direction: row-reverse; /* 아이템이 열(세로) 방향으로 배치된다. */
	flex-direction: column-reverse; /* 아이템이 열(세로) 역순 방향으로 배치된다. */
}
```

#### 3. flex-wrap
컨테이너가 아이템을 한 줄에 담을 여유 공간이 없을 때, 아이템 줄바꿈을 결정하는 속성.
```css
.container {
	flex-wrap: nowrap; /* 줄바꿈을 하지 않는다. */
	flex-wrap: wrap;   /* 줄바꿈을 한다. */
	flex-wrap: wrap-reverse;  /* 줄바꿈을 하되, 아이템을 역순으로 배치한다. */
}
```

#### 4. flex-flow
flex-direction과 flex-wrap을 한꺼번에 지정한다.
```css
.container {
	flex-flow: row wrap;
	/* 아래의 두 줄을 줄여 쓴 것 */ 
	flex-direction: row;
	flex-wrap: wrap;
}
```

#### 5. justify-content: 메인축 방향 결정
```css
.container {
	justify-content: flex-start;    /* 기본값. 아이템을 시작점으로 정렬한다. */ 
	justify-content: flex-end;      /* 아이템을 끝점으로 정렬한다. */ 
	justify-content: center;        /* 아이템을 가운데로 정렬한다. */ 
	justify-content: space-between; /* 아이템들의 사이에 균일한 간격을 만든다. */ 
	justify-content: space-around;  /* 아이템들의 둘레에 균일한 간격을 만든다. */ 
	justify-content: space-evenly;  /* 아이템들의 사이와 양끝에 균일한 간격을 만든다. */  
}
```

#### 6. align-items: 수직축 방향 결정
```css
.cotainer {
	align-items: stretch;     /* 기본값. 아이템이 수직축 방향으로 늘어나 차지한다. */ 
	align-items: flex-start;  /* 아이템을 시작점으로 정렬한다. */ 
	align-items: flex-end;    /* 아이템을 끝점으로 정렬한다. */ 
	align-items: center;      /* 아이템을 가운데로 정렬한다. */ 
	align-items: baseline;    /* 아이템을 텍스트 베이스라인 기준으로 정렬한다. */ 
}
```

#### 7. align-content: 여러 행 정렬
`flex-wrap: wrap` 이 설정된 상태에서, 아이템들의 행이 2줄 이상 되었을 때 수직축 방향 정렬을 결정한다.
```css
.container {
	flex-wrap: wrap;             
	align-content: stretch;
	align-content: flex-start;
	align-content: flex-end;
	align-content: center;
	align-content: space-between;
	align-content: space-around;
	align-content: space-evenly;
}
```

### 2. Flex Item에 적용되는 속성
#### 1. flex-basis
- Flex Item의 기본 크기를 설정한다. (flex-direction이 row일 때는 너비, column일 때는 높이)
- flex-basis의 값으로 `width`, `height` 등에 사용하는 단위의 수가 들어갈 수 있다.
- 기본값 auto는 해당 아이템의 `width` 값을 사용한다. `width`를 따로 설정하지 않으면 컨텐츠의 크기.
```css
.item {
	flex-basis: auto; /* 기본값 */
}
```

#### 2. flex-grow
- 아이템이 flex-basis의 값보다 커질 수 있을지를 결정한다.
- flex-grow의 값이 0보다 크면, 해당 아이템이 flexible 박스로 변하고, 원래의 크기보다 커지며 빈 공간을 메우게 된다.
```css
.item {
	flex-grow: 0; /* 기본값 */
	flex-grow: 1;
}
```

#### 3. flex-shrink
- flex-grow와 쌍을 이루는 속성으로, 아이템이 flex-basis의 값보다 작아질 수 있는지를 결정한다.
- flex-shrink의 값이 0보다 크면, 해당 아이템이 flexible 박스로 변하고, flex-basis보다 작아진다.
```css
.item {
	flex-basis: 150px;
	flex-shrink: 1; /* 기본값 */
}
```

#### 4. flex
flex-grow, flex-shrink, flex-basis를 한 번에 쓸 수 있는 축약형 속성.
```css
.item {
	flex: 1;        /* flex-grow: 1; flex-shrink: 1; flex-basis: 0%; */
	flex: 1 1 auto; /* flex-grow: 1; flex-shrink: 1; flex-basis: auto; */
	flex: 1 500px;  /* flex-grow: 1; flex-shrink: 1; flex-basis: 500px; */
}
```

#### 5. align-self: 수직축으로 아이템 정렬
- align-items가 전체 아이템의 수직축 방향 정렬이라면, align-self는 해당 아이템의 수직축 방향 정렬. 
- align-items보다 align-self가 우선권을 갖는다.
```css
.item {
	align-self: auto; /* 기본값 */
	align-self: stretch;
	align-self: flex-start;
	align-self: flex-end;
	align-self: center;
	align-self: baseline;
}
```

#### 6. order: 배치 순서
- 각 아이템의 시각적 나열 순서를 결정한다.
- 숫자값이 들어가며, 작은 숫자일수록 먼저 배치된다.
```css
.item:nth-child(1) { order: 3; } /* A */
.item:nth-child(2) { order: 1; } /* B */
.item:nth-child(3) { order: 2; } /* C */
/* B C A 로 나열된다. */
```

#### 7. z-index
- z-index로 Z축 정렬을 할 수 있다. 
- 숫자값이 들어가며, 큰 숫자일수록 위로 올라온다.
```css
.item:nth-child(2) {
	z-index: 1;
	transform: scale(2);
}
```

<br>

## display: grid
```html
<div class="container">
	<div class="item">A</div>
	<div class="item">B</div>
	<div class="item">C</div>
</div>
```
부모 div.container: **Grid Container**
자식 div.item: **Grid Item**

### 1. Grid Container에 적용되는 속성
#### 1. display: grid
```css
.container {
	display: grid;
}
```

#### 2. grid-template-row, grid-template-columns: 그리드 형태 정의
- Container에 Grid 트랙의 크기를 지정하는 속성. 여러가지 단위를 사용할 수 있다.
- **fr (fraction)**: 숫자 비율대로 트랙의 크기를 나눈다. 
- **repeat()**: 반복되는 값을 자동으로 처리할 수 있는 함수. 
	- repeat(반복횟수, 반복값)
- **minmax()**: 최솟값과 최댓값을 지정할 수 있는 함수
	- minmax(100px, auto) : 최소한 100px, 최대는 자동으로 늘어남.
- **auto-fill, auto-fit**: column의 개수를 미리 정하지 않고, 설정된 너비가 허용하는 한 최대한 셀을 채운다.
```css
.container {
	grid-template-columns: 200px 200px 500px;
	grid-template-rows: 200px 200px 500px;
}
```

#### 3. row-gap, column-gap, gap: 간격 설정
```css
.container {
	row-gap: 10px;    /* row의 간격을 10px로 설정 */
	column-gap: 20px; /* column의 간격을 20px로 설정 */

	gap: 10px 20px; /* row-gap: 10px; column-gap: 20px; */
	gap: 20px;      /* row-gap: 20px; column-gap: 20px; */
}
```

#### 4. grid-auto-rows, grid-auto-columns: 그리드 형태를 자동으로 정의
grid-template-columns(또는 grid-template-rows)의 통제를 벗어난 위치에 있는 트랙의 크기를 지정한다.
```css
.container {
	grid-auto-rows: minmax(100px, auto);
}
```

#### 5. grid-column-start, grid-colum-end, grid-column, grid-row-start, grid-row-end, grid-row: 각 셀의 영역 지정
- grid-...-start: 시작 번호
- grid-...-end: 끝 번호
- grid-column, grid-row: start와 end 속성을 한번에 쓰는 축약형
![각 셀의 영역 지정](https://i.imgur.com/AIDB7f8.png)

```css
/* 두 코드는 같은 영역을 지정한다. */
.item:nth-child(1) {
	grid-column-start: 1;
	grid-column-end: 3;
	grid-row-start: 1;
	grid-row-end: 2;

	grid-column: 1 / 3;
	grid-row: 1 / 2; 
}
```

#### 6. grid-template-areas: 영역 이름으로 그리드 정의
각 영역(grid area)에 이름을 붙이고, 그 이름을 이용해서 배치한다.
```css
.container {
	grid-template-areas:
		"header header header"
		"  a     main    b   "
		"  .      .      .   "
		"footer footer footer"
}
```

각 영역의 이름을 매칭할 때는 해당 아이템 요소에 grid-area 속성으로 이름을 지정한다.
```css
.header { grid-area: header; }
.sidebar-a { grid-area: a; }
.main-content { grid-area: main; }
.sidebar-b { grid-area: b; }
.footer { grid-area: footer; }
```

#### 7. grid-auto-flow: 자동 배치
- 아이템이 자동 배치되는 흐름을 결정한다.
- grid 배치의 기본 설정은 아이템이 `row`를 기준으로 순서대로 배치가 되다가, 들어갈 자리가 없으면 그 칸은 비워두고 아래로 배치된다.
- `dense`는 기본적으로 빈 셀을 채우는 알고리즘이며, `row`와 `column`에 따라 기준이 달라진다.
```css
.container {
	display: grid;
	grid-template-columns: repeat(auto-fill, minmax(25%, auto));
	grid-template-rows: repeat(5, minmax(50px, auto));
	grid-auto-flow: dense;
}
.item:nth-child(2) { grid-column: auto / span 3; }
.item:nth-child(5) { grid-column: auto / span 3; }
.item:nth-child(7) { grid-column: auto / span 2; }
```

#### 8. align-items: 세로 방향 정렬
아이템을 세로(column축) 방향으로 정렬한다.
```css
.container {
	align-items: stretch;
	align-items: start;
	align-items: center;
	align-items: end;
}
```

#### 9. justify-items: 가로 방향 정렬
아이템을 가로(row축) 방향으로 정렬한다.
```css
.container {
	justify-items: stretch;
	justify-items: start;
	justify-items: center;
	justify-items: end;
}
```

#### 10. place-items
- align-items와 justify-items를 같이 쓸 수 있는 단축 속성.
- align-items, justify-items 순서로 작성하고, 하나의 값만 쓰면 두 속성 모두에 적용된다.
```css
.container {
	place-items: center start;
}
```

#### 11. align-content: 아이템 그룹 세로 정렬
grid item들의 높이를 모두 합한 값이 grid container의 높이보다 작을 때, grid item을 통째로 정렬한다.
```css
.container {
	align-content: stretch;
	align-content: start;
	align-content: center;
	align-content: end;
	align-content: space-between;
	align-content: space-around;
	align-content: space-evenly;
}
```

#### 12. justify-content: 아이템 그룹 가로 정렬
grid item들의 너비를 모두 합한 값이 grid container의 너비보다 작을 때, grid item들을 통째로 정렬한다.
```css
.container {
	justify-content: stretch;
	justify-content: start;
	justify-content: center;
	justify-content: end;
	justify-content: space-between;
	justify-content: space-around;
	justify-content: space-evenly;
}
```

#### 13. place-content
- align-content와 justify-content를 같이 쓸 수 있는 단축 속성.
- align-content, justify-content의 순서로 작성하고, 하나의 값만 쓰면 두 속성 모두에 적용된다.
```css
.container {
	place-content: space-between center;
}
```

### 2. Grid Item에 적용되는 속성
#### 1. align-self: 개별 아이템 세로 정렬
해당 아이템을 세로(column축) 방향으로 정렬.
```css
.item {
	align-self: stretch;
	align-self: start;
	align-self: center;
	align-self: end;
}
```

#### 2. justify-self: 개별 아이템 가로 정렬
해당 아이템을 가로(row축) 방향으로 정렬.
```css
.item {
	justify-self: stretch;
	justify-self: start;
	justify-self: center;
	justify-self: end;
}
```

#### 3. place-self
- align-self와 justify-self를 같이 쓸 수 있는 단축 속성.
- align-self, justify-self의 순서로 작성하고, 하나의 값만 쓰면 두 속성 모두에 적용된다.
```css
.item {
	place-self: start center;
}
```

#### 4. order: 배치 순서
각 아이템들의 시각적 나열 순서를 결정한다.
```css
.item:nth-child(1) { order: 3; } /* A */ 
.item:nth-child(2) { order: 1; } /* B */ 
.item:nth-child(3) { order: 2; } /* C */
/* B C A 순서로 나열됨.  */
```

#### 5. z-index
z-index로 Z축 정렬을 할 수 있다. 숫자가 클 수록 위로 올라온다.
```css
.item:nth-child(5) {
	z-index: 1;
	transform: scale(2);
}
```


---
**Reference**

[[ofcourse] display 속성](https://ofcourse.kr/css-course/display-%EC%86%8D%EC%84%B1)

[inline요소는 정말 상하 padding이 적용되지 않을까?](https://hoya-kim.github.io/2021/08/25/padding-on-inline-element/)

[[TCPschool] 디스플레이](https://tcpschool.com/css/css_position_display)

[이번에야말로 CSS Flex를 익혀보자](https://studiomeal.com/archives/197)

[이번에야말로 CSS Grid를 익혀보자](https://studiomeal.com/archives/533)
