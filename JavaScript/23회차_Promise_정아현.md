# Promise



### 자바스크립트 프로미스 객체

자바스크립트 Promise 객체는 **비동기 작업의 최종 완료 또는 실패**를 나타내는 Array나 Object처럼 독자적인 객체이다. 비동기 작업이 끝날 때까지 결과를 기다리는 것이 아니라, 결과를 제공하겠다는 '약속'을 반환한다는 의미에서 Promise라 명명했다고 한다.

Promise는 **3가지 상태**를 가진다.

1️⃣ Pending(미결, 대기) : 이행하지도, 거부하지도 않은 초기 상태

2️⃣ Fulfilled(이행) : 연산이 성공적으로 완료된 상태

3️⃣ Rejected(거부, 거절) : 연산이 실패한 상태

대기중인 프로미스는 값과 함께 이행할 수도, 어떤 이유(오류)로 인해 거부될 수도 있다. 이행이나 거부될 때, 프로미스의 `then`메서드에 의해 대기열(큐)에 추가된 처리기들이 호출된다. 이미 이행했거나 거부된 프로미스에 처리기를 연결해도 호출되므로, 비동기 연산과 처리기 연결 사에 경합 조건은 없다.



✅ 예를 들어, 여러분을 아이라고 상상하고, 여러분의 어머니가 새로운 **스마트폰**을 다음 주에 사주기로 **약속** 한다.

그렇다고 다음 주에 정말 새로운 스마트폰을 가질 수 있을까? 어머니는 스마트폰을 사줄 수도 있고, 만일 기분이 좋지 않다면, 약속을 어기고, 스마트폰을 사주는 것을 잠시 보류할 수도 있다.

이게 바로 **Promise(약속)**이다.

1. 대기(Pending): 여러분은 새로운 스마트폰을 가질 수 있을지 알 수 없다.
2. 이행(Fulfilled): 어머니의 기분이 괜찮아서, 스마트폰을 사주셨다.
3. 거절: Rejected 어머니가 기분은 괜찮지만, 스마트폰은 사주지 않기로 했다.



### 프로미스 객체 생성

```js
// ES 5 //
var is MomHappy = false;

// Promise
var willGetNewPhone = new Promise((resolve, reject) => {
        var phone = {
            brand: 'Apple',
            color: 'silver'
        }
        else {
            var reason = new Error('mom is not happy');
            reject(reason); // reject
        }
    }
);
```

위 예시를 코드로 만든 것이다. 대략 봐도 내용이 이해가 간다.

```js
// Promise의 문법은 대략 다음과 같다.
new Promise(/_ executor _/ function (resolve, reject) { ... });
```

```js
const myPromise = new Promise((resolve, reject) => {
	// 비동기 작업 수행
    const data = fetch('서버로부터 요청할 URL');
    
    if(data)
    	resolve(data); // 만일 요청이 성공하여 데이터가 있다면
    else
    	reject("Error"); // 만일 요청이 실패하여 데이터가 없다면
})
```



✔ Promise 생성자 안에 들어가는 콜백 함수를 executor라고 부른다.

✔ Promise 생성자 안에 두 개의 매개변수를 가진 콜백 함수를 넣는데, 첫 번째 인수는 작업이 성공햇을 때 성공임을 알려주는 객체, 두 번째 인수는 작업이 실패했을 때 실패임을 알려주는 오류 객체이다.



### 프로미스 객체 처리

Promise 객체는 비동기 작업이 완료된 이후, 다음 작업을 연결 시켜 진행할 수 있다. 작업 결과에 따라 `.then`, `.catch()` 메서드 체이닝을 통해 성공과 실패에 대한 후속 처리를 진행할 수 있다.

만일 처리가 성공해 프로미스 객체 내부에서 `resolve(data)`를 호출하게 되면, 바로 `.then()`으로 이러져 `then()`메서드의 콜백 함수에서 성공에 대한 추가 처리를 진행한다. 이때 호출한 `resolve()` 함수의 매개변수의 값이 `then()`메서드의 콜백 함수 인자로 들어가 `then` 메서드 내부에서 프로미스 객체 내부에서 다룬 값을 사용할 수 있게 된다.

반대로 처리가 실패해 프로미스 객체 내부에서 reject`("Error")`를 호출하게 되면, 바로 `.catch()`로 이어져 `catch` 메서드의 콜백 함수에서 성공에 대한 추가 처리를 진행한다.

```js
var willGetNewPhone = new Promise((resolve, reject) => {...});

// Promise 호출
var askMom = function () {
    willGetNewPhone
    	.then((fulrilled) => {
        // 와! 새 폰을 얻었다!
        console.log(fulrilled);
        // output: { brand: 'Apple', color: 'silver' }
    })
    .catch((error) => {
        // 이런,, 엄마가 폰을 안사주네,,
        console.log(error.message);
    });
}
```

