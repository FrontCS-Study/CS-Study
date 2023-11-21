# 제너레이터
## 제너레이터 함수
`function*`
```javascript
function* generateSequence() {
	yield 1;
	yield 2;
	return 3;
}
```

제너레이터 함수를 호출하면 코드가 실행되지 않고, 실행을 처리하는 **'제너레이터 객체'가 반환**됨.
```javascript
let generator = generateSequence();
console.log(generator); // [object Generator]
```

## 제너레이터 메소드
### next()
- `value` : 산출값
- `done` : 함수 코드 실행이 끝났으면 `true`, 아니라면 `false`
```javascript
let generator = generateSequence();
let one = generator.next();
console.log(Json.stringify(one)); // {value: 1, done: false};
```

### return()
주어진 값을 반환하고, 제너레이터를 종료함.
이후 `.next()`를 통해 제너레이터를 호출하면 `done: true` 상태를 유지하고, `value: undefined`를 반환.
- `value` : 반환할 값. 
```javascript
generatorObject.return(value);
```

**반환값**
- `value` : 산출값
- `done` : `true` (`finally` 블록에 `yield`가 있는 경우,  `false`)

### throw()
제너레이터에 오류를 발생시킴.
- `exception`: 발생시킬 예외
```javascript
generatorObject.throw(exception);
```

## 제너레이터와 이터러블
제너레이터 객체는 `Iterable` 객체이므로, `for...of` 반복문을 통해 값을 얻을 수 있다.
```javascript
let generator = generateSequence();
for (let value of generator) {
	console.log(value); // 1, 2 출력
}
```
- `for...of` 반복문은 이터레이션의 상태가 `done:true` 일 때는 마지막 `value`는 무시하게 되며, 따라서 `return 3`를 출력하지 않는다.
- 모든 값을 출력하기 위해선 `return 3` -> `yield 3`로 변경한다.

전개 문법(`...`) 또한 사용할 수 있다.
```javascript
let sequence = [0, ...generateSequence()];
console.log(sequence); // 0, 1, 2, 3
```

## 제너레이터 컴포지션
제너레이터 안에 제너레이터를 임베딩(embeding, composing) 할 수 있게 해주는 기능.
```javascript
function* generateSequence(start, end) {
	for (let i = start, i <= end; i++) yield i;
}

function* generatePasswordCodes() {
	yield* generateSequence(48, 57); // 0..9
	yield* generateSequence(65, 90); // A..Z
	yield* generateSequence(97, 122); // a..z
}

let str = '';

for (let code of generatePasswordCodes()) {
	str += String.fromCharCode(code);
}

alert(str); // 0..9A..Za..z
```
- `yield*` 지시자는 실행은 다른 제너레이터에 위임함.
	- `yield* gen` 이 제너레이터 `gen`을 대상으로 반복을 수행하고, **산출값들을 바깥으로 전달**한다.
- 중간 결과 저장 용도의 추가 메모리가 필요하지 않음.

## 비동기 처리
### 제너레이터 사용하여 구현
```javascript
function getUser(getObj, username) {
	fetch(`https://api.github.com/users/${username}`)
	.then(res => res.json())
	.then(user => genObj.next(user.name)); // (1)
}

const g = (function* () {
	let user;
	user = yield getUser(g, 'jeresig'); // (2)
	console.log(user);

	user = yield getUser(g, 'ahejlsberg');
	console.log(user);

	user = yield getUser(g, 'ungmo2');
	console.log(user);
}());

// 제너레이터 함수 시작
g.next();
```
1. 비동기 처리가 완료되면 `next` 메소드를 통해 제너레이터 객체에 비동기 처리 결과를 전달한다.
2. 제너레이터 객체에 전달된 비동기 처리 결과는 `user` 변수에 할당된다.

### async/await 를 사용하여 구현
```javascript
function getUser(getObj, username) {
	fetch(`https://api.github.com/users/${username}`)
	.then(res => res.json())
	.then(user => genObj.next(user.name)); // (1)
}

async function getUserAll() {
	let user;
	user = await getUser('jeresig');
	console.log(user);

	user = await getUser('ahejlsberg');
	console.log(user);

	user = await getUser('ungmo2');
	console.log(user);
}

getUserAll();
```

<br/>

# async/await
## async 함수
```javascript
async function f() {
	return 1;
}
```
function 앞에 `async`를 붙이면, 해당 함수는 항상 Promise를 반환한다.
Promise가 아닌 값을 반환하더라도, **이행 상태(resolved)의 Promise로 값을 감싸** 이행된 Promise가 반환되도록 한다.
```javascript
f().then(alert); // 1
```

## await
```javascript
let value = await promise;
```
- `await`는 async 함수 안에서만 동작함.
- `await` 키워드를 만나면, Promise가 처리될 때까지 기다림.
- Promise가 처리되면 그 결과와 함께 실행이 재개되며, 기다리는 동안 엔진이 다른 일을 할 수 있기 때문에 CPUI 리소스가 낭비되지 않는다.
```javascript
async function f() {
	let promise = new Promise((resolve, reject) => {
		setTimeout(() => resolve("완료"), 1000)
	});
	let result = await promise; // Promise가 이행될 때까지 기다림
	console.log(result); // "완료!"
}
f();
```
1. `f()`가 호출됨.
2. `await` 키워드로 인해 실행이 잠시 중단되었다가, Promise가 처리되면 실행이 재개됨.
3. Promise 객체의 `result` 값이 변수 `result`에 할당됨.
4. `result` 값이 출력됨.

## 에러 핸들링
1. try/catch
```javascript
async function f() {
	try {
		let response = await fetch('http://유효하지-않은-주소');
	} catch(err) {
		alert(err);
	}
}
f();
```

2. .catch
```javascript
async function f() {
	let response = await fetch('http://유효하지-않은-주소');
}
f().catch(alert);
```

## Promise to async/await
### Promise
```javascript
function loadJson(url) {
	return fetch(url)
	.then(response => {
		if (response.status == 200) return response.json();
		else throw new Error(response.status);
	})
}
loadJson('no-such-user.json').catch(alert); // Error:404;
```
### async/await
```javascript
async function loadJson(url) { 
	let response = await fetch(url); 
	if (response.status == 200) {
		let json = await response.json(); // *대체 가능
		return json;
	} 
	throw new Error(response.status);
}
loadJson('no-such-user.json').catch(alert); // Error:404 
```

```javascript
if (response.status == 200) return response.json(); 
```
- 위 `json`을 return 하는 코드를 대체할 수 있음.
- 단, Promise가 이행되는 것을 `await`을 사용해 바깥 코드에서 기다려야 함.

---
**Reference**

[제너레이터](https://ko.javascript.info/generators)

[Generator](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Generator)

[제너레이터와 async/await](https://poiemaweb.com/es6-generator)

[async와 await](https://ko.javascript.info/async-await)
