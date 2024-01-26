## SVG (Scalable Vector Graphics)
SVG는 각 위치 값을 표시하는 벡터와 같은 방식의 2차원 그래픽용 XML 기반의 형식.
어떤 디바이스 크기에도 깨지지 않고, 마크업 언어로 이루어져 css와 javascript로 수정이 가능하다.

### SVG 사용법
1. `<img />` 요소로 넣는다.
2. CSS `background`로 넣는다.
3. 인라인으로 구현: SVG 코드를 그대로 HTML 안에 넣는다.
4. `<object>` 요소로 넣어 내부 요소에 접근할 수 있다.
```html
<object data="./image.svg" type="image/svg+xml" />
```

### SVG 장점
- SVG는 벡터 그래픽이기 때문에, 크기에 상관 없이 항상 해상도를 유지한다. 특정 브라우저나 다양한 위치에 맞게 크기를 조정해도 품질이 저하되지 않는다.
- 기본 SVG 파일의 크기가, 많은 컬러 픽셀로 생성되는 래스터 이미지(PNG 등)보다는 크기가 작다.
- SVG는 텍스트를 디자인이 아닌 텍스트 그대로 처리하기 때문에, 스크린 리더가 SVG 이미지에 포함된 모든 단어를 스캔할 수 있어 웹 페이지를 읽을 때 유용하다. 검색엔진 또한 SVG 이미지 텍스트를 읽을 수 있다.

### SVG 단점
- SVG는 로고, 일러스트, 차트 등 웹 그래픽에 적합하나, 고품질 디지털 사진에 사용할 수 없다.
- 구성요소가 복잡해지면, 파일의 크기가 급격하게 커질 수 있다.

## 그 외 파일 타입
| type | File format | File extension(s) | Summary |
| ---- | ---- | ---- | ---- |
| GIF | Graphics Interchange Format | .git | - 래스터 포맷, 무손실 압축<br>- 8비트 256색상과 단일 투명색 지원<br>- 여러 프레임을 데이터로 가질 수 있음(애니메이션 가능) |
| JPEG | Joint Photographic Expert Group image | .jpg, .jpeg, .jfif, .pjpeg, .pjp | - 래스터 포맷, 손실 압축<br>- 압축률이 높을 경우, 이미지 손실 발생<br>- 동일한 이미지를 여러번 편집/저장시 점점 퀄리 저하<br>- 가장 많이 사용하는 포맷 |
| PNG | Portable Network Graphics | .png | - 래스터 포맷, 무손실 압축<br>- GIF를 대체하기 위해 만들어짐.<br>- 8비트(투명도 옵션 존재), 24비트 트루컬러(1600만 색), 48비트 트루컬러, 알파채널(옵셔널) 지원<br>- 애니메이션은 APNG 포맷으로 가능 |

<br/>

## 이미지 태그
### `<picture>` 태그
`<img>` 요소의 다중 이미지 리소스(multiple image resources)를 위한 컨테이너를 정의할 때 사용한다.

`<picture>` 요소는 뷰포트의 너비에 따라 커지거나 작아지는 하나의 이미지를 사용하는 대신, **서로 다른 디스플레이나 기기에서 해당 뷰포트에 알맞게 채워질 수 있도록 여러 개의 이미지 중에서 적절한 이미지를 선택하여 사용**할 수 있도록 한다.

`<picture>` 요소는 0개 이상의 `<source>` 요소와 하나의 `<img>` 요소로 구성되며, 브라우저는 `<source>` 요소 중에서 해당 뷰포트와 가장 잘 어울리는 `<source>` 요소를 선택한다.

브라우저는 `<source>` 요소들의 속성값을 각각 확인하며 조건을 만족하는 첫 번재 `<source>`를 사용하고, 나머지 `<source>` 요소는 무시한다. 이 때, `<img>`요소는 `<picture>` 요소의 자식 요소 중에 가장 마지막에 위치해야 한다. `<img>` 요소는 `<picture>`요소를 지원하지 않는 브라우저를 위한 하위 호환성을 위해 사용되거나, 명시된 `<source>` 요소가 모드 조건을 만족하지 못할 경우 상용된다.

```html
<picture>
	<source media="(min-width: 700px)" srcset="/examples/images/people_700.jpg">
	<source media="(min-width: 400px)" srcset="/examples/images/people_400.jpg">
	<img src="/examples/images/people_200.jpg" alt="People">
</picture>
```

<br/>

## 이미지 최적화
이미지 최적화란, 좋은 품질의 이미지를 제공하는 동시에 가능한 가장 작은 크기를 유지하는 과정.

1. 사용자 경험 향상
	1. 콘텐츠를 더 빠르게 다운로드하여 렌더링하여, 최적의 사용자 경험을 제공한다.
