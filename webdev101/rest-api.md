# REST API 개요

> 목표: REST API의 개념을 간단한 사용 예시와 함께 알아봅니다.

[REST(Representational State Transfer)](https://en.wikipedia.org/wiki/Representational_state_transfer) API는 웹 개발자라면 필수적으로 알아야 하는 개념이자 설계 원칙, 방법론입니다. HTTP 통신 프로토콜 위에서 적용되며 HTTP 통신이 이뤄지는 모든 플랫폼에서 사용 가능합니다.

## 기본 개념

아래와 같은 상황에 처해있다고 생각해보겠습니다.

- `mysite.com` 이라는 서비스와 API 연동을 개발해야 합니다.
- 통신 방식은 HTTP 통신을 사용해야 합니다.
- 아래의 API 주소들을 예시로 받았습니다.

```
mysite.com/get_users
mysite.com/get_products
mysite.com/delete_products?userId=1
mysite.com/update_product?userId=1&productId=10
```

URI를 보고 개발자의 의도를 어느 정도 알 수 있지만, 조금 모호하게 느껴질 수 있습니다. 이번에는 REST 스타일의 API로 받았다고 가정해봅시다.

```
[GET] mysite.com/users
[GET] mysite.com/products
[DELETE] mysite.com/users/1/products
[PUT] mysite.com/users/1/product/10
```

겉으로 보기에는 처음과 별 차이가 없어 보일 수 있지만, REST의 관점으로 보면 기능이 명확한 API가 되었습니다.

기존 API와 REST API의 차이점을 보면 아래와 같습니다.

- GET, PUT, DELETE 등의 HTTP Method를 통해 하고자 하는 행동을 표현 (생성,조회,수정,삭제 = CRUD)
- URI에는 행동의 대상(자원)을 표현, 되도록 동사보다 명사를 사용

## 제한 조건

REST API는 아래의 조건을 준수해야 한다고 합니다.

- 인터페이스 일관성 : 일관적인 인터페이스로 분리되어야 한다
- 무상태(Stateless): 각 요청 간 클라이언트의 콘텍스트가 서버에 저장되어서는 안 된다
- 캐시 처리 가능(Cacheable): WWW에서와 같이 클라이언트는 응답을 캐싱할 수 있어야 한다.
  - 잘 관리되는 캐싱은 클라이언트-서버 간 상호작용을 부분적으로 또는 완전하게 제거하여 scalability와 성능을 향상시킨다.
- 계층화(Layered System): 클라이언트는 보통 대상 서버에 직접 연결되었는지, 또는 중간 서버를 통해 연결되었는지를 알 수 없다. 중간 서버는 로드 밸런싱 기능이나 공유 캐시 기능을 제공함으로써 시스템 규모 확장성을 향상시키는 데 유용하다.
- Code on demand (optional) - 자바 애플릿이나 자바스크립트의 제공을 통해 서버가 클라이언트가 실행시킬 수 있는 로직을 전송하여 기능을 확장시킬 수 있다.
- 클라이언트/서버 구조 : 아키텍처를 단순화시키고 작은 단위로 분리(decouple)함으로써 클라이언트-서버의 각 파트가 독립적으로 개선될 수 있도록 해준다..

## Method

REST API의 동작은 HTTP의 Method를 그대로 사용합니다. HTTP 통신 프로토콜의 Method는 아래와 같습니다.

- Create : 생성(POST)
- Read : 조회(GET)
- Update : 수정(PUT)
- Delete : 삭제(DELETE)
- HEAD: header 정보 조회(HEAD)

## Resource
