[TOC]

## strict mode

**strict mode란?** 

오타나 문법 실수로 인한 오류를 줄이고 안정적인 코드를 생산하고자, ES5부터 등장한 개발 환경이다.

즉, stric mode를 사용하면 자바스크립트 언어의 문법을 보다 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.

strict mode를 적용하려면 전역의 선두 또는 함수 몸체의 선두에 `'use strict';`를 추가한다.

### strict mode가 발생시키는 에러(strict mode로 예방 가능한 코드)

1. **암묵적 전역** : 선언하지 않은 변수를 참조하면 ReferenceError 발생

   ```js
   (function () {
     'use strict';
   
     x = 1;
     console.log(x); // ReferenceError: x is not defined
   }());
   ```

2. **변수, 함수, 매개변수의 삭제** : `delete` 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError 발생

   ```js
   (function () {
     'use strict';
   
     var x = 1;
     delete x;
     // SyntaxError: Delete of an unqualified identifier in strict mode.
   
     function foo(a) {
       delete a;
       // SyntaxError: Delete of an unqualified identifier in strict mode.
     }
     delete foo;
     // SyntaxError: Delete of an unqualified identifier in strict mode.
   }());
   ```

3. **매개변수 이름의 중복** : 중복된 매개변수 이름을 사용하면 SyntaxError 발생

   ```js
   (function() {
     'use strict';
     
     // SyntaxError: Duplicate parameter name not allowed in this context
     function foo(x, x) {
       return x + x;
     }
     console.log(foo(1, 2));
   }());
   ```

4. **with 문의 사용** : with 문을 사용하면 SyntaxError가 발생한다.

   ```js
   (function () {
     'use strict';
   
     // SyntaxError: Strict mode code may not include a with statement
     with({ x: 1 }) {
       console.log(x);
     }
   }());
   ```

<br>

> 💡 **ES6에서 도입된 클래스와 모듈은 기본적으로 strict mode가 적용된다.**

<br>

## 실행 컨텍스트

실행 컨텍스트는 **실행할 코드에 제공할 환경 정보들을 모아놓은 객체**를 의미한다.

실행 컨텍스트는 자바스크립트 코드가 실행되는 환경이다. 모든 JavaScript 코드는 실행 컨텍스트 내부에서 실행된다고 생각하면 된다.

즉, 함수가 실행되면 함수 실행에 해당하는 **실행 컨텍스트**가 생성되고, 자바스크립트 엔진에 있는 콜 스택에 차곡차곡 쌓인다.

스택의 경우 LIFO(Last In First Out)의 구조이기에 **순서를 보장**, 콜스택 내부에 쌓인 실행 컨텍스트의 정보를 통해 **환경을 보장**할 수 있다.

실행 컨텍스트는 식별자(변수, 함수, 클래스 등의 이름)를 등록하고 관리하는 스코프와 코드 실행 순서 관리를 구현한 내부 매커니즘으로, 실행 컨텍스트는 곧 자바스크립트의 핵심 원리다.

### 실행 컨텍스트 구성

> 실행 컨텍스트 객체는 활성화 되는 시점에 **VariableEnviroment, LexcialEnvrionment, ThisBinding**의 세 가지 정보를 수집한다.

- **VariableEnvironment**

  - 현재 컨텍스트 내부의 식별자 정보(`environmentRecord`), 외부 환경 정보(`outerEnvironmentReference`)로 구성된다.

  - 선언 시점의 LexicalEnvironment의 스냅샷으로 변경사항 반영 되지 않는다.
  - VariableEnvironment 에 먼저 정보를 담고, 이를 복사해서 `LexicalEnvironment`를 만든다.
  - 즉, VariableEnviroment는 스냅샷 유지를 목적으로 사용한다.