```js
myPromise
    .then((value) => { // 성공적으로 수행했을 때 실행될 코드
    	console.log("Data: ", value); // 위에서 return resolve(data)의 data값이 출력된다
    })
    .catch((error) => { // 실패했을 때 실행될 코드
     	console.error(error); // 위에서 return reject("Error")의 "Error"가 출력된다
    })
    .finally(() => { // 성공하든 실패하든 무조건 실행될 코드
    	
    })
```



### 프로미스 함수 등록

프로미스 객체를 변수에 바로 할당하는 방식을 사용할 수도 있지만, 보통은 별도로 함수로 감싸서 사용하는 것이 일반적이다.

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

// 프로미스 객체를 반환하는 함수 사용
myPromise()
    .then((result) => {
      // 성공 시 실행할 콜백 함수
    })
    .catch((error) => {
      // 실패 시 실행할 콜백 함수
    });
```

함수를 만들고 그 함수를 호출하면 프로미스 생성자를 return 함으로서, 곧바로 생성된 **프로미스 객체를 함수 반환값**으로 얻어 사용하는 기법이다. 이렇게 프로미스 객체를 함수로 만드는 이유는 다음과 같다.

➰ 재사용성 : 프로미스 객체를 함수로 만들면 필요할 때마다 호출하여 사용함으로써, 반복되는 비동기 작업을 효율적으로 처리할 수 있다.

➰ 가독성 :  프로미스 객체를 함수로 만들면 코드의 구조가 명확해져, 비동기 작업의 정의와 사용을 분리해 코드의 가독성을 높일 수 있다.

➰ 확장성 :  프로미스 객체를 함수로 만들면 인자를 전달하여 동적으로 비동기 작업을 수행할 수 있다. 또한 여러 개의 프로미스 객체를 반환하는 함수들을 연결해 복잡한 비동기 로직을 구현할 수 있다.

> ❗ 프로미스를 생성해 반환하는 함수를 '프로미스 팩토리 함수'라고 부르기도 한다.

위와 같은 이유로 프로미스 객체를 사용할 일이 있다면 함수로 감싸 사용한다. 그래서 대부분의 자바스크립트 비동기 라이브러리도 함수 형태로 프로미스 객체를 제공한다. 대표적으로 자바스크립트의 `fetch()`메서드가 있다. 이 `fetch()` 메서드 내에서 프로미스 객체를 생성해 서버로부터 데이터를 가져오면 `resolve()`해 `.then()`으로 처리한다.

```js
// GET 요청 예시
fetch('https://jsonplaceholder.typicode.com/posts/1')
  .then((response) => response.json()) // 응답 객체에서 JSON 데이터를 추출한다.
  .then((data) => console.log(data)); // JSON 데이터를 콘솔에 출력한다.
```



### 프로미스 체이닝

프로미스 체이닝이란, **프로미스 핸들러를 연달아 연결**하는 것을 말한다. 이렇게 하면 **여러 개의 비동기 작업을 순차적으로 수행**할 수 있다.

```js
// 위 예시에 이어 새로운 스마트폰을 사면 친구들에게 보여주기로 약속(Promise)했다고 생각해보자.

...
// 2nd promise
var showOff = (phone) => {
    var message = 'Hey friend, i have a new' + phone.color + ' ' + 'phone.brand' + 'phone';
    return Promise.resolve(message);
};
```

위 예시의 `willGetNewPhone` Promise를 수행한 이후에만, `showOff` Promise를 수행할 수 있다.

```js
...
// call our promise
var askMom = () => {
    willGetNewPhone
    .then(showOff) // 여기서 체이닝
    .then((fulfilled) => {
        console.log(fulfilleded);
        // output: 'Hey friend, i have a new silver Apple phone.'
    })
    .catch((error) => {
        console.log(erroe.message);
        
    })
}
```



아래는 `doSomething` 함수를 호출해 프로미스를 생성하고, then 메서드를 통해 이행 핸들러를 연결하는 과정을 보여준다. 각 이행 핸들러는 이전 프로미스의 값에 50을 더한 값을 반환하고, 마지막 이행 핸들러는 최종 값을 콘솔에 출력한다.

```js
function doSomething() {
  return new Promise((resolve, reject) => {
      resolve(100) // chaining
  });
}

doSomething()
    .then((value1) => {
        const data1 = value1 + 50;
        return data1
    })
    .then((value2) => {
        const data2 = value2 + 50;
        return data2
    })
    .then((value3) => {
        const data3 = value3 + 50;
        return data3
    })
    .then((value4) => {
        console.log(value4); // 250 출력
    })
