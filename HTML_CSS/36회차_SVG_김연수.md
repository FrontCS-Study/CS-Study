[TOC]

## 이미지 포맷 종류

- GIF
  - 여러장의 이미지로 만들어진 애니메이션 표현 가능하다.
  - 단, 지원되는 컬러범위가 적어 화질이 조금 떨어진다.
  - 무손실 인덱스 색상 이미지에는 PNG, 애니메이션 시퀀스에는 WebP, AVIF, APNG가 더 좋다.
  - **지원**: 대부분의 브라우저
- JPEG
  - 정적 이미지의 손실 압축 저장에 적합하다.
  - 파일의 색을 단순화함으로써 압축을 할 수 있으나, 이미지 손실이 일어날 수 있다.
  - 압축 시 크기와 이미지의 품질 중에 선택하게 된다.
  - 이미지의 정확한 재현이 필요하면 PNG, 정확한 재현에 더 높은 압축률까지 필요하면 WebP의 사용을 고려
  - **지원**: 대부분의 브라우저
- PNG
  - JPEG와 비교했을 때 이미지의 정확한 재현이 중요하거나 투명도를 지원하는 형식이 필요할 때 적합하다.
  - 압축에 유리하다.
  - WebP와 AVIF가 더 높은 압축률을 지원하지만 PNG는 대부분의 브라우저에서 지원한다.
  - **지원**: 대부분의 브라우저
- SVG
  - 벡터로 이루어져 있기 때문에, 이미지의 크기를 정하는데 있어서 크고 늘리는데 손실이 없다.
  - 파일 사이즈가 굉장히 작다.
  - 다양한 크기로 정확하게 그려야 하는 아이콘, 다이어그램 등에 사용된다.
  - **지원**: 대부분의 브라우저
- WebP
  - 구글이 만든 이미지 포맷으로 GIF, JPG, PNG 포맷을 대체하기 위해 개발되었다.
  - 이미지 품질이 같을 때 WebP 파일의 크기가 다른 포맷의 파일에 비해 훨씬 작다.
  - 품질, 압축률 등이 훨씬 우수하나 지원 브라우저가 제한적이다.
  - **지원**: Chrome, Edge, Firefox, Opera, Safari
- APNG
  - 무손실 애니메이션 시퀀스
  - 성능면에서는 AVIF와 WebP가 좋지만 더 많은 브라우저가 APNG를 지원한다.
  - **지원**: Chrome, Edge, Firefox, Opera, Safari