- **LexicalEnvironment(렉시컬 환경)**

  - VariableEnvironment와 동일한 내용(`environmentRecord`,`outerEnvironmentReference`)으로 구성된다.
  - 처음에는 VariableEnvironment와 같지만, 변경 사항이 실시간으로 반영된다.

  - 즉, VariableEnvironment **초기 상태를 기억**하고 있으며, LexicalEnvironment **최신 상태를 저장**한다.

  - **environmentRecord**
    
    - 함수 안에 코드가 실행되기 전, 현재 컨텍스트에 필요한 식별자 정보가 기록되는 공간(매개변수의 이름, 함수 선언, 변수명 등)
    - 즉, 코드가 실행되기 전에 자바스크립트 엔진은 이미 해당 환경에 속한 코드의 변수명 등을 모두 알고 있게 된다.(**호이스팅**)
    - 더불어 실행 컨텍스트 내부 전체를 처음부터 끝까지 확인하며 **순서대로** 수집한다.
  - **outerEnvironmentReference**
    
    - 현재 호출된 함수가 선언될 당시의 `LexicalEnvironment`를 참조한다. 
    - 즉, 상위 스코프(실행 컨텍스트를 생성한 함수의 바깥 환경)를 가리킨다.
    - 자바스크립트 엔진이 현재 `LexicalEnvironment`에서 변수를 찾을 수 없다면 `outerEnvironmentReference`에서 찾는 과정을 반복한다. 만약 상위 스코프에서도 해당 식별자를 찾을 수 없다면 참조 에러(uncaught reference error)를 발생시킨다.  
    - 위의 과정을 **스코프 체인**이라고 하며 `outerEnvironmentReference`때문에 스코프가 형성되고 스코프체인을 통해 상위 컨텍스트에 접근 할 수 있다.
    
    > 🔍 스코프와 스코프 체인의 개념
    >
    > **scope(스코프)** : 식별자에 대한 유효 범위
    >
    > **scope chain(스코프 체인)** : 식별자의 유효범위를 안에서 바깥으로 차례로 검색해나가는 것

- **ThisBinding**

  - `this` 식별자가 바라봐야 할 대상 객체 정보
  - 실행 컨텍스트가 활성될 때 `this`가 지정되지 않은 경우 `this`에는 전역 객체가 저장된다.

<details>
  <summary> <h3>실행컨텍스트 생성과 작동 방식</h3> </summary>
    <div align="center">
		<img src="https://github.com/FrontCS-Study/CS-Study/assets/83412032/5227f40e-99e4-4cb8-a7e3-4c9ea8043824" alt="실행 컨텍스트 생성과 작동 방식" width="80%">
	</div>


1. 처음 자바스크립트 코드를 실행하는 순간 전역 컨텍스트(브라우저는 `window`객체, Node.js는 `global`객체)가 콜스택에 담긴다.
   콜스택엔 전역 컨텍스트를 제외하곤 다른 컨텍스트가 없기에 전역 컨텍스트와 관련된 코드를 진행한다.
2. 전역 컨텍스트와 관련된 코드를 진행 중 a함수를 실행하였기에 <b>a 함수의 환경 정보들을 수집</b>하여 <b>a 실행 컨텍스트를 생성</b>, 콜스택에 담는다.
   콜스택 최상단에 a 실행 컨텍스트가 있기에 <b>기존의 전역 컨텍스트와 관련된 코드의 실행을 일시적으로 중단</b>한 후 a 실행 컨텍스트의 코드를 실행한다.
3. a 함수 내부에서 b 함수를 실행하였기에 <b>b 함수의 환경 정보들을 수집, 실행 컨텍스트를 생성</b>, 콜스택에 담는다.
이전과 똑같이 콜스택 최상단에 b 실행 컨텍스트가 있기에 <b>기존 a 실행 컨텍스트와 관련된 코드의 실행을 일시적 중단</b>한다.
4. b 함수가 종료된 후 b 실행 컨텍스트가 콜스택에서 제거된다.
   제거 후 콜스택 최상단에는 a 실행 컨텍스트가 있기에 이전에 중단된 지점부터 코드 진행이 재개한다.
