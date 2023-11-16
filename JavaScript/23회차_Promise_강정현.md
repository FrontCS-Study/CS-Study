# Promise
자바스크립트 비동기 처리에 사용되는 객체.

콜백 패턴이 갖는 단점인 콜백 헬을 극복하고, 비동기 처리 시점을 명확하게 표현하기 위한 방법으로, ES6에서 프로미스(Promise)를 도입하였음.

<br/>

## 문법
```javascript
let promise = new Promise(function(resolve, reject) {
	// executor
	if (비동기 작업 수행 성공) {
		resolve('result');
	} else {
		reject('failed');
	}
});
```
- `new Promise`에 전달되는 콜백 함수는 **excutor(실행자, 실행 함수)** 라고 부르며, `new Promise`가 만들어질 때 자동으로 실행된다.
- excutor의 인수는 `resolve`, `reject` 이며, **excutor의 처리 성공 여부**에 따라 자바스크립트에서 자체 제공하는 콜백. 
	- `resolve(value)` - 일이 성공적으로 끝난 경우, 그 결과를 나타내는 `value`와 함께 호출.
	- `reject(error)` - 에러 발생 시 에러 객체를 나타내는 `error` 와 함께 호출.

`new Promise` 생성자가 반환하는 `promise` 객체는 다음과 같은 내부 프로퍼티를 갖는다:
- `state` - 처음엔 `pending`(보류) 상태였다가, `resolve`가 호출되면 `fulfilled`, `reject`가 호출되면 `rejected`로 변함.
- `result` - 처음엔 `undefined` 상태였다가, `resolve(value)`가 호출되면 `value`로, `reject(error)`가 호출되면 `error`로 변함.

<br/>

## 사용 예시
```javascript
let promise = new Promise(function(resolve, reject) {
	setTimeout(() => resolve("완료"), 1000);
});
```
- executor 처리가 시작 된 지 1초 후, `resolve("완료")`이 호출되고 결과가 만들어지며, `promise` 객체의 상태는 `state: "fulfilled"`, `result: "완료"` 가 된다.
```javascript
let promise = new Promise(function(resolve, reject) {
	setTimeout(() => reject(new Error("에러 발생.")), 1000);
});
```
- excutor 처리가 시작된 지 1초 후, `reject(...)`가 호출되면, `promise` 객체의 상태는 `state: "rejected"`, `result: Error 객체`가 된다.

<br/>

## Promise 핸들러
생성된 Promise 객체는 비동기 작업이 완료된 이후에 다음 작업을 연결시켜 진행할 수 있다.
작업 결과에 따라 `.then()`과 `.catch()` 메소드 체이닝을 통해 성공과 실패에 대한 후속 처리를 진행할 수 있다.
### then
```javascript
promise.then(
	function(result) {/* 결과 */},
	function(error) {/* 에러 */}
)
```
- `.then`의 첫 번째 인수는 `promise`가 이행되었을 때 실행되며, 실행 결과를 받는다.
- `.then`의 두 번재 인수는 `promise`가 거부되었을 때 실행되며, 에러를 받는다.
### catch
Promise의 에러를 처리할 수 있는 메소드. 
```javascript
function getData() {
	return new Promise(function(resolve, reject){
		reject('failed');
	});
}

getData().then().catch(function(err){
	console.log(err);
})
```
#### then 보다 catch로 에러를 처리하자
```javascript
getData().then(function(result) {
	console.log(result);
	throw new Error("Error in then()"); // Uncaught (in promise) Error
}, function(err) {
	console.log('then error: ', err);
});
```
`then()`의 첫 번째 콜백 함수 내부에서 오류가 나는 경우, 오류를 제대로 잡아내지 못함.
```javascript
getData().then(function(result){
	console.log(result);
	throw new Error("Error in then()");
}).catch(function(err){
	console.log("then error: ", err); // then error : Error : Error in then()
})
```
`then()`에서 발생한 오류를 잡아낼 수 있음.
### finally
Promise의 이행 및 거부와 관계 없이 실행할 콜백 함수를 등록하고, 새로운 Promise를 반환.

<br/>

## Promise 함수 등록
프로미스 객체를 변수에 바로 할당하는 방식을 사용할 수 있지만, 별도로 함수에 감싸서 사용하는 것이 일반적.
```javascript
function myPromise() {
	return new Promise((resolve, reject) => {
		if (성공조건) {
			resolve(결과값);
		} else {
			reject(에러값);
		}
	});
}

myPromise()
	.then((result) => {
		// 성공 시 실행할 콜백 함수
	})
	.catch((error) => {
		// 실패 시 실행할 콜백 함수
	})
```
함수를 만들고, 그 함수를 호출하면 Promise 생성자를 return 함으로써, 곧바로 생성된 Promise 객체를 함수 반환값으로 얻어 사용한다.

