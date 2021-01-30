# HTML 개요

[HTML(Hyper Text Markup Language)](https://en.wikipedia.org/wiki/HTML)은 웹 페이지 문서를 표현하기 위한 마크업 언어입니다. 과거에는 정보 전달 기능이 주요 목적이었으며 말 그대로 구조적인 문서를 표현하기 위해 개발되었지만, 웹의 비약적인 발전으로 인해 사용자들이 폭발적으로 늘어 현재는 대부분의 웹 페이지를 구성하는 대중적인 언어가 되었습니다.

현대의 웹 페이지는 HTML만으로 구성되어 있지는 않습니다. 크게 두 가지를 추가해야 하는데, [CSS(Cascading Style Sheet)](https://en.wikipedia.org/wiki/CSS)와 [Javascript](https://en.wikipedia.org/wiki/JavaScript)입니다. 이들 각각이 웹 페이지에서 하는 역할을 보면 아래와 같습니다.

- **HTML**: 정보 전달
- **CSS**: 디자인 표현
- **Javascript**: 상호 작용

이 세 가지는 현재 웹 페이지를 구성하는 기본 요소로 여겨지며, 웹 페이지를 직접 작성하는 개발자가 아니더라도 기본적인 상식으로 알고 있으면 많은 도움이 될 수 있습니다.

## 웹 페이지가 우리 눈으로 보이기까지

HTML을 살펴보기 전에 HTML의 개념을 좀 더 이해하기 위해 웹 페이지가 우리에게 전달되는 과정을 생각해봅시다. 과정은 크게 아래와 같이 나눌 수 있습니다.

1. 문서 작성 = 정적 파일 (HTML, CSS, Javascript)
2. 문서 저장 = 로컬 PC, 서버
3. 문서 검색 = 문서 위치 입력
4. 문서 표현 = 브라우저

[아이패드 그림 here]

여기서 주목해야할 것은 문서 표현 단계입니다. 문서 표현 단계에서 브라우저는 최종적으로 전달된 정적 파일(HTML, CSS, Javascript)을 **해석**하여 시각적으로 그려주는 역할을 합니다. 이 때 문제가 발생하는데, 작성된 정적 파일은 동일한데 브라우저 별로 해석하는 방식과 성능이 다르기 때문에 사용자가 어떤 브라우저를 통해 웹 페이지를 보는지가 웹 개발자에게는 중요한 문제가 됐습니다.

이는 브라우저의 시장 점유율에 따라 웹 기술이 영향을 받는 결과를 초래하게 되었고, 한 때 웹 개발자들을 괴롭히는 이슈였습니다. 하지만 이는 웹 표준화 및 다양한 해결 방법을 통해 대부분 해소되었고, 결과적으로 현재 시점으로 전 세계에서 가장 많이 사용되는 브라우저인 [크롬 브라우저](https://en.wikipedia.org/wiki/Google_Chrome)를 기준점으로 생각하면 됩니다.

## HTML 요소

[HTML 요소(Elements)](https://en.wikipedia.org/wiki/HTML_element)는 HTML 문서를 구성하는 기본 단위입니다. 다른 용어로 태그(Tag)라고 불리기도 합니다. 여는 기호(<)와 닫는 기호(>)로 표현되며, 아래와 같은 태그들이 존재합니다. ([w3schools.com](https://www.w3schools.com/tags/) 참고)

```
<!-->, <!DOCTYPE>, <a>, <abbr>, <acronym>, <address>, <applet>, <area>, <article>, <aside>, <audio>, <b>, <base>, <basefont>, <bdi>, <bdo>, <big>, <blockquote>, <body>, <br>, <button>, <canvas>, <caption>, <center>, <cite>, <code>, <col>, <colgroup>, <data>, <datalist>, <dd>, <del>, <details>, <dfn>, <dialog>, <dir>, <div>, <dl>, <dt>, <em>, <embed>, <fieldset>, <figcaption>, <figure>, <font>, <footer>, <form>, <frame>, <frameset>, <h1>, <h2>, <h3>, <h4>, <h5>, <h6>, <head>, <header>, <hr>, <html>, <i>, <iframe>, <img>, <input>, <ins>, <kbd>, <label>, <legend>, <li>, <link>, <main>, <map>, <mark>, <meta>, <meter>, <nav>, <noframes>, <noscript>, <object>, <ol>, <optgroup>, <option>, <output>, <p>, <param>, <picture>, <pre>, <progress>, <q>, <rp>, <rt>, <ruby>, <s>, <samp>, <script>, <section>, <select>, <small>, <source>, <span>, <strike>, <strong>, <style>, <sub>, <summary>, <sup>, <svg>, <table>, <tbody>, <td>, <template>, <textarea>, <tfoot>, <th>, <thead>, <time>, <title>, <tr>, <track>, <tt>, <u>, <ul>, <var>, <video>, <wbr>
```

태그를 사용해서 실제 HTML 문서를 작성할 땐 아래와 같습니다.

예시: (https://jsfiddle.net/ 에서 코드를 실행해볼 수 있습니다.)

```html
<!DOCTYPE html>
<html>
<head>
    <title>HTML 요소를 사용해봅시다.</title>
</head>
<body>
    <div>
        <h1>안녕하세요? 1번 제목입니다.</h1>
        <h2>안녕하세요? 2번 제목입니다.</h2>
        <h3>안녕하세요? 3번 제목입니다.</h3>
        <p>
            p 태그를 사용해서 이렇게 단락을 표현합니다.<br>
            단락 내에서 줄을 바꿀 때는 br 태그를 사용합니다.<br>
        </p>
        <p>
            링크는 a 태그를 사용합니다. <br>
            <a href="https://naver.com" target="blank">네이버 열기</a>
        </p>
        <p>
            목록은 ul(unordered list), ol(ordered list) 태그, 목록 내의 각 항목은 li(list item) 태그를 사용합니다.
        </p>
        순서 없는 목록:
        <ul>
            <li>목록 첫 번째</li>
            <li>목록 두 번째</li>
            <li>목록 세 번째</li>
        </ul>
        순서 있는 목록:
        <ol>
            <li>목록 첫 번째</li>
            <li>목록 두 번째</li>
            <li>목록 세 번째</li>
        </ol>
        <p>
            테이블은 table, thead, tbody, th, tr, td 태그를 사용합니다.
        </p>
        <table>
            <thead>
                <th></th>
                <th>이름</th>
                <th>성별</th>
                <th>기혼유무</th>
            </thead>
            <tbody>
                <tr>
                    <td>
                        <img src="https://search.pstatic.net/common?type=a&size=120x150&quality=95&direct=true&src=http%3A%2F%2Fsstatic.naver.net%2Fpeople%2Fportrait%2F201804%2F20180415131721771.jpg">
                    </td>
                    <th>김범수</th>
                    <td>남</td>
                    <td>N</td>
                </tr>
                <tr>
                    <td>
                        <img src="https://search.pstatic.net/common?type=a&size=120x150&quality=95&direct=true&src=http%3A%2F%2Fsstatic.naver.net%2Fpeople%2Fportrait%2F201803%2F20180329135907725.jpg">
                    </td>
                    <th>나얼</th>
                    <td>남</td>
                    <td>N</td>
                </tr>
                <tr>
                    <td>
                        <img src="https://search.pstatic.net/common?type=a&size=120x150&quality=95&direct=true&src=http%3A%2F%2Fsstatic.naver.net%2Fpeople%2Fportrait%2F201409%2F20140922203416508-3731873.jpg">
                    </td>
                    <th>박효신</th>
                    <td>남</td>
                    <td>N</td>
                </tr>
                <tr>
                    <td>
                        <img src="https://search.pstatic.net/common?type=a&size=120x150&quality=95&direct=true&src=http%3A%2F%2Fsstatic.naver.net%2Fpeople%2Fportrait%2F201901%2F20190102123032425.jpg">
                    </td>
                    <th>이수</th>
                    <td>남</td>
                    <td>Y</td>
                </tr>
            </tbody>
        </table>
    </div>
</body>
</html>
```

## HTML 속성

[HTML 속성(attributes)](https://en.wikipedia.org/wiki/HTML_attribute)은 HTML 요소를 제어하기 위해 태그 안에 추가적으로 제공되는 정보입니다. 이름-값 형태로 정의되며 주요 속성은 아래와 같습니다.

- id
- class
- style
- href

## 간단하게 웹페이지 만들어보기

