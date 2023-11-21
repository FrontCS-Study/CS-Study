[TOC]

## 제너레이터

**제너레이터(Generator)** 란, ES6부터 도입된 이터러블을 생성하는 함수이다.

제너레이터는 함수의 실행을 중간에 멈추고 재개할 수 있는 특별한 함수로, 시간이 지남에 따라 일련의 값을 생성 할 수 있다.

### 제너레이터와 일반함수의 차이점

1. **제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.**
   - 일반함수는 매 실행마다 자신의 (함수)바디를 "모두" 실행하며 중간에 종료 할 수 없다.  
   - 제네레이터는 함수의 제어권을 함수가 독점하는 것이 아니라 함수 호출자에게 양도(yield)할 수 있어서, 
     함수 호출자가 함수 실행을 일시 중지 시키거나 다시 시작하게 하도록 할 수 있다.  
2. **제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.**
   - 일반함수는 호출되는 순간에 매개변수를 통해 함수 외부에서부터 값을 전달 받고 실행된다. 즉, 함수가 실행되고 있는 동안에 함수 외부에서 함수 내부로 값을 전달하여 함수의 상태를 변경할 수 없다.
   - 제너레이터 함수는 함수 호출자와 양방향으로 함수의 상태를 주고받을 수 있다. 즉, 함수 호출자에게 상태를 전달할 수 있고 함수 호출자로부터 추가적으로 상태를 전달 받을 수도 있다.

3. **제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.**

   - 일반함수는 호출이 되면 함수의 코드 블록을 실행하고 값을 return 시킨다.

   - 제너레이터 함수를 호출하면 함수 코드를 실행하는 것이 아니라 **제너레이터 객체**(이터러블이면서 동시에 이터레이터)를 **반환** 한다.  

     > 💡 제너레이터는  **이터러블(iterable)이면서 동시에 이터레이터(iterator)인 객체이다.** 다시 말해 제너레이터 함수가 생성한 제너레이터는 Symbol.iterator 메소드를 소유한 이터러블이다. 그리고 제너레이터는 next 메소드를 소유하며 next 메소드를 호출하면 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 이터레이터이다.



> 🔍 **이터러블(iterable)**
>
> 이터러블(iterable)은 자료를 반복할 수 있는 객체를 말한다. `Symbol.iterator`를 사용한 메소드를 가지고 있어야 하며, 가장 대표적으로 배열과 문자열, Map, Set이 있다. 또, 이터러블은 `for ... of` 와 같은 반복문을 사용할 수 있으며, 스프레드 문법도 사용 할 수 있다.
>
> 🔍 **이터레이터(iterator)**
>
> 이터레이터(iterator)는 `{value : 값 , done : true/false}` 형태의 이터레이터 객체를 리턴하는 next() 메서드를 가진 객체.
> next 메서드로 순환 할 수 있는 객체다.
>
> ```js
> const arr = [1,2,3]; //arr는 그냥 평범한 배열
> const iter = arr[Symbol.iterator](); 
> 
> iter.next()
> // {value:1,done: false}
> iter.next()
> // {value:2, done: false},
> iter.next()
> // {value:3, done: false}
> iter.next()
> // {value: undefined, done: true}
> ```

<br>

### 제너레이터의 구조

제너레이터 함수는 `function*` 키워드로 선언한다. 그리고 하나 이상의 `yield` 문을 포함한다. 

제너레이터는 ① yield 키워드와 ② next 메서드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개할 수 있다.

- 제너레이터가 처음 만들어지면 함수의 흐름은 멈춰 있는 상태이다.   

- `next()` 가 호출되면 다음 `yield` 가 있는 곳까지 호출하고 다시 함수가 멈춘다.
- 제너레이터 함수의 호출이 완료되면, `done: true` 가 같이 리턴된다.
- 함수 결과를 함수 호출자에게 객체 형식으로 리턴 → `{value : 값 , done : true/false}`

<details>
  <summary>
     일반적인 순차적으로 값을 전달하는 예시
  </summary>

```js
function* testGenerator() { 
  console.log('제너레이터')
  yield 1;
  console.log('사용법을')
  yield 2;
  console.log('공부하는중')
  yield 3;
 
  return "입니다";
}
 
const generator = testGenerator();
 
generator.next();
// 제너레이터
// {value: 1, done: false}
generator.next();
// 사용법을
// {value: 2, done: false}
generator.next();
// 공부하는중
// {value: 3, done: false}
generator.next();
// {value: '입니다', done: true}
```

</details>

<details>
  <summary>
     값을 파라미터로 전달 및 조회하는 예시
  </summary>

