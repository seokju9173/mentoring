이 두가지를 비교하기 전, 먼저 Multi Module이 무엇인지 알아야 한다.
하지만, 그 전에 Module이라고 자주 언급하며 사용하지만, 이 것이 무엇을 뜻하는지 100% 정의내리고 사용하는사람은 극히 드물다고 생각이 든다.

# Module

>모듈이란,
여러 가지로 정의될 수 있지만, 일반적으로 큰 체계의 구성요소이고, 다른 구성요소와 독립적으로 운영되는 것이다.
즉, 독립적으로 운영될 수 있는 의미를 가진 구성요소라고 할 수 있다.


# Multi Module Project

그렇다면, 모듈이 여러개인 프로젝트를 멀티 모듈 프로젝트라고 할까?
이는 조금 다른 개념의 접근이라고 할 수 있을것같다.

![](https://velog.velcdn.com/images/vpdls1511/post/ad523b94-e43e-466c-8bfb-50b2b33b5da6/image.png)


이 그림을 아무것도 모르는 상태에서 보게 된다면 이해가 안될 수밖에 없다. 필자도 이러한 그림을 보고 알아보는데 정말 오랜 기간이 걸렸다.

한 서비스에 Batch와 API가 존재하는데, 이 두개의 서버는 Member.class **내부에 동일한 로직의 공통적인 코드가 존재**한다.
이를 각각 적용시켜주기 위해서는 아래의 루틴이 돌아갈 것이다.

>Memer.class코드 수정사항 발생 → Batch 프로젝트 오픈 → Batch 프로젝트의 Member.class 수정 → Batch 프로젝트 닫고 API 프로젝트 오픈 → API 프로젝트 Member.class 수정


이를 보면 벌써부터 답답하다.

그런데 우리가 조금만 생각을 하면 아래와같은 질문이 생길 수 있다.

“차라리 이 두 프로젝트에서 공통된 코드를 담을 수 있는 공간(모듈)을 만들어서 두 프로젝트에서 가져다가 쓰면 되지 않을까?”

그렇다. 이러한 문제를 해결해주는게 바로 Multi Module이다.

## 필요한 이유

위에 언급했듯 Multi Module은 **공통된 코드를 묶기 위해**서 사용한다고 했다.

현 예제에서는 2개의 프로젝트만 예시를 들었는데, 만약 5개 6개의 프로젝트라고 한다면 매우 귀찮은 일이다.
개발자는 귀찮은것을 싫어하기 때문에 여러 프로젝트에 공통된 코드를 하나로 묶는 방법을 생각해 낸것이 바로, Multi Module이라는 개념이다.

그런데 과연 위와같이 단순히 귀찮아서일까?

아니다, **접근성**에도 문제가 있다.

위에 설명한 루틴에서 프로젝트를 오픈하고 닫고 오픈하고 닫고 의 루틴을 5~6개의 프로젝트에 적용하거나, 5~6개의 IDE를 열어두고 작업을 해야한다.

이렇게 되면 개발하는데 굉장히 번거로워지며 어려움을 겪게된다.

## 구성

위에는 정말 간단한 예시이다. Batch와 API를 Common이라는 프로젝트에 모아두는것이다.

그렇다면, 실제 프로젝트에는 어떤식으로 적용이 될까?

필요한 이유에 언급했듯 여러개의 IDE를 오픈할 필요 없이 하나의 IDE만 오픈하면 된다.

즉, 한 프로젝트에 모듈이 여러개 들어있다는 것이다.

![](https://velog.velcdn.com/images/vpdls1511/post/d46453cf-f8c3-437e-99d8-3b32f114d337/image.png)


실제 프로젝트에는 이와같은 형식으로 들어가있게 된다.
자, 그런데 여기서 주의해야할 점이 하나가 있다.

>Batch와 API 모듈은 모두 Common 즉, member-core에 공통된 코드가 존재하기 때문에 Common Module을 의존한다는 코드를 작성해주어야 한다.

결국 빌드를 하게되면 어떻게 될까?

## 빌드

![](https://velog.velcdn.com/images/vpdls1511/post/a3cf0cc8-4bbd-4ac5-8ad2-d39de1545dd9/image.png)


빌드를 하게 되면, 이와같이 빌드가 된다.

Api.jar 안에 Common Module이 포함이 되고,
Batch.jar 안에도 Common Module이 포함이 된다.

---

# Multi Module VS MSA

이 글을 작성하는 이유이다.

Multi Module과 MSA는 공통점을 가지고 있다.

MSA - API Gateway를 통한 기능별 API 서버 분할
Multi Module - Common Module에 있는 공통된 코드를 각종 Module에서 사용

즉, 이 두가지는 한 곳을 의존하게 된다는 공통점이다.

하지만, 이 두가지는 다른 개념이라고 할 수 있다.

Monolitic이 여러개가 모여 MSA가 될 수 있고,
Multi Module이 모여 MSA가 될 수 있기 때문에 이 둘은 다른 구조이고, 더 크게 보자면

Monolitic이 MSA로 전환할 때 중간에 Multi Module이 조금 더 수월하게 전환할 수 있게 해주는 구조를 가질 수 있다.

### 참고

[https://velog.io/@soyeon207/스프링-부트-멀티-모듈-프로젝트-만들기](https://velog.io/@soyeon207/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B6%80%ED%8A%B8-%EB%A9%80%ED%8B%B0-%EB%AA%A8%EB%93%88-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0)
[https://techblog.woowahan.com/2637/](https://techblog.woowahan.com/2637/)
