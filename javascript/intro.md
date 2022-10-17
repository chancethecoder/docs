---
description: '목표: 자바스크립트의 개념과 문법, Web API를 간단히 살펴봅니다.'
---

# 자바스크립트 개요

[자바스크립트](https://en.wikipedia.org/wiki/JavaScript)는 **사용자와 웹 페이지 간의 상호작용**을 구현하기 위해 만들어진 프로그래밍 언어입니다. 1995년도에 [브렌던 아이크](https://en.wikipedia.org/wiki/Brendan_Eich)에 의해 최초로 개발 되었으며, 인터넷과 브라우저의 발전 역사와 함께 다양한 변화를 겪었습니다.

최초 개발 이후, 월드와이드웹의 성공과 함께 자바스크립트의 인기는 폭발적으로 늘어 다양한 분야에서 사용 가능하도록 발전했습니다. 현재 자바스크립트를 사용하면 아래와 같은 일들을 할 수 있습니다.

* 웹 페이지 내 인터렉티브 동작
* [서버 프로그래밍 \(Nodejs\)](https://nodejs.org/)
* [모바일 어플리케이션 \(React Native\)](https://reactnative.dev/)
* [데스크탑 어플리케이션 \(Electron\)](https://www.electronjs.org/)
* [터미널 명령어 도구 작성](https://github.com/tj/commander.js) 

## 기본 문법

{% hint style="success" %}
Tip: 코드 실행하기

아래에서 배울 자바스크립트 코드는 브라우저의 개발자 도구를 통해서 연습해볼 수 있습니다. [크롬의 브라우저 개발자 도구](https://developers.google.com/web/tools/chrome-devtools?hl=ko)를 여는 방법은 아래와 같습니다.

* 윈도우: `ctrl` + `shift` + `I`
* 맥: `cmd` + `opt` + `I`
{% endhint %}

### 주석

```javascript
// 주석입니다.

/*
* 다중 열 주석입니다.
* 주석은 프로그램에 아무런 영향을 주지 않습니다.
* 코드에 적절하게 주석을 달아주는 것은 좋은 습관입니다.
*/
```

### 출력

```javascript
console.log('Hello!'); // Hello!
```

### 변수

```javascript
// 문자열
var greeting = 'Hello';

// 블록 스코프 변수 (ES6+)
let mutable = 'This is mutable block-scoped variable';
const immutable = 'This is immutable block-scoped variable';

// 숫자
let twoHundred = 200; // Integer
let pi = 3.141592; // Float

// 배열
let items = [1, 'two', []];

// 배열의 원소 접근하기
console.log(items[1]) // two

// 객체
let person = {
  name: {
    firstName: 'Youngkyun',
    lastName: 'Kim'
  },
  country: 'South Korea',
  gender: 'Male'
}

// 객체 속성 접근하기
console.log(person.gender); // Male
console.log(person.name.lastName); // Kim
```

### 함수

```javascript
function add (A, B) {
  return A + B;
}

// 무명 함수
// 함수를 변수에 할당도 가능
let add2 = function (A, B) {
  return A + B;
}

// 화살 함수 (ES6+)
let subtract = (A, B) => {
  return A - B;
}

// 제너레이터 함수
function* incremental(){
  let index = 0;
  while(true)
    yield index++;
}
const gen = incremental();

// 함수 호출하기
console.log(subtract(add(10, 20), 5)); // 25

console.log(gen.next().value); // 0
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
```

### 조건문

```javascript
function isGreaterThanFifty (value) {
  if (value > 50) {
    return true;
  }
  return false;
}

console.log(isGreaterThanFifty(49)); // false
console.log(isGreaterThanFifty(51)); // true
```

### 반복문

```javascript
let companies = ['Apple', 'Google', 'Amazon'];

// for 반복문
for(let i=0; i < companies.length; i++) {
  console.log(companies[i])
}

// for...of 반복문 (ES6+)
for(let company of companies) {
  console.log(company)
}

// while 반복문
let j = 0;
while(true) {
  if (j >= 3) break;
  console.log(companies[j++]);
}

// do while 반복문
let k = 0;
do {
  console.log(companies[k++]);
} while (k < 3)
```

### 연산자

```javascript
// 단항 연산자
let x = 0;
console.log(x += 10);  // 10
console.log(x -= 5);   // 5
console.log(x *= 20);  // 100
console.log(x /= 4);   // 25
console.log(x %= 23);  // 2
console.log(x **= 10); // 1024

// 이항 연산자
console.log(100 + 1); // 101
console.log('100' + 1); // 1001
console.log('Hello' + ' ' + 'there'); // Hello there
console.log(100 - 1); // 99
console.log('100' - 1); // 99
console.log('Hello' - 'there'); // NaN
console.log(5 * 4); // 20
console.log(20 / 3); // 6.666666666666667

// 비교 연산자
console.log(1 == '1');  // true
console.log(1 != '1');  // false
console.log(1 === '1'); // false
console.log(1 === 1);   // true
console.log(1 !== 1);   // false
console.log(1 < 2);     // true

// 논리 연산자
console.log(true && true);    // true
console.log(true && false);   // false
console.log(false && false);  // false
console.log(true || true);    // true
console.log(true || false);   // true
console.log(false || false);  // false

// 삼항 연산자
let age = 25
let isAdult = (19 < age) ? 'adult' : 'minor';
```

### 에러 핸들링

```javascript
const divide = function (A, B) {
  if (B == 0) {
    // throw 문법
    throw new Error('Divide by zero is not acceptable');
  }
  return A / B;
}

// try-catch 문법
try {
  let dividedByZero = divide(100, 0);
  console.log('divided by 0 success.');
} catch (err) {
  // Error handling
  console.error(err);
}

let dividedByZero = 100 / 0; // Error: Divide by zero is not acceptable
```

## Web API

[Web API](https://developer.mozilla.org/ko/docs/Web/API)는 자바스크립트를 통해 브라우저의 유용한 작업들을 수행할 수 있도록 정의된 API의 모음입니다. 가장 흔하게 사용되는 API로 DOM이 있으며, 이외에도 디바이스 정보, 데이터 저장, 통신 등 다양한 기능을 수행하기위한 API들이 정의되어있습니다.

다음은 Web API에서 중요한 항목들을 간략하게 살펴보겠습니다.

### DOM \(Document Object Model\)

HTML은 계층 구조를 가지며, 하나의 [노드 트리](https://ko.javascript.info/dom-nodes)로 표현될 수 있습니다. 이러한 논리적인 HTML 문서 트리를 자바스크립트 상에서 컨트롤 가능한 API로 제공하는 것이 [DOM](https://developer.mozilla.org/en-US/docs/Glossary/DOM)입니다.

모든 DOM API는 `document`로 시작하며, 이는 모든 브라우저에서 동일합니다. 아래에서 DOM을 사용하는 코드 예시를 살펴보겠습니다.

```javascript
// 1. DOM 객체 확인하기
document      // #document
document.body // <body ...>...</body>
document.body.lastElementChild // ...

// 2. <script> 태그를 삽입하여 코드를 실행해보기
let script = document.createElement('script');
script.src = './';
script.type = 'text/javascript';
script.onload = function () {
  alert('스크립트 바인딩 완료. 페이지를 클릭해보세요.')
  let clickCount = 3;
  let clickCallback = function (e) {
    if (clickCount < 1) {
      alert('스크립트 바인딩을 삭제합니다.')
      document.body.removeEventListener('click', clickCallback);
      script.parentNode.removeChild(script);
    } else {
      alert(`남은 클릭 횟수: ${clickCount}`);
    }
    clickCount--;
  };
  document.body.addEventListener('click', clickCallback);
}
document.getElementsByTagName('head')[0].appendChild(script);
```

### BOM \(Browser Object Model\)

브라우저와 상호작용하며 브라우저를 컨트롤할 수 있도록 제공되는 API 모음이 [BOM](https://www.w3schools.com/js/js_window.asp)입니다.

모든 BOM API는 `window`로 시작하며, 이는 모든 브라우저에서 동일합니다. 또한, `window`는 전역 객체이며 프로퍼티나 메소드를 사용할 때 `window`를 생략하여 사용할 수 있습니다.

```javascript
// 1. BOM 객체 확인
window
window.alert          // 브라우저 네이티브 알림 창 
window.location       // 브라우저의 현재 주소 값
window.screen         // 브라우저 창의 크기 값
window.history        // 브라우저 이동 히스토리 (뒤로가기, 앞으로 가기)
window.setTimeout     // 일정 시간 이후에 콜백 함수를 비동기적으로 실행시켜주는 함수
window.setInterval    // 일정 시간 이후에 콜백 함수를 비동기적으로 실행시켜주는 함수

// 2. location, history를 이용해 브라우저 주소 이동 해보기
location.href = 'https://google.com' // google로 페이지 이동
history.back() // 이전 페이지로 이동

const state = { 'page_id': 1, 'user_id': 5 }
const title = ''
const url = 'hello-world.html'

history.pushState(state, title, url) // history에 내용 추가
```

### 스토리지

프로그램을 구현하기 위해서 꼭 필요한 요소 중 하나는 저장소입니다. 자바스크립트에서 이를 구현할 수 있는 기능은 브라우저의 스토리지 관련 API를 사용하는 것입니다. 브라우저의 스토리지는 아래와 같은 API를 제공합니다.

* localStorage
* sessionStorage
* cookie

```javascript
// HTML5+
window.localStorage
window.sessionStorage
document.cookie

// localStorage 사용 예시
// sessionStorage도 문법은 동일하나, 윈도우나 브라우저 탭을 닫을 경우 데이터가 삭제됩니다.
localStorage.length
localStorage.setItem('foo', 'bar') // 로컬스토리지에 값 저장
localStorage.getItem('foo') // 값 불러오기
```

### 서버 통신

{% hint style="info" %}
Tip: 자바스크립트의 서버 통신을 살펴보기 전에 아래의 개념들을 먼저 이해하는것이 좋습니다.

* [AJAX\(Asynchronous JavaScript And XML\)](https://developer.mozilla.org/ko/docs/Glossary/AJAX)
* [JSON\(Javascript Object Notation\)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON)
{% endhint %}

자바스크립트는 전통적으로 브라우저 내의 클라이언트사이드 언어로 사용되었습니다. 여기서 필요한 핵심 기능 중 하나는 서버와의 통신입니다. 자바스크립트에서 통신 기능을 제공하기 위해 아래와 같은 Web API들을 제공합니다.

* [XMLHttpRequest](https://developer.mozilla.org/ko/docs/Web/API/XMLHttpRequest)
* [Fetch](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API)

아래에서 각각의 코드 예시를 통해 살펴보겠습니다.

#### XMLHttpRequest

```javascript
// Example
var oReq = new XMLHttpRequest();
oReq.addEventListener('load', function () {
    console.log(this.responseText);
});
oReq.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');
oReq.send();
```

#### Fetch API

```javascript
// Example
fetch('https://jsonplaceholder.typicode.com/todos/1')
  .then(response => response.json())
  .then(json => console.log(json))
  .catch(console.err)
```

