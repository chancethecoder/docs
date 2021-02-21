# REST API 개요

> 목표: REST API의 개념을 간단한 사용 예시와 함께 알아봅니다.

[REST\(REpresentational State Transfer\)](https://en.wikipedia.org/wiki/Representational_state_transfer)는 좀 더 이해하기 쉽고 표준화된 API 작성에 도움을 주는 개념이자 설계 원칙입니다.

## 기본 개념

아래와 API를 만들었다고 가정하고 살펴봅시다.

```bash
# 모든 유저 목록 조회
/get_users

# 1번 아이디의 유저 정보 조회
/get_users?userId=1

# 모든 상품 목록 조회
/get_products

# 1번 아이디의 유저가 가진 모든 상품 삭제
/delete_products?userId=1

# 1번 아이디의 유저가 가진 10번 상품 업데이트
/update_product?userId=1&productId=10
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

## Method

REST API의 동작은 Method로 표현됩니다. 여기서 Method는 일반적으로 HTTP 통신 프로토콜의 Method를 말하며, 아래와 같은 정의를 가지고 있습니다.

* POST: 생성
* GET: 조회
* PUT: 수정
* PATCH: 부분 수정
* DELETE: 삭제

## Resource

Resource는 REST에서 정보의 핵심 추상화 개념입니다. 문서, 이미지, 비가상 객체\(예: 사람\) 등 이름을 지정할 수 있는 모든 정보는 자원이 될 수 있습니다. REST는 자원 식별자를 사용하여 구성요소 간의 상호 작용에 관련된 특정 자원을 식별합니다.

## REST 원칙

> 참고: [https://restfulapi.net/](https://restfulapi.net/)

* 클라이언트-서버: 유저 인터페이스와 데이터 저장을 분리하여 서버 컴포넌트를 단순화 시킴으로써 플랫폼 이식성과 확장성을 증가시킵니다.
* 무상태성: 요청에 필요한 컨텍스트는 모두 클라이언트에 저장되어 매 요청마다 서버에 전달되어야 하며, 서버에 컨텍스트 정보를 저장해서는 안됩니다.
* 캐시 가능: 요청에대한 응답은 캐시 가능 여부를 포함해야 하며 클라이언트는 응답을 캐싱할 수 있어야 합니다.
* 인터페이스 일관성: 시스템 내의 리소스는 URI를 통해 표현되며 일관된 접근 방법을 제공해야 합니다.
* 계층화된 시스템: 시스템을 다중 계층화 구조로 구성할 수 있어야 합니다.

