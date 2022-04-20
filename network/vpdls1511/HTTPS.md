# HTTPS가 필요한 이유

앞서 HTTP를 설명하며 HTTPS라는 개념을 잠시 언급을 했다. 그런데 이는 왜 필요할까?

우선, HTTP는 모든 데이터가 보내는 그대로 전송이 된다.

이게 무슨소리인가 한번 간단하게 살펴보자.

>필자가 일반 http 사이트에 접속을 해서 로그인을 하려고 한다.
id : test
pw : test1234!!
이렇게 작성을 하고 전송을 눌렀다면, 이 데이터는 어떻게 전달이 될까?
위 plain text 그대로 넘어가게 된다. 즉, 중간에 누가 가로챈다면 해당 사이트에 가입한 필자의 정보가 노출이 되는것이다.

위 예시를 보면 딱 느낌이 왔으리라 생각이 된다.

http를 그냥 사용을 한다면 암호화가 되지 않은 채 값이 Server에게 넘어가게 되며, 이 사이에 해커가 가로채기라도 한다면 큰일이다..

그래서 TLS를 적용한 HTTPS 가 등장을 하게 되었다.

“그럼 두 개가 뭐가 다른데?”

http는 test1234!! 라는 값이 그대로 노출이 되는데에 비해 https는 kdjfpqwjdkflj12!@$ 이런식으로 암호화가 되어 Server로 넘어가게 된다.

만약, 위 처럼 보안이 된다면 Server는 어떻게 알고 이를 해석을 할까?

바로, SSL이라는것을 이용하게 되는데, 이 SSL을 통해 비밀번호를 암호화하여 https포트인 443포트를 통해 네트워크를 타고 Server에 데이터가 도착을 하면 해당 데이터가 443 포트를 통해 왔다면, 기존에 등록된 SSL을 통해 암호를 해독하여 Server에서 해당 데이터를 가지고 Response를 보내게 된다. 여기서 해당 Response도 암호화가 된다.

# HTTPS HandShake

위에서 언급한 443 포트를 이용한 보안은 이 HTTPS HandShake( SSL HandShake라고도 불리며, TLS HandShake라고도 한다) 개념이 들어가게 된다.

이런 HandShake는 Client와 Server의 TCP의 3-Way HandShake에 연장이 된다.
이러한 이유는 HTTPS는 TCP 기반의 프로토콜이기 때문이다.

본론으로 돌아와 다시 개념을 설명하자면, 3-Way HandShake 이 후 아래의 순서대로 진행이 된다.

## 1. Client Hello

SSL HandShake의 첫 번재 단계로, Client가 Server와 SSL통신을 위한 최초 협상을 시도하는 단계이다.

이 때 Client가 사용이 가능한 Cipher Suite 목록과 Session ID, SSL Protocol Version 등이 Server측에게 전송이 된다.

>Cipher Suit : SSL Protocl version, 키 교환방식, 인증서 검증, 암호화 방식, 블록암호 운용방식, 메시지 인증의 정보들이 포함된 것이다.

## 2. Server Hello, Certificate, Server Hello Done

이를 한번에 묶은 이유는, 이 3가지의 패킷이 Server → Client로 한번에 넘어가기 때문이다.

### Server Hello

Client가 보낸 Client Hello 패킷을 받아 Cipher Suite중 하나를 선택한 후 Client에게 이를 알리며, 함께 자신의 SSL Protocol Version도 함게 알려준다.

### Certificate

Server가 가지고 있는 인증서(SSL)를 Client에게 전달하는 부분이다. 이 SSL안에는 공개키가 존재한다.

이 때 받은 인증서를 복호화 하여 Client는 해다 인증서가 공식 기관에서 서명한 CA인지 확인을 한다.

### Server Hello Done

이 단계에서 Server Key Exchange 라는 작업도 진행을 하게 되는데, 이는 Certificate에서 보낸 SSL에 공개키가 내부에 존재하지 않을 경우, Server가 직접 전달함을 의미한다. 하지만, 공개키가 SSL 내부에 있을 경우, 해당 과정은 거치지 않는다.

그리고 이 모든 과정이 끝나게 된다면 Server Hello Done  작업이 진행되어 인증서 전달에 관련된 행동을 모두 마쳤음을 Client에게 전달을 한다.

## 3. ClientKeyExchange

이는, 대칭키를 Client가 생성하여 SSL 인증서 내부에서 추출한 Server의 공개키를 이용해 암호화 한 후 Server에게 다시 전송을 하는것이다. 여기서 나오는 대칭키의 개념이 SSL HandShake의 목적 이며, 가장 중요한 수단인 데이터를 실제 암호화할 대칭키 생성 이다.

이 대칭키를 만들어 Server에 전달하지 않는다면 Server는 암호화된 데이터를 받아도 받은데이터를 사용할 수 없다.

## 4. ChangeCipherSpec / Finished

이는 Client와 Server양측 모두 패킷을 통하여 TLS/SSL 즉, HTTPS 통신에 필요한 데이터를 주고받았고, 통신을 할 준비가 되었음을 알리는 패킷이다.

그리고, 통신을 종료할 때 Finished 패킷을 통하여 종료한다.

---

참고

[https://aws-hyoh.tistory.com/entry/HTTPS-통신과정-쉽게-이해하기-3SSL-Handshake](https://aws-hyoh.tistory.com/entry/HTTPS-%ED%86%B5%EC%8B%A0%EA%B3%BC%EC%A0%95-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-3SSL-Handshake)

궁금증 : Client는 SSL을 어떻게 알고 사용을 할까?
