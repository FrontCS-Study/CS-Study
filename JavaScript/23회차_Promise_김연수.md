

[TOC]

## Promise 

### 콜백 함수(Callback)

특정 함수 내 인자로 다른 함수를 받아서 어떤 특정 시점에 도달하였을 때, 시스템에서 인자로 받은 함수를 호출하는 것을 말한다.  
즉, 비동기 작업이 끝난 뒤 호출(Call back)되는 함수를 의미한다.

#### 콜백 함수의 단점

콜백 패턴을 사용하여 처리 순서를 보장하기 위해 중첩이 많이 생길 경우,  코드 복잡도가 증가한다.

- **콜백 지옥(callback hell)** : 여러개 비동기 작업을 순차적으로 수행해야 할 때, 콜백 함수가 중첩되어 코드의 깊이가 깊어지는 현상.  

이러한 콜백 함수의 코드 형태는 코드 가독성을 떨어트리고, 코드의 흐름을 파악하기 어려워진다.  
또한 콜백 함수마다 예외를 따로 처리해야 하고,  에러가 발생한 위치를 추적하기 힘들다.  

```js
/* 숫자 n 을 파라미터로 받아와서 다섯번에 걸쳐 1초마다 1씩 더해서 출력하는 비동기 함수 */
function increaseAndPrint(n, callback) {
  setTimeout(() => {
    const increased = n + 1;
    console.log(increased);
    if (callback) callback(increased); // 콜백함수 호출
  }, 1000);
}

increaseAndPrint(0, n => {
  increaseAndPrint(n, n => {
    increaseAndPrint(n, n => {
      increaseAndPrint(n, n => {
        increaseAndPrint(n, n => {
          console.log('끝!');
        });
      });
    });
  });
});
```

<br>

### 프로미스(Promise)

ES6에 추가된 문법으로 **콜백함수의 단점을 보안하기 위해 등장**하였으며 비동기 작업이 수행되어 결과를 반환하는 객체를 의미하며,  
완료되면 resolve 메소드를 호출하고 작업이 실패하면 reject를 호출하는 함수이다.

비동기 작업이 끝날 때까지 결과를 기다리는 것이 아니라, 결과를 제공하겠다는 '약속'을 반환한다는 의미에서 Promise라 지어졌다.

```js
// 앞 콜백 함수를 프로미스로 변경했을 때 코드
function increaseAndPrint(n) {
  return new Promise((resolve, reject)=>{
    setTimeout(() => {
      const increased = n + 1;
      console.log(increased);
      resolve(increased);
    }, 1000)
  })
}

increaseAndPrint(0)
  .then((n) => increaseAndPrint(n))
  .then((n) => increaseAndPrint(n))
  .then((n) => increaseAndPrint(n))
  .then((n) => increaseAndPrint(n)); // 체이닝 기법
```

> 🔍 **프로미스 체이닝(Promise Chaning)**
>
> 프로미스 체이닝이란, 프로미스 핸들러를 연달아 연결하는 것을 의미하며, 체이닝 기법을 통해 비동기 작업을 순차적으로 수행할 수 있다. 
>
> 체이닝이 가능한 이유는 then 핸들러에서 값을 리턴하면, 그 **반환값은 자동으로 프로미스 객체로 감싸져 반환**되기 때문이다.

<br>

### 프로미스 객체 사용법

#### 프로미스 객체를 변수에 바로 할당하는 방식

Promise 객체를 생성하려면 new 키워드와 Promise 생성자 함수를 사용한다.  
이때 Promise 생성자 안에 두 개의 매개변수를 가진 콜백 함수를 넣게 되는데, 첫 번째 인수는 작업이 성공했을 때 성공(resolve)임을 알려주는 객체이며, 두 번째 인수는 작업이 실패했을 때 실패(reject)임을 알려주는 오류 객체이다.

