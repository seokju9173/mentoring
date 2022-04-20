# AMQP

>**AMQP**
Advanced Message Queing Protocol의 약자로, 흔히 알고 있는 MQ의 오픈소스에 기반한 표준 프로토콜을 의미한다.

대부분 AMQP를 기반으로 제작된 메시지 큐 이다.
그렇다면, AMQP는 어떤 방식으로 동작을 할지 알아야 해당 프로그램들이 어떻게 작동하는지 알기 쉬울것이다.

AMQP는 메시징 프로토콜의 한 종류로, 인스턴스끼리 서로 데이터를 교환할 때 사용하는 방법이다.

## 구성요소 및 라우팅 알고리즘

Exchange → Publisher(Producer)로부터 수신한 메시지를 큐에 분배하는 라우터 역할
Queue → 메시지를 메모리나 디스크에 저장했다가 Consumer에게 메시지를 전달하는 역할
Binding → Exchange와 Queue의 관계를 정의한다.

여기에서 Exchange는 수신한 메시지를 큐에 분배하는 라우터 역할을 한다고 했다.
하지만, 이 Exchange들은 어떻게 큐에 분배를 하게 될까? 사실 이게 AMQP에서 가장 중요한 역할이라 생각한다.

## Standard Exchange Type

Exchange는 큐에 분배하는 라우터 역할을 한다고 언급을 했는데 라우팅을 할 때 여러가지 규칙이 존재한다.

규칙의 종류는 다음과 같다.

### Direct Exchange

메세지의 라우팅 키를 큐에 1:1로 매칭하는 방법이다. 이는 Round Robin 방식으로 여러 Costomer간 Task를 분리한다.

### Topic Exchange

Topic Exchange는 일반적으로 메시지의 멀티캐스트 라우팅에 이용이된다. 
와일드 카드를 이용해 메시지를 큐에 매칭시키는 방식으로, 이 때 라우팅 키는 “,”으로 구분된  0개 이상의 단어의 집합으로 간주되고 와일드카드 문자를 이용해 특정 큐에 바인딩한다.

> **와일드카드**
" * " → 하나의 단어
" # " → 0개의 단어

### Fanout Exchange

바인딩된 모든 큐에 메시지를 라우팅하는데 이 때 라우팅 키는 무시가 된다.즉, Fanout Exchange는 브로드캐스트 라우팅을 하는데 사용이 된다.

### Header Exchange

key-value로 정이된 헤더에 의해 라우팅이 결전되는 것으로
큐를 바인딩 할 때 x-match라는 특별한 argument로 헤더를 어떤식으로 해석하고 바인딩할지 결정하게 된다.

> all : 모두 충족 시켜야 한다 (AND방식)
any : 하나만 충족시키면 된다 (OR방식)
> 

# 메시지 큐 종류

## RabbitMq

이는 AMQP의 대표적인 방식 중 하나이다.

신뢰성과 안정성이 있다는 장점을 가지고 있으며, 라우팅이 유연하다.
이는, Message Queue가 도착하기 전 라우팅이 되며 플러그인을 이용해 더 복잡한 라우팅이 가능하다.

개방형 프로토콜을 위한 AMQP구현을 위해 개발이 되었다.
Broker 중심적으로 설계가 되었으며, product/consumer간의 보장이되는 메시지 전달에 초점을 맞추고 있다.
또, 클러스터의 구성이 쉬우며 ManageUI가 제공이 되어 플러그인도 제공이 되어 확정성이 띄어나다는 장점과 brocker상에서 전달 상태를 확인하기 위한 메세지 표식을 사용한다.

마지막으로 데이터 처리보단 관리적 측면이나 다양한 기능 구현을 위한 서비스를 구축할 때 사용된다.

## ActiveMq

[https://velog.io/@gjrjr4545/AMQP#activemq](https://velog.io/@gjrjr4545/AMQP#activemq)

- 다양한 언어와 프로토콜 지원 (JAVA, C, C++, C#, Ruby, Peri, Python, PHP 클라이언트)
- OpenWire을 통해 고성능의 JAV, C, C++, C# 클라이언트 지원
- Stomp를 통해 C, Ruby, Peri, Python,PHP 클라이언트가 다른 인기 있는 메세지 브로커들과 마찬가지로 ActiveMQ에 접근 가능
- Message Groups, Virtual Destinations, Wildcards와 Composite Destination을 지원
- Spring 지원으로 ActiveMQ는 Spring Application에 매우 쉽게 임베딩될 수 있으며, Spring의 XML 설정 메커니즘에 의해 쉽게 설정 가능
- Geronimo, JBoss 4, GlassFish, WebLogic과 같은 인기있는 J2EE 서버들과 함께 테스트됨
- 고성능의 저널을 사용할 때에 JDBC를 사용하여 매우 빠른 Persistence를 지원
- REST API를 통해 웹기반 메시징 API를 지원

## Kafka

풀 네임으로는 Apache Kafka(아파치 카프카)는 LinkdIn에서 개발된 분산 메시징 시스템이다.
이는 대용량의 실시간 로그처리에 특화된 아키텍처 설계를 통해 기존 메시징 시스템보다 우수한 TPS를 보여준다.

AMQP나 JSM API를 사용하지 않고, 단순한 메세지 헤더를 지닌 `TCP 기반의 프로토콜을 사용해 오버헤드가 비교적 작다는 특징`을 가지고 있다.

publish-subscribe 모델을 기반으로 동작하여 크게 producer - broker - consumer로 구성이 되어있다.

pub-sub 모델은 메시지를 특정 수신자에게 직접적으로 보내주는 시스템은 아니고, publisher는 메세지를 topic을 통해 카테고리화 하게 된다. 이 때 분류된 메세지를 받기 위해 subscriber는 그 해당 topic을 subscribe함으로써 메세지를 읽어올 수 있다.

즉, publisher, subscriber 모두 topic에대한 정보만 알고 있다.
Kafka는 확장성(scale-out)과 고가용성(high availability)을 위하여 brocker들이 클러스터로 구성되어 동작하도록 설계가 되어있다. 심지어 broker가 1개밖에 없을 때에도 클러스터로 동작하며, 이 클러스터 내부의 분산처리는 ZooKeeper가 담당을 하게 된다.

### Topic / partition

메시지는 Topic으로 분류가 되며, topic은 여러개의 partition으로 나뉘게 된다.
partition내의 한 칸은 log라고 불리며, 하나의 topic은 여러 개의 partition을 나눠서 메시지를 쓰기때문에 병렬로 처리하기 위해 분산 저장을 한다.

이 때 한번 늘린 partition은 줄일 수 없으므로 충분히 고려하여 늘려야 하며, partition을 늘리면 메시지는 Round Robin 방식으로 쓰여진다.

### broker / zookeeper

broker는 kafka의 서버를 뜻하며, zookeeper는 분산 메세지 큐의 정보를 관리하는 역할을 한다.
여기서 zookeeper는 반드시 필요하다.

---

### 출처

[https://devahea.github.io/2019/04/30/AMQ-모델과-Exchange-Queue-Binding-에-대해/](https://devahea.github.io/2019/04/30/AMQ-%EB%AA%A8%EB%8D%B8%EA%B3%BC-Exchange-Queue-Binding-%EC%97%90-%EB%8C%80%ED%95%B4/)
[https://velog.io/@gjrjr4545/AMQP](https://velog.io/@gjrjr4545/AMQP)
[https://www.rabbitmq.com/tutorials/amqp-concepts.html](https://www.rabbitmq.com/tutorials/amqp-concepts.html)