```

✔ 이런식으로 체이닝이 가능한 이유는 then 핸들러에서 값을 리턴하면, 그 **반환값은 자동으로 프로미스 객체로 감싸져 반환**되기 때문이다. 그리고 다음 **then 핸들러에서 반환된 프로미스 객체를 받아 처리**하는 것이다. 그래서 프로미스 상태의 흐름도를 표현하면 아래와 같다.

![promise status flow](https://blog.kakaocdn.net/dn/btbl7U/btq98vwJvki/ZwOhWDQTkcH7g1n82atKkk/img.png)

✔ 만일 연결된 이행 핸들러에서 중간에 오류가 있는 처리를 행하면, 예외처리를 함으로써 catch 핸들러에 점프하도록 설정할 수 있다. `try`-`catch`와 비슷한 개념.

```js
function doSomething(arg) {
  return new Promise((resolve, reject) => {
      resolve(arg) // 2. resolve 되고
  });
}

doSomething('100A')  // 1. 여기서 doSomething 100A arg로 호출 
    .then((value1) => {
        const data1 = value1 + 50; // 숫자에 문자를 연산

        if (isNaN(data1)) // 3. 여기서 에러 판단.
            throw new Error('값이 넘버가 아닙니다') 
        
        return data1
    })
    .then((value2) => {
        const data2 = value2 + 50;
        return data2
    })
    .catch((err) => { // 4. 에러 반환
        console.error(err);
    })
```

만일 catch 핸들러 다음으로 then 핸들러가 이어서 체이닝 되어 있다면, 에러가 처리되고 가까운 then 핸들러로 제어 흐름이 넘어가 실행이 이어지게 된다.

```js
new Promise((resolve, reject) => {
  throw new Error("에러 발생!"); // 1. 에러발생
})
    .catch(function(error) { // 2. catch 에서 에러 잡고,
      console.log("에러가 잘 처리되었습니다. 정상적으로 실행이 이어집니다.");
    })
    .then(() => { // 3. 에러 처리 정상정으로 되면 체이닝 된 then으로 넘어감
        console.log("다음 핸들러가 실행됩니다.")
    })
    .then(() => {
        console.log("다음 핸들러가 또 실행됩니다.")
    });

// 에러가 잘 처리되었습니다. 정상적으로 실행이 이어집니다.
// 다음 핸들러가 실행됩니다.
// 다음 핸들러가 또 실행됩니다.
```



### 프로미스 정적 메서드

프로미스 객체는 생성자 함수 외에도 여러가지 정적 메서드(static method)를 제공한다. 정적 메서드는 객체를 초기화 & 생성하지 않고도 바로 사용할 수 있기 때문에 비동기 처리를 보다 효율적이고 간편하게 구현할 수 있도록 도와준다.

**✅ Promise.resolve()**

보통 프로미스의 `resolve()`메서드는 프로미스를 생성자로 만들고 그 안의 콜백 함수의 매개변수를 통해 호출하여 사용해왔다.

```js
// 프로미스 생성자를 사용하여 프로미스 객체를 반환하는 함수
function getPromiseNumber() {
  return new Promise((resolve, reject) => {
      const num = Math.floor(Math.random() * 10); // 0 ~ 9 사이의 정수
      resolve(num); // 프로미스 이행
  });
}
```

이러한 과정을 `Promise.resolve()` 정적 메서드로 한 번에 호출할 수 있도록 편의 기능을 제공해 주는 것으로 보면 된다. 그래서 프로미스 정적 메서드를 이용하면 **프로미스 객체와는 전혀 연관없는 함수 내에서 필요에 따라 프로미스 객체를 반환해 핸들러를 이용**할 수 있도록 응용이 가능하다. 이 방법은 비동기 작업을 수행하지 않는 함수에서도 프로미스의 장점을 활용하고 싶은 경우에 유용하다.

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



**✅ Promise.reject()**

마찬가지로 주어진 사유로 거부하는 Promise 객체를 반환한다.

```js
// 주어진 사유로 거부되는 프로미스 생성
const p = Promise.reject(new Error('error'));

// 거부 사유를 출력
p.catch(error => console.error(error)); // Error: error
```



**✅ Promise.all()**

배열, Map, Set 에 포함된 여러개의 프로미스 요소들을 한꺼번에 비동기 작업을 처리해야 할 때 굉장히 유용한 프로미스 정적메소드이다**. 모든 프로미스 비동기 처리가 이행(fulfilled) 될 때 까지 기다려서, 모든 프로미스가 완료되면 그때 then 핸들러가 실행하는 형태**로 보면 된다. 가장 대표적인 사용 예시로 여러 개의 API 요청을 보내고 모든 응답을 받아야 하는 경우에 사용할 수 있다.

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



**✅ Promise.allSettled()**

`Promise.all()` 메서드의 업그레이드 버전으로, 주어진 모든 프로미스가 처리되면 모든 프로미스 각각의 상태와 값(또는 거부 사유)을 모아놓은 배열을 반환한다.

```js
// 1초 후에 1을 반환하는 프로미스
const p1 = new Promise(resolve => setTimeout(() => resolve(1), 1000));

