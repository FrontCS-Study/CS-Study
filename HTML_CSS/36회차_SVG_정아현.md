# SVG

### 💡 SVG 란?

Scalable Vector Graphics 의 약자로, 2차원 벡터 그래픽을 표현하기 위한 XML 기반 마크업 언어이다. SVG는 본질적으로 그래픽을 표현하며, 1999년 W3C의 주도하에 개발된 오픈 표준 벡터 그래픽 파일 형식이다. 
즉, SVG는 점과 점 사이의 계산을 통해 그리는 그래픽이다.

![RasterVsVector](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbyeDJd%2FbtqA7pxZbtv%2FFF3je5HymLTvAK70Rz1eG0%2Fimg.png)

### 👍🏻 SVG 장점

- 확대해도 절대 깨지지 않고 절대 늘어나지 않는다. 따라서 웹 그래픽의 표준으로 쓰이며, 아이콘 이미지, 그래픽 등에 널리 쓰인다.
- JPG, PNG 비트맵 이미지로 사용 안하고 SVG로 많이 쓰인다.
- 모양이 복잡하지 않을 시 파일 사이즈가 작고, 모양이 복잡할수록, 또는 그림을 구성하는 포인트가 많을수록 용량이 커지고 성능이 떨어진다. 따라서 간단한 이미지들은 벡터 그래픽인 SVG를 사용할 때 유리하다.



### 📌 SVG 특징

- XML 기반 2차원 그래픽 - 코드로 이루어져있다.
- DOM의 일부로 HTML 엘리먼트에 추가된다.
- SVG는 Adobe illustrator등의 디자인 툴로 만들어진다.
- SVGO(웹 기반 GUI SVGOMG)와 같은 도구를 이용하면 SVG 파일 크기를 30~60% 이상 쉽게 압축이 가능하다.



### 🔍 SVG 사용법

1. `<img>` 에 넣을 수 있다.

   ```html
   <img src="/resources/images/test.svg"/>
   ```

2. CSS background로 사용

   - class에 아래와 같이 css background로 넣어주고, html 태그 요소의 class로 넣어줌

   - css의 background: url('/resources/images/test.svg'); 

3. SVG를 직접 HTML에 inline으로 삽입

   - 아래처럼 html body 태그 안에 바로 삽입한다.

   ```html
   <body>
       <svg>
           <rect x="0" y="0" width="100" height="100"></rect>
       </svg>
   </body> 
   ```

4. object 태그를 이용한다.

   ```html
   <object data="/resources/images/test.svg" type="image/svg+xml"></object>
   ```




# 이미지 최적화

이미지 최적화는 실제로 우리가 서버에서 데이터를 받아올 때 불필요한 요청을 여러번 하지 않아도 되며, 서버의 저장 공간이 적게 필요로 한다. 이는 비용 절감으로도 이어지며, 유저가 서비스에 접근했을 경우 더 빠르고 쾌적한 웹 환경을 제공해준다. 또 구글 SEO 순위를 결정할 때 모바일 응답성을 고려하여, 검색 순위에 노출된다.

**🔍 최적화를 하지 않으면?**

- 이미지 로딩이 늦어지기 때문에 웹화면이 그려지는데 오랜 시간이 소요됨
- 서버에 저장공간을 많이 잡아먹기 때문에 서버 비용이 올라감
- 불필요한 요청을 계속해서 받아와 그려내야 함
- 웹 페이지 바이트가 커지며 버벅거릴 수 있음



### 💡 최적화 방법

웹 화면에 랜더링을 빠르게 하기 위해선, 이미지 리소스 최적화가 반드시 필요하다.

1. **width 값 조정하기**

   웹사이트에서 사용하는 이미지는 보통 width값 1,000px을 넘지 않는다. 블로그처럼 좌우에 메뉴바가 존재한다면 800px로도 충분하다.
   width값을 줄여주는 것만으로도 10배, 20배, 30배는 더 이미지 용량을 줄여줄 수 있다.

   ```html
   <img src="url" width="800" height="300" alt="이미지" />
   ```

