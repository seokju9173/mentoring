NginX는 쉽게 말하면, Apache의 업그레이드 버전이라고 할 수 있다.
왜 업그레이드 버전이라고 할 수 있는것일까?
이를 알기 위해서는 Apache의 구조에 대해서 조금 알아두는게 좋다.

# Apache

Apache 는 MPM 방식으로 HTTP 요청을 처리한다.
이게 무슨소리인가 싶을것이다.

MPM → Multi Porcess Module 방식의 약어로,

PreFork MPM, Worker MPM 이렇게 총 2개의 방식이 있다.

## PreFork MPM (다중 프로세스)

이 방식은 멀티 프로세스를 이용하는 방식이다.
즉, 여러개의 프로세스를 이용한다는 뜻인데 이는 1개의 Connection은 1개의 Process가 담당을 하게 된다.

이 방법은 각각의 프로세스가 서로 메모리를 공유하지 않아 프로세스는 서로 독립적인 상태이다. 스레딩을 지원하지 않는 라이브러리와 연결된 어플을 실행하는데 안전하다는 장점을 보유하고 있지만,

반대로 같은 기능을 하더라도 완전히 독립적으로 실행이 되고 있어 메모리 공유가 일어나지 않아 각 프로세스마다 메모리를 할당해주어야 하므로 메모리 낭비가 꽤 심하다는 단점을 보유하고 있다.

## Worker MPM(멀티 프로세스 → 스레드)

해당 방식은 멀티 프로세스를 사용하지만, 덩달아 멀티 스레드까지 사용하는 하이브리드 방식이다.

부모가 여러 Connection을 받고, 해당 Connection들을 스레드들이 처리해주는 방식으로, 한 개의 프로세스가 여러개의 Connection을 동시에 제어할 수 있다. 이로 인해 Connection의 안정성을 유지하면서도 자원 활용에 유리하다는 장점을 보유하고있는 방식이다.

---

Apache의 MPM방식인 PreFork와 Worker는 각각의 특징을 가지고 있다.

하지만, 공통점인게 있는데 모두 멀티쓰레딩 보다는 멀티 프로세스를 사용한다는 단점이 있다.

C10K 라는 단어를 들어본 적 있는가?
1만개의 클라이언트 문제라는 뜻을 가지고 있는 단어로, Apache의 경우 한 시스템에 접속자수가 1만명이 된다면 CPU와 메모리 사용량이 급증하고,  Process및 Thread의 생성 비용이 드는 등 대용량 처리에서 한계를 보인다.

또한, 프로세스가 Blocking될 때 처리하지 못하고 처리가 완료될 때 까지 대기상태에 있는다. 이는 HTTP1.0의 옵션중 하나인  Keep-Alive로 해결이 가능하지만, 효율이 매우 떨어지게된다.

# NginX

NginX는 Apache의 한계점인 C10K의 문제를 해결하기 위해 나온 웹서버SW이다.
어떻게 해결을 했을까? 라는 궁금증이 먼저 들게 되는데 이는 NginX의 특징인 Event-Driven으로 해결이 되었다.

## Event Driven

<aside>
💡 기술이 발점함에 따라 트래픽이 많아지며, C10K의 문제를 해결하기 위해 나온 방법으로 클라이언트의 요청을 병렬로 처리하는 방식이다.

</aside>

이게 무슨소리인지 정확히 이해가 가지 않는다.

Nginx는 설정파일을 읽고, 해당 설정에 맞게 CPU의 코어 개수만큼 Worker Process를 만들어내는 Master Process가 있다.
여기서 만들어진 Worker Process는 Connection이 생성되면 해당 Connection을 처리하고 Keep-Alive 시간동안 Connection을 유지한다.

이 때 다른 Connection이 들어오면 어떻게 될까? 기존에 연결되어 Keep-Alive의 요청으로 연결된 Connection이 사용되지 않고있고, 사라지지 않았다면 새롭게 연결된 Connection은 기존 Connection을 사용하게 된다. 만약, 기존 Connection 이 사라지거나 사용중이라면 새로운 Connection을 생성하게 된다.

여기서 Connection이 들어오고, 생성되고, 삭제되는 모든 과정을 Event라고 하며, 이 Event들은 OS 커널이 Queue 형식으로 Worker Process 에게 전달을 해 주는데, 이 때 Event들은 Queue에서 처리가될때까지 비동기 방식으로 흘러가게 된다.

하지만, Queue에는 여러개의 Event가 대기를 하고 있는데 이제 작업을 시작해야할 Event의 작동 시간이 너무 오래 걸리게 된다. 그렇다면 어떻게 될까? 바로 “스레드 풀”로 오래걸리는 Event를 전달하고, 스레드 풀에서 작업을 진행하게 된다.

마지막으로, 위에서 Worker Process는 CPU의 코어 수 만큼 만들어진다고 했다. 듀얼코어면 2개, 쿼드면 4개 이런식으로 만들어 진다. 이렇게 되면 코어당 담당하는 프로세스를 바꾸는 횟수 즉, Context Switching이 대폭 줄어들게 된다.

이는 Apache와 비교를 하였을 때 Apache는 너무나 많은 Process를 보유하고 있어 리소스 낭비가 심한 방면, NginX는 리소스의 소모도 낮고, 적은 Context Switching으로 인해 성능이 많이 좋아진다.

이러한 방식이 Event-Driven Model 이라고 할 수 있다.

## 단점

하지만, 이게 무조껀 좋은것 만은 아니다.

개발자가 기능 추가를 시도했다가 잘 돌아가고있던 Worker Process를 종료했다. 이러면 정말 큰일이다.
Worker Process가 담당하던 Event들이 모조리 사라지게 되어버리기 때문에 문제가 발생하게 된다.

그렇기 때문에 개발자가 직접 모듈을 제작하는 과정에 있어 상당히 까다롭다는 단점이 존재한다.

## 장점

장점은 무엇일까?

많은 양의 동시 커넥션을 연결할 수 있으며, 많은 커넥션이 연결 되어도 process의 개수가 많지 않고 Context Switching이 적게 일어나므로 성능이 매우 좋다

추가로, 약간의 세팅을 추가한다면 로드밸런서 역할을 할 수 있다는 장점도 있다.

# 정리

그렇다면 Apache보다 NginX를 사용하는게 좋을까?

이 부분에 대해서는 선택과 집중의 차이라고 생각한다. 이 둘은 각각의 장단점을 보유하고 있고, 이러한 장단점을 고려하여 선택을 해야한다고 생각한다.

---

### 출처

[https://mytory.net/2021/05/06/apache-mpm.html](https://mytory.net/2021/05/06/apache-mpm.html)

[https://velog.io/@ksso730/Nginx-Apache-비교](https://velog.io/@ksso730/Nginx-Apache-%EB%B9%84%EA%B5%90)
