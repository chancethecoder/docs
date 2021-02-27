---
description: '목표: 모듈 번들러가 무엇인지, 왜 쓰는지, 어떤 것들이 있는지 살펴봅니다.'
---

# 모듈 번들러 개요

모듈 번들러는 다양한 리소스를 하나의 병합된 결과물로 만들어주는 도구입니다. 최적화된 결과물을 제공함으로써 서비스를 효율화 하는 것이 모듈 번들러의 도입 목적입니다.

현재 가장 많이 쓰이는 모듈 번들러는 [webpack](https://webpack.js.org/)이며, 이 외에도 몇 가지 도구들을 아래에서 알아보겠습니다.

## 모듈이 뭔가요?

프로그래밍에서 모듈은 기능 단위의 프로그램을 말합니다. 프로그램을 하나의 거대한 덩어리가 아니라 기능 단위의 모듈로 쪼개어 구현하면, 프로젝트의 규모가 커지더라도 복잡도를 크게 낮출 수 있습니다.

자바스크립트 모듈 예시:

```javascript
// 계산기 모듈 작성 예시 (IIFE 방식)
const Calculator = (function () {
    function circleArea(r) {
        return r * r * PI;
    }

    function substract(a, b) {
        return a - b;
    }

    function sum(a, b) {
        return a + b;
    }

    const PI = 3.141592;

    return {
        circleArea,
        substract,
        sum,
        PI,
    }
})()

// 계산기 모듈을 사용할 땐 이렇게
console.log(Calculator.sum(5, 10));
console.log(Calculator.substract(100, Calculator.circleArea(4)));
```

## 모듈 번들러가 왜 필요한가요?

자바스크립트에서 모듈 단위로 파일을 만들고, 이를 로딩했다고 생각해봅시다.

```markup
<html>
  <head>
    <script src="/src/module1.js"></script>
    <script src="/src/module2.js"></script>
    <script src="/src/module3.js"></script>
    <script src="/src/module4.js"></script>
    <script src="/src/main.js"></script>
  </head>
</html>
```

여기서 문제점은 아래와 같습니다.

1. **파일을 나눠서 전송받는 것**은 하나의 파일로 합쳐서 전송받는 것에 비해 네트워크 측면에서 **비효율적**입니다.
2. **함수 스코프**인 자바스크립트에서는 이런 방식에서 **변수 충돌**이 일어날 수 있습니다.
3. 로딩 순서에 따라 **모듈 간의 의존성**을 보장하기 어렵습니다.

이런 문제점을 해결하기 위해 도입된 것이 모듈 번들러입니다.

모듈 번들러는 모듈을 모아서 하나의 병합된 번들 파일로 생성하는 역할을 합니다. 그리고 최종적으로 사용자에게는 최적화된 번들 파일을 전달하게 됩니다. 이로써 개발은 모듈 단위로, 제공은 단일 파일로 하는 것이 가능해집니다.

```markup
<html>
  <head>
    <script src="/dist/bundle.js"></script>
  </head>
</html>
```

모듈 번들러는 파일 병합 뿐만 아니라 다양한 플러그인을 통해 기능을 확장하기도 합니다. 모듈 번들러가 할 수 있는 일들은 크게 아래와 같습니다.

* 모듈 의존성 계산
* 번들링
* 압축
* 난독화
* [미사용 코드 제거 \(Tree Shaking\)](https://ui.toast.com/weekly-pick/ko_20180716)
* [코드 스플릿](https://ko.reactjs.org/docs/code-splitting.html)

## webpack

[webpack](https://webpack.js.org/)은 현재 가장 많이 쓰이는 모듈 번들러입니다. 사용법이 다소 어렵고 기능이 복잡하다는 인식이 있지만, 다양한 기능을 지원하고 있으며 꾸준히 사용되고 있기 때문에 다양한 레퍼런스를 가지고 있다는 장점이 있습니다.

주요 개념으로는 엔트리, 아웃풋, 로더가 있습니다.

* 엔트리: 의존성 그래프의 시작점
* 아웃풋: 번들링 결과물이 저장될 위치
* 로더: 자바스크립트 뿐만 아니라 CSS, 이미지, 폰트 등의 리소스를 모듈 단위로 처리하기 위한 것
* 플러그인: 번들링된 최종 결과물에 특정 기능을 수행하기 위한 것

아래는 webpack 설정\(`webpack.config.js`\)의 예시입니다. \(세부 설정: [https://webpack.js.org/configuration/](https://webpack.js.org/configuration/)\)

```javascript
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  }
};
```

이제 실제로 아래와 같은 프로젝트가 있다고 가정합시다.

```javascript
// src/index.js
import Calculator from './calculator.js';

console.log(Calculator.sum(5, 10));
console.log(Calculator.substract(100, Calculator.circleArea(4)));
```

```javascript
// src/calculator.js
function circleArea(r) {
  return r * r * PI;
}

function substract(a, b) {
  return a - b;
}

function sum(a, b) {
  return a + b;
}

const PI = 3.141592;

export default {
  circleArea,
  substract,
  sum,
  PI,
};
```

위 프로젝트에서 webpack을 통해 빌드를 수행하면 아래와 같은 결과물이 생성될겁니다. \(실제와는 다를 수 있으나, 이해를 돕기 위한 내용입니다.\)

```bash
npx webpack --config webpack.config.js # webpack 번들링 수행하기
```

```javascript
// dist/bundle.js
function a(r) { return r * r * d; }
function b(a, b) { return a - b; }
function c(a, b) { return a + b; }
const d = 3.141592;
console.log(c(5, 10));
console.log(b(100, a(4)));
```

## parcel

[parcel](https://parceljs.org/)은 설정이 필요 없는 모듈 번들러입니다. webpack의 복잡한 설정을 따라가면서 사용해야할 이유가 없다면 사용을 고려해볼만 합니다.

## rollup

[rollup](https://rollupjs.org/)은 Tree Shaking이라고 하는 성능 최적화 부분에서 이점이 있는 모듈 번들러입니다. 다만 사용층이 Webpack 진영이 더욱 많고 개발도 활발하기 때문에 여러 측면에서 Webpack의 장점을 따라가지는 못하고 있습니다.