2. **최적화된 이미지 포맷(format)사용**

   이미지의 종류에 맞게 포맷을 설정하면 이미지 최적화를 할 수 있다

   - JPG → 카메라로 찍은 실제 사진
   - PNG → 만들어진 이미지

   ex) 아이폰이나 카메라로 찍은 사진 (JPG), 일러스트나 아이패드로 그린 그림(PNG)

   > **JPEG, WdbP, AVIF**
   >
   > - **JPEG** : 손실이 많은 압축 디지털 이미지를 만드는 데 사용할 수 있는 압축 방법이며, 크기와 품질 사이에서 절충하기 위해 압축 정도를 선택할 수 있다.
   > - **WebP** : 구글이 발표한 포맷. 파일 크기를 줄이기 위해 손실 없는 압축과 무손실 압축을 모두 사용하고 있다. 웹사이트의 트래픽 감소 및 로딩 속도 단축을 겨냥한 것으로, 주로 사진 이미지 압축 효과가 높다.
   > - **AVIF** : 압축과 비 손실 압축을 전부 지원하기 때문에 WebP처럼 GIF, PNG, JPEG등 상용 이미지 포맷을 대체할 수 있다. 애니메이션 기능이 있어 움짤로 쓸 수 있고, 압축 효율이 뛰어나 WebP와 닮았다.
   >
   > 이 세가지를 같은 이미지로 압축 시 결과는 AVIF > WebP > PNG/JPEG 순이라고 한다.
   >
   > **브라우저가 AVIF를 지원하면 AVIF를 사용하고, 그렇지 않으면 WebP, 그렇지 않으면 PNG 혹은 JPEG를 사용하는 것이 좋다**

   아래는 HTML에서의 사용법이다.

   ```html
   <picture>
     <source srcset="supercar.avif" type="image/avif" />
     <source srcset="supercar.webp" type="image/webp" />
     <img src="supercar.jpeg" alt="Fast red car" />
   </picture>
   ```

3. **이미지 고정값**

   Reflow를 발생시키는 원인(치수가 없는 이미지) 해결법

   > Reflow?
   >
   > 어떤 액션이나 이벤트에 의해 DOM요소의 크기나 위치 등을 변경하면 해당 노드의 하위 노드와 상위의 노드들을 포함하여 Layout 단계를 다시 수행하게 된다. 변경하려는 특정 요소와 위치, 크기 뿐만 아니라 연관된 요소들의 위치와 크기도 재계산을 하기 때문에 퍼포먼스를 저하시키는 요인이다.

   치수가 없는 이미지들은 Reflow를 발생시켜 퍼포먼스를 저하시킨다.
   이를 해결하기 위해 이미지 및 비디오 요소에 width와 height 속성을 항상 포함하거나, 또는 CSS를 사용하여 필요한 공간 aspect-ratio를 잡는다. 이 방법을 사용하면 이미지가 로드되는 동안 브라우저가 문서의 공간을 올바르게 할당할 수 있다.

   ```html
   <img src="puppy.jpg" width="640" height="360" alt="Puppy with balloons" />
   ```

   반응형 웹 디자인의 경우 width와 height를 생략하고 CSS를 사용하여 이미지 크리를 조정하기 시작하는데, 이 접근 방식의 단점은 다운로드가 시작되고 브라우저가 크기를 결정할 수 있는 경우에만 이미지를 위한 공간을 할당할 수 있다는 점이다.

   이미지가 로드되어 각 이미지가 화면에 나타나면 reflow 되어 텍스트가 갑자기 화면 아래로 튀어나가는 등의 문제가 발생하였는데 이것을 방지하기 위해 aspect-ratio를 사용한다. 이 속성을 사용하면 복잡한 계산 없이 간단하게 속성으로 레이아웃 이동 방지를 할 수 있다.