5. a 함수 또한 종료된 후 실행 컨텍스트가 콜스택에서 제거된다.
6. 이후엔 전역 공간에 실행할 코드가 남아있지 않다면 콜스택에서 전역 컨텍스트 또한 제거되며 콜스택에 아무 것도 남지 않은 상태로 종료한다.

</details>


## 스코프

> 스코프(Scope, 유효범위)는 식별자(변수)에 접근할 수 있는 유효한(다른 코드가 자신을 참조할 수 있는) 범위를 의미한다.

### 스코프 종류

- **전역 스코프 (Global scope)** : 코드 어디에서든지 참조할 수 있다.
- **지역 스코프 (Local scope)** : 함수 코드 블록이 만든 스코프로 함수 자신과 하위 함수에서만 참조할 수 있다.

전역에 선언된 변수는 전역 스코프를 갖고, 지역에 선언된 변수는 지역 스코프를 갖는다.

```js
var a = 1; // 전역 스코프

function print() { // 지역(함수) 스코프
 var a = 111;
 console.log(a);
}

print(); // 111
console.log(a); // 1
```

### 렉시컬 스코프

렉시컬 스코프는 함수를 어디서 호출하는지가 아니라 **어디에 선언하였는지에 따라 결정되는 것**을 의미한다.

자바스크립트는 렉시컬 스코프를 따르므로 함수를 선언한 시점에 상위 스코프가 결정된다.  
함수를 어디에서 호출하였는지는 스코프 결정에 아무런 의미를 주지 않는다.

```javascript
var x = 1;

function foo() {
  var x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); // 1
bar(); // 1
// 위 예제의 함수 bar는 전역에 선언되었다. 따라서 함수 bar의 상위 스코프는 전역 스코프가되고, 전역 변수 x의 값 1을 두 번 출력한다.
```

<br>

### 전역변수의 문제점

**1\. 암묵적 결합**

- 모든 코드가 전역 변수를 참조하고 변경할 수 있는 **암묵적 결합**을 허용한다.
  - 변수의 유효 범위가 크면 클수록 코드 가독성은 나빠지고 의도치 않게 상태가 변경될 수 있는 위험성도 높아진다.

**2\. 변수의 긴 생명 주기**

- 전역 변수는 생명 주기가 길다.
  - 메모리 리소스를 오랜 시간 소비한다.
  - 전역 변수의 상태를 변경할 수 있는 시간도 길고 기회도 많다.
- `var` 키워드는 변수의 중복 선언을 허용한다. 이름이 중복될 경우 의도치 않은 재할당이 발생할 수 있다.

**3\. 스코프 체인 상에서 종점에 존재**

- 변수를 검색할 때 가장 마지막에 검색된다. 즉 검색 속도가 가장 느리다.

**4\. 네임스페이스 오염**

- 자바스크립트는 파일이 분리되어 있다 해도 하나의 전역 스코프를 공유한다.
  - 다른 파일 내에서 동일한 이름으로 명명된 전역 변수나 전역 함수가 같은 스코프 내에 존재하면 예상치 못한 결과가 발생한다.

> 🔍 **전역변수 사용 억제하는 방법**
>
> 변수의 스코프는 좁을수록 좋다. 전역 변수를 반드시 사용해야 할 이유가 없다면 **지역 변수를 사용**해야 한다.
>
> **1\. 즉시 실행 함수** 
>
> - 모든 코드를 즉시 실행 함수로 감싸서 모든 변수는 즉시 실행 함수의 지역 변수로 만든다.
>
> **2\. 네임스페이스 객체**
>
> - 전역에 네임스페이스 역할을 담당할 객체를 생성하고, 전역 변수처럼 사용하고 싶은 변수를 프로퍼티로 추가한다.
>
> **3\. 모듈 패턴**
>
> - 클래스를 모방해서 관련 있는 변수와 함수를 모아 즉시 실행 함수로 감싸 하나의 모듈을 만든다.  
>   모듈 패턴은 자바스크립트의 `클로저`를 기반으로 동작하며, 전역 변수 억제는 물론 `캡슐화`까지 구현할 수 있다.
>
> **4\. ES6 모듈**
>
> - ES6 모듈을 사용하면 더는 전역 변수를 사용할 수 없다.
>
> - ES6 모듈은 파일 자체의 독자적인 모듈 스코프를 제공한다. 모듈 내에서 var로 선언한 변수는 전역 변수도, window 객체의 프로퍼티도 아니다.
>
> - script 태그에 `type="module"`을 추가하면 파일은 모듈로서 동작한다.
>
>   ```js
>   <script type="module" src="app.mjs></script>
>   ```
>
>   ES6 모듈은 IE를 포함한 구형 브라우저에서 동작할 수 없으며, 트랜스파일링이나 번들링이 필요하기 때문에 아직은 Webpack 등의 모듈 번들러를 사용하는 것이 일반적이다.