```js
// 프로미스 객체 생성
const myPromise = new Promise((resolve, reject) => {
    const data = fetch('서버로부터 요청할 URL'); // 비동기 작업 수행
    if(data) resolve(data); // 만일 요청이 성공하여 데이터가 있다면
    else reject("Error"); // 만일 요청이 실패하여 데이터가 없다면
})
```

Promise 객체는 비동기 작업이 완료된 이후에 다음 작업을 연결시켜 진행할 수 있다. 작업 결과 따라 .then() 과 .catch() 메서드 체이닝을 통해 성공과 실패에 대한 후속 처리를 진행할 수 있다.

```js
// 프로미스 객체 처리
myPromise
    .then((value) => { // 성공적으로 수행했을 때 실행될 코드
    	console.log("Data: ", value); // 위에서 return resolve(data)의 data값이 출력된다
    }).catch((error) => { // 실패했을 때 실행될 코드
     	console.error(error); // 위에서 return reject("Error")의 "Error"가 출력된다
    })
```



#### 프로미스 함수 등록 방식

함수를 만들고 그 함수를 호출하면 프로미스 생성자를 return 하여, 곧바로 생성된 프로미스 객체를 함수 반환값으로 얻어 사용하는 방식

다음과 같은 이유로 일반적으로 함수 형태로 프로미스 객체를 사용한다. ex) Fetch() 메서드

- **재사용성** : 프로미스 객체를 함수로 만들면 필요할 때마다 호출하여 사용함으로써, 반복되는 비동기 작업을 효율적으로 처리할 수 있다.
- **가독성**
  - 프로미스 객체를 함수로 만들면 코드의 구조가 명확해져, 비동기 작업의 정의와 사용을 분리하여 코드의 가독성을 높일 수 있다.
- **확장성**
  - 프로미스 객체를 함수로 만들면 인자를 전달하여 동적으로 비동기 작업을 수행할 수 있다. 또한 여러 개의 프로미스 객체를 반환하는 함수들을 연결하여 복잡한 비동기 로직을 구현할 수 있다.

```js
// 프로미스 객체를 반환하는 함수 생성
function myPromise() {
  return new Promise((resolve, reject) => {
    if (/* 성공 조건 */) {
       resolve(/* 결과 값 */);
    } else { 
       reject(/* 에러 값 */);
  	}
  });
}
```

<br>

### 프로미스 3가지 상태

- **Pending(대기)** : 프로미스 객체를 생성하고, 처리가 완료되지 않은 상태 (처리 진행중)
- **Fulfilled(이행)** : 성공적으로 처리가 완료된 상태 → resolve 함수 호출
- **Rejected(거부)** : 처리가 실패로 끝난 상태 → reject 함수 호출

<br>

### 프로미스 핸들러

- `.then()` : 프로미스가 이행(fulfilled)되었을 때 실행할 콜백 함수를 등록하고, 새로운 프로미스를 반환
- `.catch()` : 프로미스가 거부(rejected)되었을 때 실행할 콜백 함수를 등록하고, 새로운 프로미스를 반환
- `.finally()` : 프로미스가 이행되거나 거부될 때 상관없이 실행할 콜백 함수를 등록하고, 새로운 프로미스를 반환

<br>

### 프로미스 정적 메서드

프로미스 정적 메서드는 객체를 초기화 & 생성하지 않고도 바로 사용할 수 있기 때문에 비동기 처리를 보다 효율적이고 간편하게 구현할 수 있도록 도와준다.

#### Promise.resolve() / Promise.reject()

Promise.resolve / Promise.reject 는 이미 존재하는 값을 래핑하여 프로미스를 생성한다.

```jsx
/* 인수로 전달한 배열을 resolve하는 프로미스를 생성, 위 아래 예제 모두 동일하게 동작한다. */
// ① 정적 메서드 사용
const resolvedPromise = Promise.resolve([1,2,3])
resolvedPromise.then(console.log) // [1,2,3]
// ② 생성자 함수를 통해 프로토타입 메서드 사용
const resolvedPromise = new Promise(resolve => resolve([1,2,3]))
resolvedPromise.then(console.log) // [1,2,3]
```