4. **여러 버전의 이미지 제공**

   일반적으로 3~5개의 서로 다른 크기의 이미지를 제공하고, 더 많은 이미지 크기를 제공하면 성능 향상은 되지만 그만큼 서버에서 더 많은 공간을 차지하고 HTML을 조금 더 작성해야 한다.

   ```html
   # Before
   <img src="flower-large.jpg" />
   
   # After
   <img
     src="flower-large.jpg"
     srcset="flower-small.jpg 480w, flower-large.jpg 1080w"
     sizes="50vw"
   />
   ```

   > **src 속성**
   >
   > 브라우저가 srcset과 sizes 속성을 지원하지 않으면 fall back으로 src 속성이 동작한다.
   > src 속성은 모든 디바이스 크기에서 동작할 수 있을만큼 충분히 커야한다.
   >
   > **srcset 속성**
   >
   > srcset 속성은 이미지 파일명과 width 또는 density 설명응ㄹ 쉼표로 구분한다.
   > 480w는 480px임을 브라우저에게 말해준다.

5. **Image CDNs**

   Image CDN을 사용하는 이유는 이미지 최적화에 탁월하고, 이를 이용해 전환하면 파일 크기를 40%~80% 줄일 수 있고 이미지 변환, 최적화 및 전송을 전문으로 하고, 사이트에서 사용되는 이미지에 대한 접근이나 조작을 위한 API로 생각할 수 있다.

   ![imageCDNs](https://velog.velcdn.com/images/resyve/post/575665b6-e2bf-4a9c-8f05-a8413d0b9189/image.png)

   - Origin 도메인
   - Image 이미지 검색
   - Security key 다른 사람이 이미지의 새 버전을 만드는 것을 방지
   - Transformations 다양한 이미지 변환 제공

6. **Lazy loading**

   페이지를 로드할 때, 모든 이미지를 로드하는 것이 아니라 중요하지 않은 자원 또는 당장 필요 없는 자원의 경우 서버에 요청을 미루고 필요한 경우 해당 자원을 요청 받는 방법을 말한다. 

   이를 사용함으로 인해 데이터의 낭비를 막을 수 있고, 브라우저는 서버로부터 자원을 요청받고 난 뒤에 화면을 랜더링하기 때문에 불필요한 자원의 다운로드를 막는 것만으로도 프로세스 시간이 단축될 수 있다. (브라우저의 랜더링 시간을 줄여준다.)

   img 태그 내의 속성을 통해 lazy-loading을 지원한다.

   ```html
   <img loading="lazy">
   
   <picture>
     <source media="(min-width: 800px)" srcset="large.jpg 1x, larger.jpg 2x">
     <img src="photo.jpg" loading="lazy">
   </picture>
   ```

   - `auto` : 디폴트 값으로, 속성값을 지정하지 않은 것과 동일하다.
   - `lazy` : 뷰포트 상에서 해당 이미지의 위치를 계산하여 이미지 자원을 요청함
   - `eager` : 어느 위치에 있든지 이미지 자원을 바로 요청받음

   이미지 영역의 크기를 지정하는 것이 권장된다.
   영역 크기에 대한 정보가 없으면 영역을 0*0으로 인식하게 되고, 해당 이미지 영역으로 스크롤 할 경우 이미지가 로드 되면서 layout shift가 일어날 수 있기 때문에, 되도록 해당 img 태그에 명시적으로 높이,너비 값을 지정해야 한다.

   페이지의 첫 시작부터 보이는 이미지에 대해서는 lazy-loading을 사용하지 말아야 하고, background-image에서는 적용 안 된다. `<iframe loading="lazy">` 은 아직 비표준이다.

   


​	









## Reference

- [[tistory: ☼ 꿈꾸는 도전자 Felix !] [웹 프론트앤드 개발 더하기] SVG란 (이미지)](https://superfelix.tistory.com/604)
- [[velog: je] 프론트엔드 이미지 최적화에 관하여](https://velog.io/@resyve/d7uz4czw)
- [[velog: sming.log] 극한의 프론트엔드 성능최적화 2편 (Image 최적화)](https://velog.io/@baby_dev/%EA%B7%B9%ED%95%9C%EC%9D%98-%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-%EC%84%B1%EB%8A%A5%EC%B5%9C%EC%A0%81%ED%99%94-2%ED%8E%B8-Image-%EC%B5%9C%EC%A0%81%ED%99%94)