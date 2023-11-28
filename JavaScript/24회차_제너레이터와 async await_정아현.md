# 제너레이터와 async await



## 제너레이터(Generator)

> ES6는 **제너레이터 또는 Generator 함수** 형태에서 함수와 Iterator를 다루는 방법을 새롭게 소개했다. 제너레이터는 함수를 **중간에서** 멈추고, 다시 멈췄던 부분부터 실행할 수 있게 한다. 요약하면, **Generator는 함수의 형태를 띄지만, Iterator 처럼 동작한다.**
>
> `async/await`이 Generator를 기반으로 한 것이다.



### ❓ 제너레이터란?

일반적인 함수들은 작업이 끝나기 전엔 끝낼 수 없다. 함수를 다시 한 번 호출하면, 함수는 맨 위에서부터 다시 실행을 시작할 것이다.
반대로, 제너레이터는 **중간에 멈출 수 있는** 함수이다. 그리고 **멈춘 부분부터 다시 실행**을 시작할 수 있다.

✔ 제너레이터는 Iterator 작성 작업을 간단하게 해줄 수 있는 함수들의 특별한 클래스이다.

✔ 제너레이터는 하나의 값 대신에 결과의 순서를 생성하는 함수이다. 이를테면, 제너레이터는 값의 시리즈를 만들어(generate) 낸다.

JS에서 제너레이터는 `next()`를 호출할 수 있는 오브젝트를 반환하는 함수이다. `next()`호출을 할 때마다, 다음과 같은 형태의 오브젝트를 반환한다.

```js
{
    value: Any,
    done: true|false
}
```

`value` 프로퍼티는 값을 가진다. `done` 프로퍼티는 `true`혹은 `false`를 갖는다. `done`이 `true`가 될 때 제너레이터는 멈추고 더이상 값을 만들어내지 않는다.

