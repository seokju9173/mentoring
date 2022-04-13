REST API를 구축하여 작업을 하다보면 한번씩 오류로 CORS오류를 본 적 있었다.

이는 Cross-Origin Resource Sharing 의 약어인데 개발을 처음 시작할 때 이 문제로 엄청난 고뇌에 빠져 삽질을 많이 했던 적 있었다.

교차 출처 리소스 공유(Cross-Origin Resource Sharing) 즉, CORS는 정책이다.

필자가 위에서 CORS오류를 만난적 있다고 했는데, 이는 CORS 정책을 어겼기 때문에 그런것이다.

## 정책

보안을 위해 탄생한 이 CORS 에러는 서버에서 허용하지 않은 IP가 서버에 접근을 하려고 할 때 차단을 시켜버리는 것이다.

Client가 HTML의 하이퍼링크를 통해 특정 사이트에 접속하는건 허용이 되지만, 이 사이트 내부에서 이용하는 API를 호출하면 즉, 허가되지 않은 클라이언트가 리소스를 요청하게 된다면 “CORS 정책을 어긋났어!” 라고 요청을 거부하게 되버린다.

## 리소스 요청

리소스 요청의 기준은 어떻게 될까?

필자가 맨 처음 작성한 단어, 바로 REST API이다. 이 REST API는 HTTP를 기반으로 동작한다고 했는데, 이 HTTP Method로 서버에 요청을 하면 리소스를 요청하게 된다. 이처럼  HTTP Method를 이용한 요청을 할 때 아래의 조건이 허용되지 않는다면 정책위반 에러를 발생하게 된다.

- 다른 *도메인*(예: `example.com`에서 `amazondomains.com`으로)
- 다른 *하위 도메인*(예: `example.com`에서 `petstore.example.com`으로)
- 다른 *포트*(예: `example.com`에서 `example.com:10777`으로)
- 다른 *프로토콜*(예: `https://example.com`에서 `http://example.com`으로)

# 해결 방법

그렇다면 해결 방법이 없는것일까?

해결방법은 충분히 존재한다.

## Access-Controll-Allow-Origin

가장 대표적인 방법은 서버에서 Access-Controll-Allow-Origin 헤더에 알맞은 값을 세팅해주는것이다.

```jsx
router.get('/data',(req,res) => {
	// 모든 요청 허가
	res.header('Access-Controll-Allow-Origin','*')

	// naver.com의 요청 허가
	res.header('Access-Controll-Allow-Origin','https://naver.com')
})
```

위 처럼 `*`을 사용해 모든 요청을 허가하거나, 일부 요청만을 허가할 수 있다.

## Proxy

Proxy도 하나의 해결 방법이다.

<aside>
💡 Proxy
프록시는 클라이언트 혹은 서버에게 접속하는 IP를 속이는 방법이다.
클라이언트의 IP를 우회하여 서버를 속이는 방법은 Forward Proxy,
서버의 IP를 우회하여 클라이인트를 속이는 방법은 Reverse Proxy

</aside>

대표적으로 React에서 proxy를 사용하는 방법이다.

```json
//package.json
{
	.....생략....
	"proxy":"http://localhost:8080",
	.....생략....
}
```

> React의 Package.json에서 사용하는 proxy는 말 그대로 proxy일 뿐이며, 이를 누가 읽어서 설정 옵션으로 사용하는지에 따라 리버스프록시와 포워드 프록시가 나뉜다.
> 

---

# SOP

웹 관련된 정책은 CORS말고도 SOP(Same-Origin Policy)가 존재한다.

2011년 RFC6454에 등장한 보안 정책으로, 이름에 나와있듯 “같은 출처의 리소스만 공유 가능" 이라는 규칙을 가진 정책이다.

그런데 요즘 웹은 엄청나게 오픈이 되어있다. 그렇다보니 Open API같이 다른 출처를 가진 API를 호출할 수 있다보니 전부 막을 수 없는 노릇이다.

그렇다보니 몇몇 예외사항을 정해두었다. 그 중 하나는 “CORS 정책을 지킨 다른 출처 리소스 요청" 이었다.

CORS의 경우, 2009년에 나와 SOP보다 빠르게 나온 개념이다.

# 출처의 기준

CORS, SOP 모두 출처를 기반으로 정책이 정해진다.
그렇다면, 출처는 어떻게 나뉘는것일까?

출처의 기준은 `스킴,호스트,포트` 이 3가지가 모두 동일해야 한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/721a8027-f276-4b5b-8906-ff457a980abc/Untitled.png)

[출처](https://evan-moon.github.io/2020/05/21/about-cors/)

위 이미지를 보면 알겠지만, Protocol(Schem), Host, Port 이 3가지가 동일해야 한다.