프로미스 객체와 전혀 연관없는 함수 내에서 필요에 따라 프로미스 객체를 반환하여 핸들러를 이용할 수 있도록 응용이 가능하다.   
이 방법은 비동기 작업을 수행하지 않는 함수에서도 프로미스의 장점을 활용하고 싶은 경우에 유용하다.

<details>
  <summary>
     Promise.resolve() 응용
  </summary>


  ```js
// 프로미스 객체와 전혀 연관없는 함수
function getRandomNumber() {
  const num = Math.floor(Math.random() * 10); // 0 ~ 9 사이의 정수
  return num;
}

// Promise.resolve() 를 사용하여 프로미스 객체를 반환하는 함수
function getPromiseNumber() {
  const num = getRandomNumber(); // 일반 값
  return Promise.resolve(num); // 프로미스 객체
}

// 핸들러를 이용하여 프로미스 객체의 값을 처리하는 함수
getPromiseNumber()
    .then((value) => {
      console.log(`랜덤 숫자: ${value}`);
    })
    .catch((error) => {
      console.error(error);
    });
  ```

</details>

#### Promise.all()

`Promise.all()` 은 여러 개의 비동기 처리를 한꺼번에 병렬처리할 때 사용한다.

모든 프로미스 비동기 처리가 이행(fulfilled) 될때까지 기다려서, 모든 프로미스가 완료되면 그때 then 핸들러가 실행하는 형태이다.

모든 프로미스가 fulfilled 상태가 되면 **모든 처리 결과를 배열에 저장**해 새로운 프로미스 반환하고, 인수로 전달받은 배열의 프로미스가 **하나라도 rejected되면 즉시 에러 띄우고 종료**한다.

가장 대표적인 사용 예시로 여러 개의 API 요청을 보내고 모든 응답을 받아야 하는 경우에 사용할 수 있다.

<details>
  <summary>
     Promise.all()로 API 요청 예시
  </summary>

```js
// 1. 서버 요청 API 프로미스 객체 생성 (fetch)
const api_1 = fetch("https://jsonplaceholder.typicode.com/users");
const api_2 = fetch("https://jsonplaceholder.typicode.com/users");
const api_3 = fetch("https://jsonplaceholder.typicode.com/users");

// 2. 프로미스 객체들을 묶어 배열로 구성
const promises = [api_1, api_2, api_3];

// 3. Promise.all() 메서드 인자로 프로미스 배열을 넣어, 모든 프로미스가 이행될 때까지 기다리고, 결과값을 출력
Promise.all(promises)
    .then((results) => {
      // results는 이행된 프로미스들의 값들을 담은 배열.
      // results의 순서는 promises의 순서와 일치.
      console.log(results); // [users1, users2, users3]
    })
    .catch((error) => {
      // 어느 하나라도 프로미스가 거부되면 오류를 출력
      console.error(error);
    });
```

</details>

#### Promise.allSettled()

`Promise.all()` 메서드의 업그레이드 버전으로, 주어진 모든 프로미스가 처리되면 모든 프로미스 각각의 상태와 값(또는 거부 사유)을 모아놓은 배열을 반환한다.

**fulfilled 상태**인 경우 비동기 처리 상태를 나타내는 status 프로퍼티와 **처리 결과를 나타내는 value 프로퍼티** 가짐  
**rejected 상태**인 경우 비동기 처리 상태를 나타내는 status 프로퍼티와 **에러를 나타내는 reason 프로퍼티** 가짐

<details>
  <summary>
     Promise.allSettled() 사용 예시
  </summary>

