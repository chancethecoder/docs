# 모던 자바스크립트

> 목표: 모던 자바스크립트(ES6+) 문법을 살펴보고 이해합니다.

자바스크립트는 [ECMAScript](https://ko.wikipedia.org/wiki/ECMA%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8)의 구현체입니다. ECMAScript는 표준 규격이며, 이를 각 판에서 명세로 지정하고 있습니다. 여기서 ECMAScript의 제 6판에 해당하는 명세가 바로 **ES6**입니다. (ES6는 2015년도에 제정되었으므로 ES2015로 부르기도 합니다.)

ES6에서 추가된 새로운 문법은 자바스크립트 개발자라면 꼭 알아야 할 내용입니다. 간단하게 나열하자면 아래와 같습니다.

- 기본 매개 변수 (Default Parameters)
- 템플릿 리터럴 (Template Literals)
- 멀티 라인 문자열 (Multi-line Strings)
- 비구조화 할당 (Destructuring Assignment)
- 향상된 객체 리터럴 (Enhanced Object Literals)
- 화살표 함수 (Arrow Functions)
- Promises
- 블록 범위 생성자 Let 및 Const (Block-Scoped Constructs Let and Const)
- 클래스 (Classes)
- 모듈 (Modules)

참고할만한 문서
- [JavaScript와 ECMAScript는 무슨 차이점이 있을까?](https://wormwlrm.github.io/2018/10/03/What-is-the-difference-between-javascript-and-ecmascript.html)
- [개발자가 필히 알아야 할 ES6 10가지 기능](https://blog.asamaru.net/2017/08/14/top-10-es6-features/)

## 변수 선언 및 스코프

변수의 스코프와 접근성에 대해서 알아봅니다.

### 함수 스코프 vs 블록 스코프

자바스크립트의 스코프는 크게 함수 레벨 스코프, 블록 레벨 스코프가 존재합니다.

- 함수 레벨 스코프(Function-level scope): 함수 내에서 선언된 변수는 함수 내에서만 유효하며 함수 외부에서는 참조할 수 없음
- 블록 레벨 스코프(Block-level scope): 모든 코드 블록(함수, if 문, for 문, while 문, try/catch 문 등) 내에서 선언된 변수는 코드 블록 내에서만 유효하며 코드 블록 외부에서는 참조할 수 없음

### let, const

ES6에서는 스코프 문제를 해결함과 동시에, 명확한 변수 사용을 위해 선언에 대한 두 가지 문법(let, const)이 추가되었습니다. 각 차이점을 예제로 확인해봅시다.

```javascript
// 기존의 var 방식
if (true) {
  var location = "seoul";
}
console.log(location); // seoul

// let 방식
if (true) {
  let location = "seoul";
}
console.log(foo); // Uncaught ReferenceError: foo is not defined
                  // 블록 스코프 개념을 추가 함으로써 변수의 오용을 막고 프로그램 버그를 줄일 수 있습니다.

// const 방식
if (true) {
  const gender = "male";
  gender = 100;      // Uncaught TypeError: Assignment to constant variable
                     // 바뀌면 안되는 값을 상수로 지정함으로써 명확한 의도대로 코딩하고 버그를 줄일 수 있습니다.
}
console.log(gender); // Uncaught ReferenceError: foo is not defined
```

## 화살함수

ES6에서부터는 흔하게 사용되는 함수 형태로 [화살함수](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) 문법을 지원합니다.

화살함수는 항상 익명함수이며, functions 문법에 비해 짧다는 것이 특징입니다.

```javascript
const materials = [
  'Hydrogen',
  'Helium',
  'Lithium',
  'Beryllium'
];

// 화살함수 예시 1
// 익명 화살함수를 그대로 인자로 사용
console.log(materials.map(material => material.length)); // [8, 6, 7, 9]

// 화살함수 예시 2
// 화살함수를 변수에 할당하여 사용
const getLength = (str) => str.length;
console.log(materials.map(getLength)); // [8, 6, 7, 9]
```

## 클래스

많은 프로그래밍 언어에서 클래스라고 하는 추상화 개념을 지원하고 있습니다. 기존의 자바스크립트에서는 이와 비슷한 개념으로 [프로토타입](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)을 지원했는데요, 프로토타입에서 아쉬웠던 부분을 해소하기 위해 ES6에서부터 [클래스](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes) 문법을 제공합니다.

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

## 비동기 흐름 제어

기존의 자바스크립트에서는 비동기 흐름을 제어할 수 있는 방법이 다양하지 않았습니다. ES6에서부터는 비동기 흐름을 더욱 쉽게 제어할 수 있도록 아래와 같은 방법들을 제공합니다.

### Promise

```javascript
```

### Async/Await

```javascript
```

### 비동기 흐름에서의 에러 핸들링

```javascript
```

## 비구조화 할당

```javascript
const person = {
  name: {
    firstName: 'Youngkyun',
    lastName: 'Kim'
  },
  country: 'South Korea',
  gender: 'Male'
}

// 변수에서 기존의 할당
const firstName = person.name.firstName;
const lastName = person.name.lastName;
const gender = person.gender;
console.log(`${firstName} ${lastName} is a ${gender}`);

// 함수 인자에서 기존의 할당
const sayHello = (person) => {
  return `Hello, ${person.name.firstName} ${person.name.lastName} from ${person.country}!`
}
console.log(sayHello(person)); // "Hello, Youngkyun Kim from South Korea!"

// 변수에서 비구조화 할당
const { name: { firstName, lastName }, gender } = person;
console.log(`${firstName} ${lastName} is a ${gender}`);

// 함수 인자에서 비구조화 할당
const sayHelloDestructured = ({ name: { firstName, lastName }, country }) => {
  return `Hello, ${firstName} ${lastName} from ${country}!`
}
console.log(sayHelloDestructured(person)); // "Hello, Youngkyun Kim from South Korea!"
```

## 템플릿 리터럴

```javascript
const now = new Date().toString()

// 기존 방식
console.log("안녕하세요.\n지금 시각은 " + now + " 입니다.");

// 템플릿 리터럴 사용
console.log(`안녕하세요.\n지금 시각은 ${now} 입니다.`);
```