![normal func vs generator](https://velog.velcdn.com/post-images%2Fjakeseo_me%2F6bc9dbc0-aae3-11e9-8649-35506338cef4%2FGeneratorArchitecture.png)

위 그림은 일반 함수와 제너레이터 함수를 비교한 것이다.

제너레이터가 실행되다가 `yield`를 만나면 멈추고 그 구간을 빠져나오면 다시 시작되고 다시 `yield`를 만나면 멈춘다는 이야기이다.



### 제너레이터 함수 사용

제너레이터 함수는 `function*` 키워드를 사용하며, 아래와 같이 작성하면 된다.

```js
// 제너레이터

function* generatorFunc1() {...}
const generatorFunc2  functoin* () {...}
```

 ❗ 제너레이터 함수는 화살표 함수를 사용할 수 없다.

- `*`(asterisk) 의 위치는 `function` 키워드와 함수 이름 사이면 아무데나 붙여도 되지만, 일관성 유지 목적으로 `function` 키워드 바로 뒤에 붙이는 것이 권장된다고 한다.



###  제너레이터 함수 vs 일반 함수

1️⃣ **제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도(yield)할 수 있다.**

- 일반 함수의 경우, 함수가 어딘가에서 호출되면 그 함수에 대한 제어권은 호출된 함수 본인에게 넘어간다. 
- 하지만 제너레이터 함수는 **함수 실행의 제어권을 함수 호출자에게 양도**할 수 있다. 즉, **함수 호출자는 함수 실행을 일시 중지시키거나 다시 시작하게 하도록 할 수** 있다.

2️⃣ **제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.**

- 일반 함수는 호출되는 순가에 매개변수(parameter)를 통해 함수 외부에서부터 값을 전달 받고 실행된다. 이는 즉, **함수가 실행되고 있는 동안에는 함수 외부에서 함수 내부로 값을 전달해 함수의 상태를 변경할 수 없다**는 것을 뜻한다.
- 반면 **제너레이터 함수는 함수 호출자와 양방향으로 함수의 상태를 주고받을 수 있다**. 즉, 제너레이터 함수는 함수 호출자에게 자신의 상태를 전달할 수 있고 함수 호출자로부터 추가적으로 상태를 전달받을 수도 있다.

3️⃣ **제너레이터 함수는 호출 시 제너레이터 객체를 생성해 반환한다.**

- 일반 함수는 호출이 되면 함수의 코드 블록을 실행시킨다. 
- 반면, 제너레이터 함수는 코드 블록을 실행시키는 것이 아니라 **제너레이터 객체**를 생성해서 반환한다.



### 제너레이터 객체(Generator object)란?

제너레이터 함수를 호출했을 때 리턴되는 객체이다. 제너레이터 객체는 이터러블(iterable) 이면서 동시에 이터레이터(iterator) 이다.

1. **이터러블(iterable)**

   이터러블은 **이터러블 프로토콜을 준수한 객체**를 뜻한다. 여기서 '어터러블 프로토콜을 준수했다' 는 것은 Symbol.iterator 를 프로퍼티로 사용한 메소드를 직접 구현하거나, 프로토타입 체인을 통해 상속받은 객체를 말한다. 배열, 문자열, Map, 그리고 Set 모두 이터러블이다.

   제너레이터 객체는 이터러블이기 때문에 `for ... of` 문을 사용할 수 있으며, 스프레드 문법과 배열 디스트럭처링도 사용할 수 있다.

2. **이터레이터(iterator)**

   이터레이터는 **이터러블에 Symbol.iterator 메소드를 호출했을 때 반환되는 값**이다.

   ```js
   const myArr = [1,2,3]
   const iterator = myArr[Symbol.iterator]()
   ```

   이터레이터는 next라는 메소드를 가지고 있는데, 이걸 이용해서 이터러블 각 요소를 순회할 수 있다. `next` 메소드를 호출하면, 이터레이터 리절트 객체(Iterator result object)가 반환된다. 이터레이터 리절트 객체는 value 와 done 이라는 프로퍼티를 갖는다.

   ```js
   const myArr = [1,2,3]
   const iterator = myArr[Symbol.iterator]()
   const iteratorResultObject = iterator.next()
   console.log(iteratorResultObject) // { value: 1, done: false }
   ```

​		제너레이터 객체도 이터레이터이기 때문에, `next` 메소드를 사용할 수 있다.



## async / await

> ES8에 도입된 문법으로서, Promise 로직을 더 쉽고 간결하게 사용할 수 있게 해주는 문법이다. 유의해야 할 점이 **`async/await`가 Promise를 대체하기 위한 기능이 아니라는 것**이다. 내부적으로는 여전히 Promise를 사용해 비동기 처리를 하고, 단지 코드 작성 부분을 프로그래머가 유지보수하게 편하게 **보이는 문법만 다르게 해줄 뿐**이라는 것이다.

### async / await 기본 사용법

async 와 await 는 절차적 언어에서 작성하는 코드와 같이 사용법도 간단하고 이해하기도 쉽다. **function 키워드 앞에 `async` 만 붙여주면 되고, 비동기로 처리되는 부분 앞에 `await` 만 붙여주면 된다.**

```js
// 프로미스 객체 반환 함수
function delay(ms) {
  return new Promise(resolve => {
    setTimeout(() => {
      console.log(`${ms} 밀리초가 지났습니다.`);
      resolve()
    }, ms);
  });
}
```

```js
// 기존 Promise.then() 형식
function main() {
  delay(1000)
      .then(() => {
        return delay(2000);
      })
      .then(() => {
        return Promise.resolve('끝');
      })
      .then(result => {
        console.log(result);
      });
}

// 메인 함수 호출
main();
```

```js
// async/await 방식
async function main() {
  await delay(1000);
  await delay(2000);
  const result = await Promise.resolve('끝');
  console.log(result);
}

// 메인 함수 호출
main();
```

- promise는 then 메서드를 연속적으로 사용하여 비동기 처리를 하지만, async/await는 await 키워드로 비동기 처리를 기다리고 있다는 것을 직관적으로 표현하고 있음을 볼 수 있다. 
- async/await의 장점은 비동기적 접근방식을 동기적으로 작성할 수 있게 해주어 코드가 간결해지며 가독성을 높여져 유지보수를 용이하게 해준다.





## Reference

- [자바스크립트 개발자라면 알아야 할 33가지 개념 #24-2 자바스크립트 : 예제와 함께 자바스크립트 ES6 generator](https://velog.io/@jakeseo_me/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%9D%BC%EB%A9%B4-%EC%95%8C%EC%95%84%EC%95%BC-%ED%95%A0-33%EA%B0%80%EC%A7%80-%EA%B0%9C%EB%85%90-24-2-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%98%88%EC%A0%9C%EC%99%80-%ED%95%A8%EA%BB%98-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-ES6-generator)

- [[JS] 제너레이터란(Genrerator)?!](https://seo-tory.tistory.com/77)
- [자바스크립트 개발자라면 알아야 할 33가지 개념 #26 자바스크립트 : Async / Await](https://velog.io/@jakeseo_me/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%9D%BC%EB%A9%B4-%EC%95%8C%EC%95%84%EC%95%BC-%ED%95%A0-33%EA%B0%80%EC%A7%80-%EA%B0%9C%EB%85%90-26-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-Async-Await-2bjygyrlgw)

- [📚 자바스크립트 Async/Await 개념 & 문법 정복](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EB%B9%84%EB%8F%99%EA%B8%B0%EC%B2%98%EB%A6%AC-async-await)