```js
Promise.allSettled([
  new Promise(resolve => setTimeout(() => resolve(1), 2000)),
  new Promise((_, reject) => setTimeout(() => reject(new Error('Error!!!')), 1000))
]).then(data => console.log(data))

/* 출력 결과
[
  { status: 'fulfilled', value: 1 },
  { status: 'rejected', reason: Error: 'Error!!!' }
]
*/
```

</details>

#### Promise.any()

`Promise.all()` 메서드의 반대 버전으로, `Promise.any()` 는 주어진 모든 프로미스 중 **하나라도 완료되면 바로 반환**하는 정적 메서드이다.

첫번째로 이행(fulfilled) 된 프로미스만을 취급하기 때문에 거부된 프로미스는 무시하게 된다.  
만일 요청된 모든 프로미스가 거부되면, AggregateError 객체를 사유로 하는 거부 프로미스를 반환한다.

<details>
  <summary>
     Promise.any() 사용 예시
  </summary>

- promise2의 처리만 처리 도출, 그 외 promise1과 promise3의 거부는 무시한다.

```js
const promise1 = new Promise((resolve, reject) => {
  setTimeout(() => reject("promise1 failed"), 3000);
});

const promise2 = new Promise((resolve, reject) => {
  setTimeout(() => resolve("promise2 succeeded"), 2000);
});

const promise3 = new Promise((resolve, reject) => {
  setTimeout(() => reject("promise3 failed"), 1000);
});

// promise1, promise2, promise3은 각각 3초, 2초, 1초 후에 거부되거나 이행
Promise.any([promise1, promise2, promise3])
  .then((value) => {
    console.log(value); // "promise2 succeeded" 
  })
  .catch((error) => { // 만약 모든 프로미스가 거부 됐을 경우
    console.error(error); // AggregateError: All promises were rejected
    console.error(error.errors); // ["promise3 failed", "promise1 failed", "promise4 failed"]
  });
```

</details>

#### Promise.race()

`Promise.any()` 와 같이 여러 개의 프로미스 중 가장 먼저 처리된 프로미스의 결과값을 반환하지만,   
`Promise.race()` 는 fulfilled(이행), rejected(실패) 여부 상관없이 **무조건 처리가 끝난 프로미스 결과값**을 반환하다. 

모든 프로미스가 fulilled 상태가 되기를 기다리는 게 아니라 **하나만 fulfilled 되면 종료, 하나라도 reject되면 에러 띄우고 종료**

마치 프로미스 참가자들이 레이싱(race) 경주를 하는 것과 같다.

<br>

### 프로미스 지옥

프로미스의 then() 메서드가 지나치게 체인되어 반복되면 코드가 장황해지고 가독성이 굉장히 떨어질 수 가 있다.

then을 늘어뜨어 놓으면 코드가 길어지고, 각 then 메서드가 어떤 값을 반환하는지 파악하기 어렵게 된다.   
또한, catch 메서드가 마지막에 한 번만 사용되어 있기 때문에, 중간에 발생할 수 있는 에러나 예외 상황에 대응하기 어렵다.

이를 극복하기 위해 **ES8에서 도입**된 **async/await** 키워드가 등장했다.  
async/await 키워드는 프로미스를 기반으로 하지만 then과 catch 메서드를 사용하지 않고 비동기 작업을 수행할 수 있다.   
또한 **비동기 작업을 마치 동기 작업처럼** 쓸 수 있어서 코드가 간결하고 가독성이 좋아지게 된다.

<br>

---

**참고**

[[JS] Promise 이해하기 -1 (콜백함수, 정의, 요청상태)](https://adjh54.tistory.com/29)

[자바스크립트 Promise 개념 & 문법 정복하기 - Inpa Dev ‍](https://inpa.tistory.com/entry/JS-📚-비동기처리-Promise)

[[JavaScript] 프로미스 체이닝, 정적 메서드 정리](https://velog.io/@eunjin/JavaScript-프로미스-체이닝-정적-메서드-정리)