2. 더 나은 페이지 성능
	1. 웹 페이지 바이트를 절약할 수 있다.
	2. 브라우저가 다운로드해야 하는 바이트가 줄어든다.
3. 향상된 가시성(visibility)
	1. 최적화된 이미지는 검색엔진에 맥락을 제공하고, 시각적 콘텐츠의 접근성을 높인다. 이에 따라 구글의 이미지 검색에서 이미지가 검색 결과의 상단에 노출될 수 있도록 보장한다. 

### 이미지 최적화 방법론
#### 1. 이미지 폭 조절
규격 4032 × 3024 이미지를 800 × 600 규격으로 줄인 결과 용량 차이:
- 4032 × 3024 : 2.7MB
- 800 × 600 : 133KB
이미지의 크기를 줄인 결과, 20배 정도 용량이 줄어듦.

#### 2. 이미지 포맷 설정
이미지 종류에 맞게 포맷을 설정하여 이미지를 최적화할 수 있다. 

#### 3. 이미지 고정값
치수가 없는 이미지들은 리플로우를 발생시켜 퍼포먼스를 저하시킨다.
이를 해결하기 위해 이미지 및 비디오 요소에 `width`와 `height` 속성을 항상 포함하거나, CSS를 사용하여 필요한 공간 `aspect-ratio`를 잡는다.
```html
<img src="puppy.jpg" width="640" height="360" alt="Puppy with balloons" />
```

#### 4. 여러 버전의 이미지 제공
여러 버전의 이미지를 지정하면 브라우저에서 사용하기에 가장 적합한 버전을 선택한다.
```html
<!-- Before -->
<img src="flower-large.jpg" />

<!-- After -->
<img src="flower-large.jpg" srcset="flower-small.jpg 480w, flower-large.jpg 1080w" sizes="50vw" />
```

- `src`
브라우저가 `srcset`과 `sizes` 속성을 지원하지 않으면 fallback으로 `src` 속성이 동작한다.
- `srcset` 
이미지 파일명과 width, 또는 density 설명을 쉼표로 구분한다.
- `size`
이미지가 표시될 때의 너비를 브라우저에게 전달한다. `size` 는 display 크기에 영향을 주지 않으며, CSS 가 필요하다.

#### 5. 이미지 크기 조절 툴
sharp npm package, ImageMagick CLI tool 등

#### 6. Image CDNs
Image CDN은 이미지 변환, 최적화 및 전송을 전문으로 한다. 또한 사이트에서 사용되는 이미지에 대한 접근이나 조작을 위한 API로 생각할 수 있다.
Image CDN에서 로드된 이미지의 경우 이미지 URL은 이미지 뿐만 아니라 크기, 포맷, 품질 같은 매개변수도 나타낸다. 

#### 7. CSS Sprite
웹 페이지에 필수적으로 자주 사용되는 아이콘, 버튼 같은 이미지들을 쓸 때마다 불러오는 것이 아니라, 한 이미지 파일로 통합한 후 배경 이미지로 만들어 놓고, `position` 값으로 각각의 이미지를 불러온다.
이를 통해 트래픽을 절약하고, HTTP 요청 횟수를 줄여 빠른 웹 브라우징을 할 수 있다.

```css
/* 한 이미지를 불러옴 */ 
div#sprite {
	background: url('/images/sprite.png') no-repeat;
} 

/* position으로 각 이미지를 불러옴  */
div#sprite > .first {
	background-position: 0 0;
}

div#sprite >.second {
	background-position: 0 -15px;
}

div#sprite >.third {
	background-position: 0 -30px;
}

```

#### 8. Lazy Loading
Lazy Loading이란, 페이지를 로드할 때 모든 이미지를 로드하는 것이 아니라, 중요하지 않은 자원 또는 당장 필요 없는 자원인 경우 서버에 요청을 미루고, 필요한 경우 해당 자원을 요청 받는 방법이다.

1. 데이터의 낭비를 막을 수 있다.
2. 브라우저의 렌더링 시간을 줄여준다.



---
**Reference**

[SVG 파일](https://www.adobe.com/kr/creativecloud/file-types/image/vector/svg-file.html)

[SVG 사용법 정리](https://shiwoo.dev/posts/svg-usage-summary)

[웹 성능을 위한 이미지 최적화](https://velog.io/@hustle-dev/%EC%9B%B9-%EC%84%B1%EB%8A%A5%EC%9D%84-%EC%9C%84%ED%95%9C-%EC%9D%B4%EB%AF%B8%EC%A7%80-%EC%B5%9C%EC%A0%81%ED%99%94)

[HTML `<picture>` 태그](https://tcpschool.com/html-tags/picture)


