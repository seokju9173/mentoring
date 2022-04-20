REST Api라는 단어 많이 들어보았을것이다.

그런데, REST는 도대체 무엇이길래 REST Api 라는 단어가 생겨나게 되었을까?

# REST

REpresentational State Transfer의 약어로 REST 라고 한다.

자원을 이름으로 구분해 해당 자원의 상태를 주고받는것을 의미한다.

즉, 자원의 표현에 의한 상태전달을 목적으로 한다는것의 의미를 가지고 있다.

> 자원 : 해당 소프트웨어가 관리하는 모든 것 (문서, 그림, 데이터 등)
표현 : 그 자원을 표현하기 위한 이름 (DB의 학생 정보가 자원이면, ‘student’를 자원의 표현으로 정함)
상태 전달 : 데이터가 요청되는 시점에 자원의 상태 전달(JSON혹은 XML형태로 주고받는것이 일반적이다.)
> 

REST는 기본적으로 웹 기술과 HTTP프로토콜을 그대로 사용하기 때문에, 웹의 장점을 최대로 활용할 수 있는 아키텍처이다.

REST는 네트워크 상에서 Client와 Server간의 통신 방식 중 하나이다. ( 외에 SOAP라는것도 존재한다 )

>애플리케이션을 분리하고 이 분리된 앱을 통합하기 위함이며, 다양한 형태의 디바이스가 다양하게 생김과 동시에 다양한 플랫폼이 생겨 이러한 클라이언트들을 통합해주기 위해 필요하다.

## 구체적인 개념

HTTP URI를 통해 자원을 명시하고, HTTP Method인 GET,POST,PUT,DELETE등을 통해 해당 자원에 대한 CRUD Operation을 적용하는것을 의미한다.

즉, REST는 자원 기반의 구조 ROA(Resource Oriented Architecture)설계 중심에 Resource가 있고, HTTP Method를 통해 Resource를 처리하도록 설계된 아키텍처 이다.

>CRUD Operation
>
>Create → 생성(POST)
Read → 조회(GET)
Update → 수정(PUT , PATCH)
Delete → 삭제(DELETE)
HEAD → header정보 조회 (HEAD) 
> 

## 장점

위에 언급한 바와 같이, HTTP 프로토콜을 그대로 사용하므로 별도의 인프라 구축이 필요가 없다는 점이다.

위 말은 즉슨, HTTP 프로토콜을 따르는 모든 플랫폼과 통신이 가능하며 명확한 의도를 알 수 있다는 장점이 있다.

마지막으로 서버와 클라이언트가 해야 할 역할을 명확하게 분리를 할 수 있다.

## 단점

표준이 존재하지 않다라는 단점을 제외하고는 좋다.

# REST API

REST가 어떤놈인지 알게 되었으니, API까지 붙혀서 간단하게 알아보자.

REST API는 무엇일까? 말 그대로 REST의 특징을 기반으로 API를 구현한 것을 REST Api 라고 한다.

## 디자인 가이드 및 설계규칙

REST Api를 디자인 하는데 다양한 가이드(설계규칙)들이 존재한다.

필자는 이 중 제일 중요하다 생각하는 디자인 가이드 5가지만 추려보겠다.

추가적으로 더 궁금하면 [이 블로그](https://covenant.tistory.com/241)를 방문해보는것도 좋아보인다.

1. URL에 케밥케이스(kebab-case)를 사용
    1. `[GET] /user-name`
2. Path Variable은 카멜케이스 사용
    1. `[GET] /users/{userName}`
3. Collaction은 복수를 사용
    1. `[GET] /users`
4. Collection으로 시작해서 Identifier로
    1. `[GET] /users/{userName}`
5. 적절한 상태값을 사용
    1. 요청에 대한 응답값은 Body에 응답값만 보내주는것이 아닌 응답 상태 코드도 같이 보내준다.
    2. 응답 상태 코드는 [RFC 2616](https://datatracker.ietf.org/doc/html/rfc2616#section-10)에 정의가 되어있음

# RESTful

REST는 알겠는데, RESTful은 무엇일까? ful이 왜 붙는것일까?

RESTful은 REST의 설계 규칙을 잘 지켜 설계된 API를 RESTful한 Api라고 한다.

>REST의 설계규칙을 잘 따르는 시스템 = RESTful한 시스템