**장점**

1. 재사용성: 프로미스 객체를 필요할 때마다 호출하여 사용함으로써, 반복되는 비동기 작업을 효율적으로 처리할 수 있다.
2. 가독성: 코드의 구조가 명확해져, 비동기 작업의 정의와 사용을 분리하여 코드의 가독성을 높일 수 있다.
3. 확장성: 인자를 전달하여 동적으로 비동기 작업을 수행할 수 있다. 또한, 여러 개의 Promise 객체를 반환하는 함수들을 연결하여 복잡한 비동기 로직을 구현할 수 있다.

<br/>

## Promise Chaining
여러 개의 Promise 핸들러를 연결하여, 비동기 작업을 순차적으로 수행할 수 있다.
```javascript
new Promise(function(resolve, reject) {
	setTimeout(function() {
		resolve(1);
	}, 2000);
})
.then(function(result) {
	console.log(result); // 1
	return result + 10;
})
.then(function(result) {
	console.log(result); // 11
	return result + 20;
})
.then(function(result) {
	console.log(result); // 31
})
```
만일 `.catch` 다음으로 `.then`이 이어서 체이닝 되어 있다면, 에러가 처리되고 가까운 `.then`으로 제어 흐름이 넘어가 실행이 이어진다.
```javascript
new Promise((resolve, reject) => {
	throw new Error("에러 발생!");
})
.catch(function(error){
	console.log("에러 처리. 정상적으로 실행이 이어짐.");
})
.then(() => {
	console.log("다음 핸들러가 실행됨.");
})
.then(() => {
	console.log("그 다음 핸들러가 실행됨.");
})
```

<br/>

## Promise 정적 메소드
Promise는 주로 생성자 함수로 사용되지만, 정적 메소드를 제공한다.
### Promise.resolve/ Promise.reject
Promise.resolve와 Promise.reject 메소드는 존재하는 값을 Promise로 래핑하기 위해 사용함.
Promise.resolve는 인자로 전달된 값을 resolve 하는 Promise를 생성한다.
```javascript
function getRandomNumber() {
	const num = Math.floor(Math.random() * 10);
	return num;
}

function getPromiseNumber() {
	const num = getRandomNumber();
	return Promise.resolve(num);
}

getPromiseNumber()
.then((value) => {
	console.log(`랜덤 숫자: ${value}`);
})
.catch((error) => {
	console.log(error);
});
```
Promise.reject 메소드는 인자로 전달된 값을 reject하는 Promise를 생성한다.
```javascript
const p = Promise.reject(new Error('error'));
p.catch(error => console.log(error));
```

### Promise.all()
배열, Map, Set에 포함된 여러 개의 Promise 요소들을 한꺼번에 비동기 작업을 처리할 때 사용할 수 있다.
모든 Promise 비동기 처리가 이행(fulfilled)될 때까지 기다려서, 모든 Promise가 완료되면 그 때 `.then` 을 실행한다.
```javascript
// 1. 서버 요청 API 프로미스 객체 생성 (fetch)
const api_1 = fetch("https://jsonplaceholder.typicode.com/users");
const api_2 = fetch("https://jsonplaceholder.typicode.com/users");
const api_3 = fetch("https://jsonplaceholder.typicode.com/users");

// 2. 프로미스 객체들을 묶어 배열로 구성
const promises = [api_1, api_2, api_3];

// 3. Promise.all()
Promise.all(promises)
.then((results) => {
	// results는 이행된 Promise들의 값들을 담은 배열.
	// results의 이행 순서는 promises의 순서와 일치.
	console.log(results);
})
.catch((error) => {
	// 어느 하나라도 Promise가 거부되면 에러 출력.
	console.log(error);
})
```
### 기타 정적 메소드
- Promise.allSettled() : 주어진 모든 Promise가 처리되면 모든 Promise 각각의 상태와 값 (또는 거부 사유) 을 모아놓은 배열을 반환함.
- Promise.any() : 주어진 모든 Promise 중 하나라도 완료되면, 바로 반환함.
	- 첫 번째로 '이행'된 Promise만을 취급하며, 나머지 Promise가 있다면 이는 무시된다.
	- 요청된 모든 Promise가 거부되면, AggregateError 객체를 사유로 하는 거부 Promise를 반환한다.
- Promise.race() : Promise.any()와 달리, 이행/거부의 여부 상관 없이 무조건 처리가 끝난 Promise 결과값을 반환한다.

---

**Reference**

[프라미스](https://ko.javascript.info/promise-basics)

[자바스크립트 Promise 쉽게 이해하기](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/)

[자바스크립트 Promise 개념 & 문법 정복하기](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EB%B9%84%EB%8F%99%EA%B8%B0%EC%B2%98%EB%A6%AC-Promise)

[프로미스](https://poiemaweb.com/es6-promise)