- AVIF
  - 이미지와 애니메이션 시퀀스에 모두 탁월
  - PNG, JPEG보다 훨씬 뛰어난 압축률을 보유하고도 넓은 색상 범위, 애니메이션 프레임, 투명도도 지원
  - AVIF를 사용할 땐 미지원 브라우저를 대비하여 [``](https://sorto.me/docs/Web/HTML/Element/picture) 요소 등 대체 이미지 형식을 제공
  - **지원**: Chrome, Opera, Firefox (설정 필요, 애니메이션 등 고급 기능은 미구현)

<br>

> 🔍 **SVG 태그**
>
> SVG태그는 Scalable Vector Graphics의 약자로 벡터 기반 그래픽을 XML 형식으로 정의하는 것을 의미한다.  
> SVG태그 내부에는 원, 사각형, 다각형, 라인, path 등을 정의 할 수 있다. 
>
> ```html
> <svg width="가로영역" height="세로영역">
> 	SVG그래픽
>     ...
> </svg>
> ```
>
> | 모양   | 태그        | 모양   | 태그         |
> | ------ | ----------- | ------ | ------------ |
> | 사각형 | `<rect>`    | 다각선 | `<polyline>` |
> | 정원   | `<circle>`  | 다각형 | `<polygon>`  |
> | 타원   | `<ellipse>` | 패스   | `<path>`     |
> | 선     | `<line>`    |        |              |

## picture 태그

`<picture>` 태그는 HTML5에 추가된 요소로  `<img>` 요소의 다중 이미지 리소스(multiple image resources)를 위한 컨테이너를 정의할 때 사용된다.

picture 태그는 css없이 반응형 디자인에서 주로 사용한다.

```html
<picture>   
    <source media="(min-width: 700px)" srcset="/examples/images/people_960.jpg">   
    <source media="(min-width: 400px)" srcset="/examples/images/people_575.jpg">   
    <img src="/examples/images/people_200.jpg" alt="People"> 
</picture>
```

`<picture>` 요소는 0개 이상의 `<source>` 요소와 하나의 `<img>` 요소로 구성된다.  

브라우저는 `<source>` 요소들의 속성 값을 각각 확인해 나가며 조건을 만족하는 첫 번째 `<source>` 요소를 사용하고, 나머지 `<source>` 요소들은 무시한다. 이러한 `<img>` 요소는 `<picture>` 요소를 지원하지 않는 브라우저를 위한 하위 호환성을 위해 사용되거나 명시된 `<source>` 요소가 모두 조건을 만족하지 못 할 경우 사용된다.

> 💻 **실제 사용 경험**
>
> - 랜딩 백그라운드 이미지를 최적화하고자 WebP 형식 도입했다.  
>   WebP형식을 지원하지 않은 브라우저에는 호환성을 위해서 svg 형식 이미지를 보이게 코드를 작성했다.
> - 아쉬운 점 : WebP형식 이미지 사이즈를 여러 개 가지고 반응형 디자인을 추가하는게 렌더링 속도에 좋을 것 같다. 
>
> ```html
> <picture>
>    <source srcSet={backgroundImageWebP} type="image/webp" />
>    <img className={styles.landing_backgroundImg} src={backgroundImageSvg} alt="ShadowMate플래너이미지" />
> </picture>
> ```

<br>

## 이미지 최적화

웹 페이지에서 대부분의 용량을 차지하는 것은 이미지이다.  
이미지 최적화를 통해 이미지 용량을 줄인다면, 다음과 같은 이점을 얻을 수 있다.

1. 웹페이지 바이트를 절약한다. 브라우저가 다운로드하는 바이트가 줄어든다.
2. 콘텐츠를 더 빨리 다운로드하여 화면에 렌더링 한다. (로딩이 3초가 넘어가면 절반의 사용자가 떠난다고 한다.)
3. 서버 저장 공간이 적게 사용하여, 서버 비용이 절감된다.
4. 구글의 SEO 순위를 결정할 때, 모바일 응답성을 고려하여, 검색 순위에 노출한다.

### 1. 최적화된 이미지 포맷을 사용하기

 이미지의 용도에 따라 적절한 확장자를 사용해야 한다.

- SVG : 복잡도가 작은 로고 또는 아이콘 이미지
- PNG : 투명한 배경이 필요한 이미지, 만들어진 이미지
- WEBP : 압축 손실률이 작고 압축률이 높은 차세대 이미지
  - 구형 safari나 익스플로러는 제대로 지원을 하지 않아서 대체 이미지 사용 권장
  - Next.js에서는 간단한 config 설정으로 이미지의 포맷을 변환할 수 있으나, React의 경우 변환 기능이 없어서 해당 포맷의 파일을 직접 추가해야 한다.
- JPG : 위 3개를 제외한 케이스 혹은 대체 이미지용으로 사용, 카메라로 찍은 사진

> 💡 **JPEG vs WebP vs AVIF**
>
> PNG, JPEG를 압축했을 때, 최종적으로 JPEG > WebP > AVIF 형식으로 압축률이 좋았다고 한다.  
> 브라우저가 AVIF를 지원하면 AVIF를 사용하고, 그렇지 않은 경우 WebP, 그렇지 않은 경우 PNG 또는 JPEG의 사용할 수 있다.

### 2. 이미지 고정값

Reflow는 DOM요소의 크기나 위치를 변경하면 해당 노드의 위치와 크기뿐만 아니라 연관된 요소들의 위치와 크기도 재계산하기 때문에 브라우저에 퍼포먼스를 저하시키는 요인이 된다.

치수가 없는 이미지들은 Reflow를 발생시켜 퍼포먼스를 저하시키기 때문에 이를 해결하기 위해 이미지 및 비디오 요소에 `width`와 `height`속성을 항상 포함한다. lighthouse로 분석 가이드에서도 항상 추천되는 방식이다.

```html
<img src="shadow.jpg" width="640" height="360" alt="ShadowMate background" />
```

반응형을 위해 css를 사용하여 auto로 이미지 크기를 조절할 경우, 브라우저가 다운 후에 크기를 결정하기 때문에 reflow가 발생할 수 있다.  
이것을 방지하기 위해 `aspect-ratio`를 사용한다. ( `aspect-ratio`는 `width/height` 비율을 지정할 수 있다. width : height 비율)

### 3. 반응형 이미지 제공

- **이미지 태그 속성 이용 하기 (sizes 사용하기)**

  - srcset  : 이미지 파일명과 width 또는 density 설명을 쉼표로 구분한다.
    - `400w` 는 400px 임을 브라우저에게 너비를 알려준다.
    - *권장하지 않은 방법이다...!*  
      _srcset 속성은 의도대로 제대로 동작하는 웹 브라우저는 Firefox 뿐이다. 다른 브라우저는 화면을 재랜더링 후 적용된다._

  - sizes : 이미지가 표시될 때의 너비를 브라우저에게 알려준다. 단, 단위에 % 는 사용할 수 없다.

  - src : 브라우저가 `srcset` 과 `sizes` 속성을 지원하지 않으면 fall back으로 `src` 속성이 동작한다.
    - `src` 속성은 모든 디바이스 크기에서 동작할 수 있을만큼 충분히 커야한다.

  ```html
  <img
    srcset="small.webp 400w, 
            medium.webp 900w"
    sizes="(max-width: 400px) 95vw,
           (max-width: 900px) 50vw"
    src="large.webp"
  />
  ```

- **picture 태그**

  srcset 속성은 의도대로 제대로 동작하는 웹 브라우저는 Firefox 뿐이라고 한다. 다른 브라우저는 화면을 재랜더링 후 적용된다.  
  그래서 `<picture>` 태그가 나오게 되었고, `<picture>` 태그에서 부터는 동적 이미지 파일 재 로딩이 대부분의 모던 웹 브라우저들에서 지원된다.

### 4. Lazy loading

Lazy loading은 페이지를 로드할 때, 중요도가 떨어지거나 화면에 보이지 않는 자원들의 서버 요청을 미루고, 필요한 경우에 해당 자원을 요청 받는 방법을 말한다.

레이지 로딩을 사용하면 **최초 페이지 렌더링 시간을 줄여주고**, 당장 화면에 표시되지 않은 이미지, 영상 등의 리소스를 나중에 로딩하면서 **최초 데이터 전달 양을 감소**시킨다.

<br>

> 💻 **프리온보딩 성능 최적화 특강 - 강사님의 이미지 최적화 방법**
>
> 포맷 형식은 WebP로 반응형으로 이미지를 여러개 가지고 있는게 좋다.  
> WebP만 써도 기존 이미지 용량에 3분의 1은 줄어진다.  
> 이미지 크기는, 품질을 위해서라도 크기에 2~3배정도 더 큰 이미지를 가지고 있고,  
> 보여지는건 width, height를 이용해서 조절해야 한다.  

<br>

---

**참고**

[HTML : 이미지 삽입 요소 - sorto.me docs](https://sorto.me/docs/Web/HTML/Element/img)

[[HTML] 이미지 태그 활용과 그래픽 - 깨작코딩 - 티스토리](https://siloam72761.tistory.com/7)

[[TCP School] - HTML  태그](https://tcpschool.com/html-tags/picture)

[[HTML기초문법] 13강 SVG태그 및 이미지 활용 - OSSAM강좌](https://ossam5.tistory.com/112)

[웹 성능을 위한 이미지 최적화](https://velog.io/@hustle-dev/웹-성능을-위한-이미지-최적화)

[[웹 최적화] 이미지 로딩 최적화(Image loading Optimzation)](https://ryuhojin.tistory.com/38)