```js
function* sumGenerator() {
    console.log('sumGenerator생성')
    let a = yield ;
    let b = yield ;
    yield a + b;
}
const sum = sumGenerator();
 
sum.next();
// sumGenerator생성
// {value: undefined, done: false}
sum.next(1);
// {value: undefined, done: false}
sum.next(2);
// {value: 3, done: false}
sum.next();
// {value: undefined done: true}
```

</details>

<br>

## async/await

**async/await** 는 기존의 `callback`이나 `Promise`의 단점을 해소하고자  ES8에 등장한 비동기 처리 방식이다. 

비동기 처리를 동기처럼 동작하도록 구현하여 코드를 더욱 간단하게 한다.

async/await 는 프로미스를 기반으로 동작한다. async/await 를 사용하면 then, catch, finally 후속 처리 메서드를 사용할 필요 없이 마치 동기 처럼 프로미스를 사용할 수 있다.

async/await는 함수 안에서만 동작하며 **Promise객체를 반환**한다.

- **async**
  - async는 function 앞에 위치한다.
  - function앞에 async를 붙이면 해당 함수는 항상 프로미스를 반환한다.
    - Promise가 아닌 값을 반환하더라도 이행상태의 Promise로 값을 감싸 이행된 프로미스가 반환되도록 한다.
  - async 함수는 화살표 함수, 함수 표현식으로도 정의가 가능하다.
-  **await**
  - await은 async함수 안에서만 동작한다.
  - Js는 await을 만나면 **Promise처리가 될 때까지 기다리고** 결과는 그 이후에 반환된다.

```js
async function asyncFunc() {
  const response = await fetch("https://rkskekfk.com/todos/1");
  const Json = await response.json();
  console.log(Json);
}
```

#### 예외 처리

async/await는 예외 처리는 try와 catch를 사용한다.

```js
try {
	const result = await fetch("https://rkskekfk.com/todos/1");
	console.log(result);
} catch (err) {
	console.error(err);
}
```

> 🔍 **Promise와 async/await의 차이점**
>
> - **에러 핸들링**
>
>   - Promise를 활용할 시에는 .catch() 문을 통해 에러 핸들링이 가능하다.
>
>   - async/await은 에러 핸들링 할 수 있는 기능이 없어 try-catch() 문을 활용해야 한다.
>
> - **코드 가독성**
>
>   - Promise의 .then() 지옥의 가능성
>   - 코드가 길어지면 길어질수록, `async/await` 를 활용한 코드가 가독성이 좋다.
>     `async/await` 은 비동기 코드가 동기 코드처럼 읽히게 해준다. 코드 흐름을 이해 하기 쉽다.

> 💡 **비동기 처리 방식들 차이점**
>
> - **Callback**
>   - 파라미터로 함수를 전달 받아 함수의 내부에서 실행하는 함수
>   - 함수의 처리 순서를 보장하는 과정에서 함수를 중첩해서 사용 **콜백 지옥**에 빠질 수 있다.
> - **Promise**
>   - 콜백 패턴 단점을 보안하여 ES6에 등장
>   - 비동기 처리 시점을 명확하게 표현할 수 있고, 콜백 지옥의 문제에서 비교적 자유롭다.
>   - 프로미스에는 then, catch, finally라고 하는 **후속 처리 메서드 존재**한다.
>
> - **async/await**
>   - Promise를 더욱 쉽게 사용할 수 있도록 ES8 등장
>   - 비동기 코드를 **마치 동기 코드처럼** 직관적이고 쉽게 작성할 수 있다.
>   - 동기/비동기 구분없이 try/catch로 일관되게 예외 처리를 할 수 있다.

<br>

---

**참고**

[[JS] 제너레이터란(Generator)?!](https://seo-tory.tistory.com/77)

[Generator 함수를 이해하자](https://velog.io/@rohkorea86/제네레이터를-이해해보자.-3.-제네레이터-함수란-what인가)

[ES6 - 제너레이터란? 제너레이터의 사용법 - LightSourceCoder](https://scoring.tistory.com/entry/ES6-제너레이터란-제너레이터의-사용법)

[[JS] 이터러블 & 이터레이터 - 완벽 이해 - 인파 - 티스토리](https://inpa.tistory.com/entry/JS-📚-이터러블-이터레이터-💯완벽-이해)

[[기술면접] 비동기 호출 callback, promise, async/await의 ...](https://velog.io/@ahsy92/기술면접-비동기-호출-callback-promise-asyncawait의-특징과-차이점)

[callback, promise, async/await의 차이](https://velog.io/@wkqkel/callback-promise-asyncawait의-차이)
