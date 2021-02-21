# ES6

> 목표: ES6 문법을 살펴보고 이를 이용한 실무의 활용 패턴을 알아봅니다.

자바스크립트는 [ECMAScript](https://ko.wikipedia.org/wiki/ECMA%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8) 사양을 준수하는 언어입니다. 이는 마치 우리가 쓰는 일상 언어와 국립국어원에서 제정하는 표준어 규칙 간의 관계와 비슷하다고 볼 수 있습니다. 표준어 규칙도 필요에 따라 개정하듯이, ECMAScript도 필요에 따라 개정됩니다.

**ES6**는 ECMAScript의 제 6판 표준 사양으로, 2015년도에 재정되어서 ES2015라고 불리기도 합니다. \(ES6 이후부터 새로운 언어 사양이 매년 발표되고 있습니다. 2020년 기준으로 ES11까지 재정되었습니다.\)

ES6를 알아야 하는 이유는, ES6에서 추가된 내용이 [현대적인 자바스크립트 개발자라면 꼭 알아야 할 내용](https://blog.asamaru.net/2017/08/14/top-10-es6-features/)이기 때문입니다. 해당 내용을 나열하자면 아래와 같습니다.

* 블록 범위 생성자 \(Block-Scoped Constructs Let and Const\)
* 화살표 함수 \(Arrow Functions\)
* 클래스 \(Classes\)
* 프로미스 \(Promises\)
* 비구조화 할당 \(Destructuring Assignment\)
* 템플릿 리터럴 \(Template Literals\)
* 향상된 객체 리터럴 \(Enhanced Object Literals\)
* 기본 매개 변수 \(Default Parameters\)
* 멀티 라인 문자열 \(Multi-line Strings\)
* 모듈 \(Modules\)

## 블록 범위 생성자

자바스크립트 변수의 스코프는 크게 두 가지가 있습니다.

> 스코프: 변수에 접근 가능한 범위

1. **함수 레벨 스코프**: 함수 블록 내에서 선언된 변수는 함수 내에서만 접근 가능
2. **블록 레벨 스코프**: 함수를 포함한 모든 코드 블록\(`if` 문, `for` 문, `while` 문, `try/catch` 문 등\) 내에서 선언된 변수는 코드 블록 내에서만 접근 가능

기존의 변수 선언 키워드인 `var`는 함수 레벨 스코프를 가집니다. 이는 함수를 제외한 다른 코드 블럭에서는 개별적인 스코프를 가질 수 없기 때문에 개발자의 의도와 다른 동작을 야기시킬 수 있었습니다. 또한, `var` 키워드는 재선언을 허용하는 문제점도 있었습니다.

ES6에서는 이를 해결하고 더욱 명확한 프로그램 기능을 제공하기 위해 `let`과 `const` 키워드를 추가했습니다.

각 문법의 차이점을 예제로 확인해봅시다.

```javascript
// 문제점 1: 기존의 var 변수는 함수를 제외하고 블록 내부에서 선언 되더라도 외부에서 접근 가능
{
  var one = 1;
}
console.log(one); // 1
console.log(window.one); // 1

// 해결: let, const 변수는 블록 외부에서 호출 불가
{
  let two = 2;
  const three = 3;
}
console.log(two); // Uncaught ReferenceError: two is not defined
console.log(three); // Uncaught ReferenceError: three is not defined

// 문제점 2: var 키워드는 재선언을 허용함
var one = 1;
var one = 1;
var one = 1;
console.log(one); // 1

// 해결: let, const 변수는 재선언 불가능
let two = 2;
let two = 2; // Uncaught SyntaxError: Identifier 'two' has already been declared
const three = 3;
const three = 3; // Uncaught SyntaxError: Identifier 'three' has already been declared

// let vs const 차이점
// let 변수는 새로운 값을 할당할 수 있지만 const 변수는 선언되는 시점에서 할당된 값을 바꿀 수 없음
let two = 2;
two = '2';
const three = 3;
three = '3'; // Uncaught TypeError: Assignment to constant variable
```

## 화살표 함수

[화살표 함수](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)는 ES6에서부터 지원하는 새로운 함수 문법이며, 항상 익명함수입니다. 기존의 `function` 문법에 비해 간결한 것이 특징입니다.

```javascript
// 기존의 function 문법에서 화살표 함수로 치환하기
function (a) {
  return a + 100;
}

// 1. "function" 키워드를 지우고 인자와 함수 몸통 사이에 화살표 넣기
(a) => {
  return a + 100;
}

// 2. 몸통의 중괄호와 "return" 키워드 지우기 (return 생략)
(a) => a + 100;

// 3. 인자의 괄호 지우기 (인자가 하나일 경우에만 생략 가능)
a => a + 100;

// 화살함수 사용 예시
const materials = [
  'Hydrogen',
  'Helium',
  'Lithium',
  'Beryllium'
];

// 아래 두 코드는 정확히 같은 문장
console.log(materials.map(material => material.length)); // [8, 6, 7, 9]

console.log(materials.map((material) => { // [8, 6, 7, 9]
  return material.length
}));
```

## 클래스

많은 프로그래밍 언어에서 클래스라고 하는 객체 추상화 개념을 지원하고 있습니다. 기존의 자바스크립트에서는 이와 비슷한 개념으로 [프로토타입](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)을 지원했지만, 프로토타입에서 아쉬웠던 부분을 해소하기 위해 ES6에서부터 [클래스](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes) 문법을 제공합니다.

```javascript
// 기존의 프로토타입 문법
function Circle (radius) {
  this.radius = radius;
}

Circle.prototype = {
  area: function () {
    return this.radius * this.radius * Math.PI;
  }
}

console.log(new Circle(2).area()); // 12.566370614359172

// 클래스 문법
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
  area() {
    return this.height * this.width;
  }
};

console.log(new Rectangle(5, 8).area()); // 40
```

## 프로미스

기존의 자바스크립트에서는 [비동기 흐름](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Asynchronous/Introducing)을 제어할 수 있는 방법이 다양하지 않았습니다. 하지만, ES6부터 [Promise](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise) 문법을 지원하여 비동기 흐름을 더욱 효과적이고 쉽게 제어할 수 있도록 기능을 제공하고 있습니다.

> 참고: 비동기 흐름 제어를 위해 아래와 같은 문법도 사용 가능합니다.
>
> * [제너레이터 함수 \(ES6+\)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/function*)
> * [Async 함수 \(ES8+\)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/async_function)

### 콜백 지옥

> callback: 함수에 함수를 인자로 전달하고, 해당 함수가 실행된 뒤 마지막에 인자로 전달된 함수를 실행하는 것.

기존의 자바스크립트에서는 비동기 흐름을 컨트롤하기 위해 이벤트 혹은 콜백 개념을 사용했습니다. 하지만, 콜백의 사용은 [콜백 지옥 \(callback hell\)](http://callbackhell.com/)이라고 불리는 문제점을 발생시켰으며 이로 인해 복잡성이 증가하고 가독성이 떨어지는 코드가 만들어지곤 했습니다.

콜백 지옥이 무엇인지 아래 예제로 확인해보겠습니다.

```javascript
// API 호출을 위한 wrapper 함수
function request(url, callback) {
  let xhr = new XMLHttpRequest();
  xhr.open('GET', url);
  xhr.onload = function() {
    callback(JSON.parse(xhr.response));
  };
  xhr.send();
}

const baseUrl = 'https://jsonplaceholder.typicode.com'

// 1. 유저 정보 가져오기
request(`${baseUrl}/users/1`, (user) => {
  // 2. 유저의 포스트 정보 가져오기
  request(`${baseUrl}/posts?userId=${user.id}`, (posts) => {
    // 3. 포스트의 코멘트 정보 가져오기
    request(`${baseUrl}/comments?postId=${posts[0].id}`, (comments) => {
      // 4. 최종 코드 수행
      console.log(`comments length: ${comments.length}`)
    })
  })
})
```

### 프로미스 체이닝

프로미스는 콜백 지옥을 완벽하게 해결할 수 있도록 도와줍니다. 아래는 프로미스를 도입하여 콜백 지옥을 어떻게 해결하는지 보여주는 예제입니다.

```javascript
// API 호출을 위한 wrapper 함수
function request(url, callback) {
  let xhr = new XMLHttpRequest();
  xhr.open('GET', url);
  xhr.onload = function() {
    callback(JSON.parse(xhr.response));
  };
  xhr.send();
}

// Promise wrapper 함수
function requestPromise(url) {
  return new Promise((resolve, reject) => {
    request(url, resolve)
  })
}

const baseUrl = 'https://jsonplaceholder.typicode.com'

requestPromise(`${baseUrl}/users/1`)
  .then(user => requestPromise(`${baseUrl}/posts?userId=${user.id}`))
  .then(posts => requestPromise(`${baseUrl}/comments?postId=${posts[0].id}`))
  .then(comments => console.log(`comments length: ${comments.length}`))
  .catch(err => console.error(err))
```

## 비구조화 할당

[비구조화 할당](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)은 ES6부터 도입된 문법으로, 배열 혹은 객체 타입의 변수 내부 속성을 해체하여 변수에 할당하는 방법입니다.

```javascript
const person = {
  name: {
    firstName: 'Youngkyun',
    lastName: 'Kim'
  },
  country: 'South Korea',
  gender: 'Male'
}

const counts = [1, 2, 3]

// 기존의 객체, 배열 할당 방식
const firstName = person.name.firstName;
const lastName = person.name.lastName;
const gender = person.gender;
console.log(`${firstName} ${lastName} is a ${gender}`);

const one = counts[0]
const two = counts[1]
const three = counts[2]
console.log(`one + two + three = ${one + two + three}`);

// 비구조화 객체, 배열 할당 문법
const { name: { firstName, lastName }, gender } = person;
console.log(`${firstName} ${lastName} is a ${gender}`);

const [one, two, three] = counts;
console.log(`one + two + three = ${one + two + three}`);

// 비구조화 문법으로 함수 인자에도 적용 가능
const sayHello = ({ name: { firstName, lastName }, country }) => {
  return `Hello, ${firstName} ${lastName} from ${country}!`
}
console.log(sayHello(person)); // Hello, Youngkyun Kim from South Korea!
```

### 비구조화 할당을 활용한 RORO 패턴

RORO \(Receive Object Return Object\) 패턴은 비구조화 할당 문법을 활용한 자바스크립트 프로그래밍 패턴 중 하나입니다. 해당 패턴을 통해 [**명명된 매개변수\(Named Parameter\)**](https://en.wikipedia.org/wiki/Named_parameter)라는 프로그래밍 개념을 유사하게 구현할 수 있으며, 아이디어의 구체적인 내용은 [해당 블로그 포스트](https://www.freecodecamp.org/news/elegant-patterns-in-modern-javascript-roro-be01e7669cbd/)에서 확인할 수 있습니다.

RORO 패턴의 사용법은 아래와 같습니다.

```javascript
// Bad
// 각 전달하는 인자가 어떤 역할을 하는건지 알기 어렵다.
addNewControl("Title", 20, 50, 100, 50, true);

// Good
// 인자의 기능을 명확하게 이해할 수 있다.
addNewControl({
  title: "Title",
  xPosition: 20,
  yPosition: 50,
  width: 100,
  height: 50,
  drawingNow: True
})

// 비구조화 할당을 문법을 활용해 구현된 RORO 패턴 함수 선언
function addNewControl({
  title,
  xPosition: xAxis, // alias 가능
  yPosition,
  width=200, // default 값 지정 가능
  height,
  drawingNow
}) {
  // code here
}
```

## 템플릿 리터럴

[템플릿 리터럴](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals)은 문자열 내부에 자바스크립트 표현식을 넣을 수 있는 문법입니다.

```javascript
const now = new Date().toString()

// 기존 방식
console.log('안녕하세요.\n지금 시각은 ' + now + ' 입니다.');

// 템플릿 리터럴 사용
console.log(`안녕하세요.\n지금 시각은 ${now} 입니다.`);
```

## 향상된 객체 리터럴

기존의 객체 정의 문법\([객체 초기자](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Object_initializer)\)에서 조금 더 간결하게 사용할 수 있도록 개선된 문법입니다.

```javascript
const name = {
  firstName: 'Youngkyun',
  lastName: 'Kim',
}
const city = 'seoul'
const gender = 'male'

// 기존의 객체 정의 문법
const person = {
  name: {
    firstName: firstName,
    lastName: lastName,
  },
  city: city,
  gender: gender,
  sayHello: function() {
    return `Hello, ${firstName} ${lastName} from ${country}!`
  },
}

// 향상된 객체 정의 문법
// 1. 속성과 값이 같으면 축약 가능
// 2. 무명함수가 값일 경우 function 키워드 생략 가능
const person = {
  name: {
    firstName,
    lastName
  },
  city,
  gender,
  sayHello() {
    return `Hello, ${firstName} ${lastName} from ${country}!`
  },
}
```

