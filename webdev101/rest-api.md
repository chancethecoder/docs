---
description: '목표: REST API가 무엇인지 알아봅니다.'
---

# REST API

[REST\(REpresentational State Transfer\)](https://en.wikipedia.org/wiki/Representational_state_transfer)는 [월드와이드웹\(WWW\)](https://ko.wikipedia.org/wiki/%EC%9B%94%EB%93%9C_%EC%99%80%EC%9D%B4%EB%93%9C_%EC%9B%B9)이 발전한 이후로 현재까지 많이 알려진 개념입니다. 이미 잘 정리된 글이나 영상이 많고 조금만 찾아봐도 학술적인 의미를 공부하는 것에는 어려움이 없을 것입니다.

따라서, 본 문서에서는 REST라는 개념 자체를 파고들기보다 이를 추상적으로 이해하고 실제로 REST API가 왜 쓰이는지, 어떻게 쓰이는지, 어떤 논점이 있는지 등을 살펴보겠습니다.

## REST 살펴보기

REST에 대해서 알아보기 위해 REST의 **목표**와 **원칙**을 알아보겠습니다.

{% tabs %}
{% tab title="목표" %}
* 구성 요소 상호작용의 규모 **확장성** \(scalability\)
* 인터페이스의 **범용성** \(generality\)
* 구성 요소의 **독립적인 배포** \(independency\)
* **중간적 구성요소**를 이용해 응답 지연 감소, 보안을 강화, 레거시 시스템을 인캡슐레이션 \(intermediary components\)
{% endtab %}

{% tab title="6가지 원칙" %}
* [클라이언트-서버](https://en.wikipedia.org/wiki/Client%E2%80%93server_model) : 유저 인터페이스와 데이터 저장을 분리하여 컴포넌트 단순화 및 확장성을 증가시킨다
* [무상태성](https://en.wikipedia.org/wiki/Stateless_protocol) : 서버가 클라이언트의 컨텍스트를 따로 저장하지 않는다
* [캐시 가능](https://ko.wikipedia.org/wiki/%EC%9B%B9_%EC%BA%90%EC%8B%9C) : 클라이언트는 응답을 캐싱할 수 있어야 한다
* 인터페이스 일관성 : 시스템 내의 리소스는 URI를 통해 표현되며 일관된 접근 방법을 제공해야한다
* [계층화된 시스템](https://en.wikipedia.org/wiki/Layered_system) : 클라이언트와 서버 사이에 여러 시스템을 통해 계층화 구조 구성이 가능하다
* [Code on Demand \(optional\)](https://en.wikipedia.org/wiki/Dynamic_web_page#Client-side_scripting) : 서버에서 클라이언트에 로직을 전송하여 기능을 확장할 수 있다
{% endtab %}
{% endtabs %}

REST가 설계된 본질은 월드와이드웹의 독립적인 진화에 목표가 있었습니다.

목표의 키워드\(확장성, 범용성, 독립성, 중간 구성요소\)들을 살펴보면, 이는 웹 서비스에서 자연스럽게 지켜지고 있고, 지켜져야 할 요소들로 생각되는 것들입니다. 개발자가 시스템에 변경을 가하더라도 웹 서비스를 사용하는 사용자가 영향을 느끼지 못하도록, 그것이 마치 아무렇지 않은 일인 것처럼 보이도록 만들자는 것이 핵심입니다.

이런 목표를 도달하기 위해 필요한 것이 6가지 원칙입니다. REST에서는 이 6가지 원칙 중에서 단 한개라도 준수하지 않는다면, 해당 시스템은 RESTful하지 않다고 이야기합니다.

원칙의 몇 가지 항목은 일반적인 웹 서비스에서 자연스럽게 준수되고있는 사항입니다. 우리는 **브라우저**를 통해 웹 페이지에 접속하는 **클라이언트-서버** 구조를 사용하고 있습니다. 또한, 클라이언트로부터 서버에 직접 통신할 수도 있지만, 그 사이에 또 다른 컴포넌트를 두어 로드밸런싱, 프록시 등의 역할을 수행하도록 **계층화 구조**를 만들기도 합니다. 웹 서버, 혹은 브라우저에서는 응답 값을 **캐싱**할 수 있도록 다양한 방법을 제공하기도 합니다.

하지만, 대부분의 경우 **인터페이스 일관성**을 충족하지 못하여 \(엄격한 기준으로\) RESTful하지 못한 경우도 있습니다. 일관성에 대한 부분은 REST 설계 시 중요하게 알아야 하는 내용이므로, 아래에서 다시 다루도록 하겠습니다.

REST에 대해서 살펴본 내용을 정리해보겠습니다.

* REST는 월드와이드웹과 같은 분산 [하이퍼미디어](https://ko.wikipedia.org/wiki/%ED%95%98%EC%9D%B4%ED%8D%BC%EB%AF%B8%EB%94%94%EC%96%B4) 시스템을 위한 아키텍처 스타일입니다.
* REST 아키텍처는 독립적이고 지속적인 진화가 필요한 시스템에 도움을 줍니다.
* RESTful 하려면 [6가지 원칙](https://restfulapi.net/rest-architectural-constraints/)을 준수해야 합니다.

## REST API 구성

### 자원 \(Resource\)

REST에서 자원은 정보를 추상화하기위한 개념입니다. 문서, 이미지, 비가상 객체\(예: 사람\) 등 이름을 지정할 수 있는 모든 정보는 자원이 될 수 있습니다. REST는 자원 식별자를 사용하여 구성요소 간의 상호 작용에 관련된 특정 자원을 식별합니다.

### 행위 \(Verb\)

REST API의 동작은 Method로 표현됩니다. 여기서 Method는 일반적으로 HTTP 통신 프로토콜의 Method를 말하며, 아래와 같은 정의를 가지고 있습니다.

* POST: 생성
* GET: 조회
* PUT: 수정
* PATCH: 부분 수정
* DELETE: 삭제

### 표현 \(Representation\)

ㅁ

## REST와 Non-REST 비교

아래와같이 HTTP API를 만들었다고 가정해봅시다.

```bash
[GET] /get_users
[GET] /get_users?userId=1
[GET] /get_products
[DELETE] /delete_products?userId=1
[PUT] /update_product?userId=1&productId=10
```

API 주소를 보고 개발자의 의도를 어느 정도 알 수 있지만, 조금 모호하게 느껴질 수도 있습니다. 문제점을 꼽아보자면 아래와 같습니다.

1. 동사\(조회, 삭제, 업데이트\)가 주소에 포함되었다.
2. user와 product의 포함관계 표현이 어렵다.
3. 간결하지 않고 복잡하다.

REST는 이러한 문제점을 해소할 수 있도록 Method와 Resource 개념을 적용시킵니다. 아래를 살펴보겠습니다.

```text
[GET] /users
[GET] /users/1
[GET] /products
[DELETE] /users/1/products
[PUT] /users/1/product/10
```

얼핏 보기에 이전과 별 차이가 없어 보일 수 있지만, REST의 관점으로 보면 기능이 명확한 API가 되었습니다. 기존 API와 REST API의 차이점을 보면 아래와 같습니다. \(그리고 이 차이점이 핵심 개념입니다.\)

* Method로 행위를 표현한다. \(조회, 생성, 업데이트, 삭제\)
* URI는 행위의 대상\(자원\)을 표현하며 되도록 동사보다 명사를 사용한다.

## 논점

현재 REST API라고 부르는 API들이 정말 REST API인가? \(진정한 RESTful이란?\)

* 로이 필딩의 포스트 : [https://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven](https://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven)
* 그런 REST API로 괜찮은가 : [https://www.youtube.com/watch?v=RP\_f5dMoHFc](https://www.youtube.com/watch?v=RP_f5dMoHFc)

과연 REST가 시스템을 더욱 쉽게 만드는가? \(REST 비관론자의 입장\)







