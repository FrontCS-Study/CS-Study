[TOC]

## 프론트엔드 기술 변화

- 1세대

  - 기본적인 HTML, CSS, JavaScript로 DOM을 조작하고 이벤트를 발생시켰다.

- 2세대

  - 웹이 발전하여 DOM의 조작이 빈번하게 발생함에 따라 jQuery 기술이 등장하게 됐다.
  - 결국 jQuery는 DOM을 조작한다는 행위에서 벗어나지 못해서 점점 복잡해지는 환경에서 한계에 도달하게 되었다.

  > 🔍 **DOM(Document Object Model)이란?**
  >
  > DOM은 웹 페이지 문서를 트리 구조의 노드로 표현한 것이다. JavaScript를 사용하여 이러한 노드를 조작할 수 있다. DOM은 웹 페이지의 요소에 동적으로 접근하고 수정하는 데 사용되며, 웹 애플리케이션의 동적인 기능을 구현하는 데 중요한 역할을 한다.
  >
  > [17회차_DOM\_김연수.md](https://github.com/FrontCS-Study/CS-Study/blob/main/JavaScript/17%ED%9A%8C%EC%B0%A8_DOM_%EA%B9%80%EC%97%B0%EC%88%98.md)

- 3세대

  - jQuery 한계를 느낀 이후, 대규모 프로젝트에 효율적인 코드관리와 컴포넌트 기반 UI 개발을 지원하는 새로운 프레임워크(라이브러리)가 등장하게 되었다.

    | Framework | Library |
    | :-------: | :-----: |
    |  Angular  |  React  |
    |    Vue    |         |

  > 🔍 **프레임워크와 라이브러리 차이**
  >
  > <table style="border-collapse: collapse; width: 100%;" border="1" data-ke-align="alignLeft">
  > <tbody>
  > <tr>
  > <td style="text-align: center; width: 9.18604%;">&nbsp;</td>
  > <td style="text-align: center; width: 43.9535%;"><span style="color: #000000;"><b>Framework</b></span></td>
  > <td style="text-align: center; width: 46.8605%;"><span style="color: #000000;"><b>Library</b></span></td>
  > </tr>
  > <tr>
  > <td style="text-align: center; width: 9.18604%;"><span style="color: #000000;"><b>공통점</b></span></td>
  > <td style="text-align: center; width: 90.814%;" colspan="2"><span style="color: #000000;">다른</span><span style="color: #000000;"> 사람이 만들어 둔 코드</span></td>
  > </tr>
  > <tr>
  > <td style="text-align: center; width: 9.18604%;"><span style="color: #000000;"><b>차이점</b></span></td>
  > <td style="text-align: center; width: 43.9535%;"><span style="color: #000000;">다른 사람이 만든 틀</span><span style="color: #000000;">(Frame)</span><span style="color: #000000;">안으로 들어가서 작업하는 것</span></td>
  > <td style="text-align: center; width: 46.8605%;"><span style="color: #000000;">내</span><span style="color: #000000;"> 작업에 다른 사람이 만들어 둔 코드를 가져와서 사용하는 것</span></td>
  > </tr>
  > </tbody>
  > </table>
  >
  >
  > 프레임워크는 모든 환경과 도구를 제공하는 도구이다. 개발자는 프레임워크가 제공하는 규칙과 인터페이스에 따라 코드를 작성해야 한다.
  >
  > 라이브러리는 목적을 위한 하나의 도구를 제공받는 것이다. 개발자는 필요한 기능을 호출하여 사용할 수 있다. 프레임워크와 달리 코드의 흐름과 제어를 직접 관리 할 수 있다.
  >
  > 제어의 주체가 프레임워크는 개발하는 지침을 제공하고, 라이브러리를 개발자에게 제어권이 주어진다.

<br>

## 리액트(React)

React는 **Javascript 라이브러리** 이다.

2013년 Facebook에서 개발한 라이브러리로 기존의 방식보다 더 빠른 UI 렌더링과 반응성 등 "지속적으로 데이터가 변화하는 대규모 애플리케이션 구축"하는 것을 목표로 개발했다. Angular, Vue 등 다른 프레임워크와는 달리 리액트는 오로지 view만 담당하는 라이브러리이다. 그만큼 내장되어 있는 기능이 부족해 third-party 라이브러리(react-router-dom, redux)를 함께 사용한다.

### 리액트를 사용하는 이유

1. 리액트는 자바스크립트 기반의 문법을 사용한다.
   - 리액트는 자기만의 문법을 가지는 Angular, Vue와 달리 자바스크립트 문법을 그대로 사용하기 때문에 자바스크립트에 익숙하다면 보다 쉽게 사용이 가능하다.
2. 리액트는 가볍고 유연한 라이브러리로, 필요한 부분에만 적용할 수 있다.(유연성 및 호환성)
   - 기존 프로젝트에 리액트를 통합하기 쉽게 만들어준다. 또한, 다른 프레임워크나 라이브러리와의 혼용도 가능하므로 기존 코드를 변경하지 않고도 리액트를 도입할 수 있다.
3. 리액트는 활발하고 다양한 커뮤니티와 생태계를 가지고 있다.
   - 페이스북에서 개발한 오픈 소스 프로젝트로, 페이스북의 지속적인 관리가 이루어지고 있으며, 많은 사용자수를 기반으로 생태가 활성화되어 있다.
   - 이는 문제 해결을 위한 자료와 지원을 쉽게 얻을 수 있으며, 다양한 라이브러리와 도구를 활용하여 개발 생산성을 높일 수 있다.
4. 리액트는 웹이 아닌 플랫폼에서도 활용할 수 있는 기술로 확장했다.
   - 리액트의 UI를 만드는 기능을 확장하여 웹이 아닌 플랫폼에서 활용할 수 있도록 기술을 확장했다.
   - React Native는 안드로이드와 아이폰(iOS)의 모바일 앱을 만드는 대표적인 기술로 널리 사용되고 있다.
5. Virtual DOM으로 빠른 렌더링이 가능하다.
   - DOM을 가상화하여 메모리에 보관함으로써 React는 모든 뷰 변경 사항을 가상 DOM에 즉시 반영하여 놀라운 속도의 렌더링을 제공한다.
   - DOM 변경은 시스템을 느리게 만드는데, DOM을 가상화하면 이러한 변경을 최소화하고 스마트하게 최적화할 수 있다. 
   - 모든 가상 DOM 조작은 ‘백그라운드‘에서, 내부에서 자동으로 이루어지기 때문에 하드웨어 리소스 소모율(예를 들어 모바일 기기의 CPU 전력 및 배터리)을 크게 절약할 수 있다.

<br>

## Virtual DOM

> DOM이란? [17회차_DOM\_김연수.md](https://github.com/FrontCS-Study/CS-Study/blob/main/JavaScript/17%ED%9A%8C%EC%B0%A8_DOM_%EA%B9%80%EC%97%B0%EC%88%98.md)

React에서 사용하는 가상DOM(Virtual DOM)은 실제 DOM 내용에 기반하여 만들어진다.

실제 DOM에는 브라우저가 화면을 그리는데 필요한 모든 정보가 들어있어 실제 DOM을 조작하는 작업이 무겁다.  
복잡한 SPA에서는 DOM 조작이 굉장히 빈번하게 발생하며, 그 변화를 적용하기 위해서는 브라우저가 많은 연산을 하게 되고, 결국 전체적인 프로세스가 비효율적이게 된다.  

그래서 React에서는 실제 DOM의 변경 사항을 빠르게 파악하고 반영하기 위해서 내부적으로 Virtual DOM을 만들어서 관리한다.

### Virtual DOM 처리 과정

리액트는 Virtual DOM을 사용하여 실제 DOM에 접근하여 조작하는 대신, 이를 추상화한 자바스크립트 객체를 구성하여 사용한다. 마치 **실제 DOM의 가벼운 복사본**과 비슷하다.

리액트에서 데이터가 변하여 웹 브라우저에 실제 DOM을 업데이트할 때는 다음 세 가지 절차를 가진다.

1. 전체 UI를 Virtual DOM에 리렌더링
2. 이전 내용과 현재 내용을 비교
3. 바뀐 부분만 실제 DOM에 적용

DOM의 상태를 메모리에 저장하고, 변경 전과 변경 후의 상태를 비교 한뒤 최소한의 내용만 반영하여 성능 향상을 이끌어낸다. DOM의 상태를 메모리 위에 계속 올려두고, DOM에 변경 있을 경우 해당 변경 사항만 반영하는 것이다.

<div align="center">
    <img src="https://codingmedic.files.wordpress.com/2020/11/virtualdom.png?w=1024" width="80%" />  
</div>


</div>

만약 Virtual DOM이 없었더라면 DOM은 변경된 빨간 부분뿐만 아니라 모든 동그라미들을 빨간색으로 바꿔서 렌더링 했을 것이다. 사소한 변경사항에도 전체가 리렌더링되기 때문에 브라우저에 과부하가 올 수 있기 때문에 Virtual DOM의 역할은 중요하다.

<br>

---

**참고**

[React란, 프론트엔드 대표 개발 도구 리액트의 특징과 이점](https://www.elancer.co.kr/blog/view?seq=167)

[React란 무엇인가? 최선을 다하는 하루](https://hymndev.tistory.com/45)

[리액트(React)를 왜 사용해야 할까? – 리액트가 강력한 이유](https://modulabs.co.kr/blog/react-library/)

[[React\] DOM이란? Virtual DOM을 사용하는 이유?](https://velog.io/@ctdlog/React-DOM이란-Virtual-DOM을-사용하는-이유)