// 2초 후에 에러를 발생시키는 프로미스
const p2 = new Promise((resolve, reject) => setTimeout(() => reject(new Error('error')), 2000));

// 3초 후에 3을 반환하는 프로미스
const p3 = new Promise(resolve => setTimeout(() => resolve(3), 3000));

// 세 개의 프로미스의 상태와 값 또는 사유를 출력 (배열 반환)
Promise.allSettled([p1, p2, p3])
	.then(result => console.log(result));
```



**✅ Promise.any()**

`Promise.all()` 메서드의 반대 버전으로, `Promise.all()` 이 주어진 모든 프로미스가 **모두 완료** 해야만 결과를 도출한다면, `Promise.any()`는 주어진 모든 프로미스 중 **하나라도 완료**되면 바로 반환하는 정적 메서드이다.

```js
const promise1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("promise1 failed");
  }, 3000);
});

const promise2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("promise2 succeeded");
  }, 2000);
});

const promise3 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("promise3 failed");
  }, 1000);
});

// promise1, promise2, promise3은 각각 3초, 2초, 1초 후에 거부되거나 이행
Promise.any([promise1, promise2, promise3])
  .then((value) => {
    console.log(value); // "promise2 succeeded" 
  })
  .catch((error) => {
    console.error(error);
  });
```

위 코드를 보면 `Promise.any()` 메서드의 결과로 promise2의 처리가 가장 먼저 도출됨을 볼 수 있다. 오로지 첫번째로 이행(fulfilled) 된 프로미스만을 취급하기 때문에 나머지 promise1과 promise3의 거부(rejected)는 무시되게 된다.
만일 요청된 모든 프로미스가 거부(rejected)되면, AggregateError 객체를 사유로 하는 거부 프로미스를 반환한다. (아래 예시)

```js
const promise1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("promise1 failed");
  }, 3000);
});

const promise2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("promise2 succeeded");
  }, 2000);
});

const promise4 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("promise4 failed");
  }, 4000);
});

Promise.any([promise1, promise3, promise4])
  .then((value) => {
    console.log(value);
  })
  .catch((error) => {
    console.error(error); // AggregateError: All promises were rejected
    console.error(error.errors); // ["promise3 failed", "promise1 failed", "promise4 failed"]
  });
```



**✅ Promise.race()**

`Promise.race()`는 `Promise.any()`와 같이 여러 개의 프로미스 중 가장 먼저 처리된 프로미스의 결과값을 반환하지만, 차이점이 존재한다.

`Promise.any()`는 가장 먼저 fulfilled(이행) 상태가 된 프로미스만을 반환하거나, 혹은 전부 rejected(실패) 상태가 된 프로미스(AggregateError)를 반환한다. 반면 `Promise.race()`는 fulcilled(이행), rejected(실패) **여부 상관없이 무조건 처리가 끝난 프로미스 결과값을 반환**한다.

```js
const promise1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("promise1 failed");
  }, 3000);
});

const promise2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("promise2 succeeded");
  }, 2000);
});

const promise3 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("promise3 failed"); // 1. 여기서 먼저 걸린다 any와 다르게 실패여도 걸림
  }, 1000);
});

Promise.race([promise1, promise2, promise3])
  .then((result) => console.log(result))
  .catch((error) => console.error(error)); // 2. reject니까 에러에 걸려서 promise3 failed 출력
```







## Reference

[MWD Promise](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)

[📚 자바스크립트 Promise 개념 & 문법 정복하기](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EB%B9%84%EB%8F%99%EA%B8%B0%EC%B2%98%EB%A6%AC-Promise)

[자바스크립트 개발자라면 알아야 할 33가지 개념 #25 자바스크립트 : 바보를 위한 Promise](https://velog.io/@jakeseo_me/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%9D%BC%EB%A9%B4-%EC%95%8C%EC%95%84%EC%95%BC-%ED%95%A0-33%EA%B0%80%EC%A7%80-%EA%B0%9C%EB%85%90-25-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%B0%94%EB%B3%B4%EB%A5%BC-%EC%9C%84%ED%95%9C-Promise)

[Understanding JavaScript Promises](https://www.digitalocean.com/community/tutorials/understanding-javascript-promises#toc-new-kid-on-the-block-observables)

[📚 Promise.allSettled와 Promise.all 비교 정리](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EB%8D%94%EC%9D%B4%EC%83%81-Promiseall-%EC%93%B0%EC%A7%80%EB%A7%90%EA%B3%A0-PromiseallSettled-%EC%82%AC%EC%9A%A9%ED%95%98%EC%9E%90)