<br>

## this

`this`는 식별자가 바라봐야 할 대상 객체 정보를 의미한다.

### this 바인딩

바인딩이란 식별자와 값을 연결하는 과정을 의미한다. 

예를 들어, 변수 선언은 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩하는 것이다.

this바인딩은 this(식별자 역할)와 this가 가리킬 객체를 바인딩하는 것이다.

앞서 실행 컨텍스트 객체는 활성화 되는 시점에 VariableEnviroment, LexcialEnvrionment, **ThisBinding**의 세 가지 정보를 수집한다.  
따라서 실행 컨텍스트는 함수를 호출할 때 생성되므로, **함수 호출 방식에 의해** `this`에 **바인딩 할 객체가 동적으로 결정**된다.  

### 함수 호출 방식에 따른 this에 바인딩 객체

| 함수 호출 방식                          | this 바인딩                                         |
| --------------------------------------- | --------------------------------------------------- |
| 일반 함수, 콜백 함수, 내부 함수 호출    | 전역 객체(window/ global)                           |
| 메서드 호출                             | 메서드를 호출한 객체                                |
| 생성자 함수 호출                        | 생성자 함수가 생성할 인스턴스                       |
| apply/call/bind 메서드에 의한 간접 호출 | apply/call/bind 메서드에 첫 번째 인수로 전달한 객체 |

> 여러 개의 규칙이 중복으로 적용된 경우에는 우선순위가 높은 this binding 기준에 따라 결정된다.
>
> 1순위 : new의 호출로 새로 생성된 객체  
> 2순위 : call, apply, bind로 주어진 객체  
> 3순위 : 호출의 주체인 콘텍스트 객체로 호출된 경우 그 콘텍스트 객체  
> 4순위 : 기본 바인딩인 경우 'strict mode'인 경우 undefined, 'non-strict mode'에서는 전역 객체

<br>


---

**참고**

[[모던 자바스크립트] 5.16 Stric mode](https://poiemaweb.com/js-strict-mode)

[자바스크립트 실행 컨텍스트](https://junilhwang.github.io/TIL/Javascript/Domain/Execution-Context/)

[JS Execution Context (실행 컨텍스트) 란?](https://gamguma.dev/post/2022/04/js_execution_context)

[실행 컨텍스트란 무엇인가요?](https://velog.io/@edie_ko/js-execution-context)

[[모던 자바스크립트] 5.15 스코프](https://poiemaweb.com/js-scope)

[[JavaScript] 전역 변수의 문제점](https://velog.io/@jiseung/JavaScript-%EC%A0%84%EC%97%AD-%EB%B3%80%EC%88%98%EC%9D%98-%EB%AC%B8%EC%A0%9C%EC%A0%90#%EC%A0%84%EC%97%AD-%EB%B3%80%EC%88%98%EC%9D%98-%EB%AC%B8%EC%A0%9C%EC%A0%90)

[[모던 자바스크립트] 5.17 함수 호출 방식에 의해 결정되는 this](https://poiemaweb.com/js-this)

[자바스크립트의 this는 김춘수의 〈꽃〉이다](https://velog.io/@edie_ko/js-this)
