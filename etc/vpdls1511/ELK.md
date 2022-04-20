ELK 란, 로그 수집 기록이다.

![](https://velog.velcdn.com/images/vpdls1511/post/e31d85a6-caf8-4067-ae72-5e5029a708e0/image.png)


실시간으로 서비스를 운영할 때 언제 어느 시점에서 문제가 발생하거나 이벤트가 발생 되었을 때 이러한 기록들을 수집하고 기록하여 크리티컬한 에러 발생 시 발빠르게 대처하기 위해 꼭 필요한 스택들이다.

E - Elastic Search (로그 저장 및 검색)
L - Logstash (로그 수집 엔진)
K - Kibana (로그 시각화 및 관리)

이 3개는 한 스택으로서 3가지가 한번에 같이 쓰이게 되는데, 이 3가지는 아래와 같이 사용이 된다

![](https://velog.velcdn.com/images/vpdls1511/post/9f8064ae-ad96-49fd-a76b-f983a3302acd/image.png)


## Logstash

- 다양한 소스( DB, csv파일 등 )의 로그 또는 트랜잭션 데이터를 수집, 집계, 파싱하여 Elasticsearch로 전달

## Elastic Search

- Logstash로부터 받은 데이터를 검색 및 집계를 하여 필요한 관심 있는 정보를 획득
- Lucene 기반의 검색엔진으로, HTTP 웹 인터페이스와 스키마에서 자유로운 JSON 문서와 함께 멀티테넌스 지원 전문 검색엔진을 제공한다.

## Kibana

- Elasticsearch의 빠른 검색을 통해 데이터를 시각화 및 모니터링

# 장점

ELK에 대해서 정말 간단하게만 작성했다. 하지만, ELK가 각각 어떠한 역할을 하는지 알아보았는데, 이 스택이 왜 필요하고 사용을 하는것일까?

### 가격

우선, 가장 좋은 점은 “무료" 라는 것이다. Elastic이라는 회사에서 제공하는 오픈소스이다보니 비용 측면에서 부담이 덜어진다.

### 접근성 & 사용성

ELK는 오픈소스를 내려받아 설치만하면 별 다른 설정없이 구축이 완료되어 접근성이 뛰어나다는 점과, 다른 시스템에 비해 상대적으로 사용이 쉬운편이다.

### 속도

ELK중 데이터 보관 및 분석 역할을 담당하는 Elastic Search는 거의 실시간에 가깝게 데이터를 처리할 수 있다.

### 유연성

ELK는 각자 다른 기능을 하고 있으며, 이러한 기능들이 모듈처럼 하나로 붙혀서 사용하기 때문에 Kibana를 쓰지 않고 ELK를 사용한다 해도,  Kibana와 같은 역할만 수행하면 다른걸로 대체가 가능하다.

---

### 참고

[https://aws.amazon.com/ko/opensearch-service/the-elk-stack/](https://aws.amazon.com/ko/opensearch-service/the-elk-stack/)
[https://velog.io/@holidenty/ELK-ELK-Stack-이란-무엇일까#span-stylecolor0b614b2---elk-stack의-장점span](https://velog.io/@holidenty/ELK-ELK-Stack-%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C#span-stylecolor0b614b2---elk-stack%EC%9D%98-%EC%9E%A5%EC%A0%90span)
