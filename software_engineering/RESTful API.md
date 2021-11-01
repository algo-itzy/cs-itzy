[TOC]



## RESTful API

> https://www.youtube.com/watch?v=RP_f5dMoHFc



### 1. 서론

#### 1. REST

- `REpresentational State Transfer`
- a way of providing **interoperability** between computer systems on the Internet. (상호 운용성)



#### 2. REST 출현 계기

시작은 웹이다. 1991년에 www가 팀 버너스 리에 의해 탄생했다. 그런데 하나의 고민이 있었다.

> **어떻게 인터넷에서 정보를 공유할 것인가?**

이에 대한 해답으로 "웹"이 출범하게 된다. 이에 팀 버너스 리의 답은 아래와 같다

##### 정보들을 하이퍼 텍스트로 연결

- ##### **표현 형식 : HTML**

- **식별자 : URI**

- **전송방법 : HTTP**

그래서 이제 HTTP라는 프로토콜을 여러 사람들이 설계를 하게 되었다. 그 중에 1명, 대학원생이었던 로이 필딩(Roy T. Fielding)이라는 사람이 이 프로토콜 작업에 참여하게 된다.

그 와중에 고민이 생긴다. 이미 94년도에 로이는 http 1.0 작업에 참여했다. 이 명세가 나오기 전에 이미 http는 당연히 www의 전송 프로토콜로서 이용이 되고 있었다. 그리고 또한 웹은 이미 급속도로 성장하는 도중이었다.

이 시점에서 로이은 http를 정립하고 이 명세에 기능을 더하고 기존의 기능을 고쳐야하는 상황에 놓이게 된다. 그러나 무작정 http 프로토콜을 고치게 된다면, 기존 구축된 웹하고 호환이 안되는 가능성이 존재 했다. 이에 로이는 고민을 한다.

> "How do I improve HTTp without breaking the Web?"

##### "웹을 망가뜨리지 않고 어떻게 http 기능을 증가시킬 수 있을까?"



### 2. 본론

#### 1. REST API

REST API → **REST 아키텍처를 따르는 API**

REST → 분산 하이퍼미디어 시스템(ex. 웹)을 위한 **아키텍처 스타일**

아키텍처 스타일 → **제약 조건**의 집합

즉, REST에서 정의한 제약 조건을 모두 지켜야 REST를 따른다고 말할 수 있다는 것

사실 REST는 아키텍처 스타일이면서 하이브리드 아키텍처 스타일. 아키텍처 스타일이면서, 동시에 아키텍처 스타일의 집합이기 때문



#### 2. REST 구성 스타일

1. **Client-Server**
2. **Stateless**
3. **Cache**
4. **Uniform Interface**
5. **Layered System**
6. **Code-on-Demand (optional)** : 서버에서 코드를 클라이언트로 보내서 실행할 수 있어야 한다 → 자바스크립트

HTTP만 잘 따라도 Client-Server, Stateless, Cache, Layered System은 다 지킬 수 있기 때문

Uniform Interface 역시 아키텍처 스타일



#### 3. Uniform Interface 제약 조건

- **Identification of resources** : **URI로 리소스가 식별**되면 된다
- **Manipulation of resources through representations** : **representation 전송을 통해서 리소스를 조작**

→ 리소스를 만들거나 삭제, 수정할 때 http 메시지에 그 표현을 전송

- **Self-descriptive messages**
- **Hypermedia as the engine of application state(HATEOAS)**

하지만 문제는 아래 2개이다. 이 2가지는 사실 우리가 REST API라고 부르는 거의 모든 것들은 지키지 못하고 있다.



#### 4. Self-Descriptive Messages

##### 메시지는 스스로를 설명해야한다.



```json
GET / HTTP/1.1
```

단순히 루트를 얻어오는 GET 요청이다. 이 HTTP 요청 메시지는 목적지가 빠져 있어서 Self-descriptive 하지 못하다.

```json
GET / HTTp/1.1
Host: www.example.org
```

이 요청이 [www.example.org](http://www.example.org/) 라는 도메인으로 간다라는 **목적지**가 빠져있기 때문에 이런 경우는 Self-descriptive하다



또 이런 것도 생각해볼 수 있다. 200 응답 메시지이며, JSON 본문이 있다.

```json
HTTP/1.1 200 OK
[ { "op": "remove", "path": "/a/b/c" } ]
```

이렇게 되면 당연히 Self-descriptive 하지 않는데, 어떤 문법으로 작성된 것인지 모르기 때문. 그렇기 때문에 **Content-Type 헤더가 반드시 들어가야한다.**

```json
HTTP/1.1 200 OK
Content-Type: application/json

[ { "op": "remove", "path": "/a/b/c" } ]
```

Content-Type 헤더에서 대괄호, 중괄호, 큰따옴표의 의미가 뭔지 알게 되어, 파싱이 가능하여 문법을 해석할 수 있게 된다.

그렇다면 이제 Self-descriptive 하지 않고, 그걸 해석했다고 하더라도, op 값은 무슨 뜨시고, path가 무엇을 의미하는지는 알 수 없다.

```json
HTTP/1.1 200 OK
Content-Type: application/json-patch+json

[ { "op": "remove", "path": "/a/b/c" } ]
```

이렇게 명시를 하면 완전해진다. 이 응답은 json-patch+json이라는 미디어 타입으로 정의된 메시지이기 때문에 json-patch라는 명세를 찾아가서 이해한 다음, 이 메시지를 해석을 하면 그제서야 올바르게 메시지의 의미를 이해할 수 있게 된다.



**이처럼 Self-descriptive message라는 것은 메시지를 봤을 때 메시지의 내용으로 온전히 해석이 다 가능해야된다는 것이다.**

하지만 오늘날의 REST API는 상당히 이를 만족하지 못하고 있다. 대부분 미디어 타입에는 그냥 json이라고만 되어있고 이를 어떻게 해석해야되는지는 명시가 되어있지 않다.



#### 5. HATEOAS

##### 애플리케이션의 상태는 Hyperlink를 이용해 전이되어야 한다.

![img](https://media.vlpt.us/images/kjh03160/post/edbf8e1f-7e61-411c-a1aa-744afb19638f/image.png)

루트 홈페이지 → 글 목록 보기 GET → 글 쓰기 GET → 글 저장 POST → 생성된 글 보기 GET → 목록 보기 GET → ...

이렇게 상태를 전이하는 것을 애플리케이션 상태 전이라고 하고, 이 **상태 전이마다 항상 해당 페이지에 있던 링크를 따라가면서 전이했기 때문에 HATEOAS**라고 할 수 있다. 하이퍼 링크를 통한 전이가 되는 것이다.

```html
HTTP/1.1 200 OK
Content-Type: text/html

<html>
<head> </head>
<body> <a href="/test"> test </a> </body>
</html>
```

a 태그를 통해서 하이퍼링크가 나와 있고, 이 하이퍼 링크를 통해서 그 다음 상태로 전이가 가능 → HATEOAS를 만족

```json
// json 표현
HTTP/1.1 200 OK
Content-Type: application/json
Link: </articles/1>; rel="previous",
			</articles/3>; rel="next";

{
	"title": "The second article",
	"contents": "blah blah..."
}
```

**Link 헤더 : 리소스와 하이퍼링크로 연결되어 있는 다른 리소스를 가르킬 수 있는 기능 제공**

어떤 1개의 게시물을 표현 : 이전의 게시물 URI가 /articles/1, 다음 게시물은 /articles/3에 있다는 정보를 표현

또한, Link 헤더가 이미 표준으로 문서가 나와 있기 때문에 이 메시지를 보낸 사람이 온전히 해석해서 어떻게 링크가 되어 있는가를 이해하고 하이퍼링크를 타고 다른 상태로 전이 가능



#### 6. 왜 Uniform Interface?

##### 1. 독립적 진화

- 서버와 클라이언트가 각각 독립적으로 진화한다.
- **서버의 기능이 변경되어도 클라이언트를 업데이트할 필요가 없다.**
- REST를 만든 계기 : "How do I imporve HTTP without breaking the Web"

독립적으로 진화한다면 어떻게 될까? 서버의 기능이 바뀌었다. 새로운 API가 추가되고 기존 API가 변경/삭제되고 URI가 바뀌고 변동이 있다. 하지만 클라이언트가 바뀌지 않아도 된다.

**"웹을 망가뜨리지 않고 어떻게 수정할 것인가?"**

그렇기 때문에 REST가 목적하는 바가 독립적인 진화이다. 이를 달성하기 위해서는 **Unifrom Interface가 필수**적이기기에, 이를 만족하지 못하면 REST라고 부를 수 없다.

##### 2. 실제로 잘 지켜지고 있는가?

REST를 아주 잘 지키고 있는 사례는 바로 웹이다.

- 웹 페이지를 변경했다고 웹 브라우저를 업데이트할 필요는 없다.
- 웹 브라우저를 업데이트 했다고 웹 페이지를 변경할 필요도 없다.
- HTTP 명세가 변경되어도 웹은 잘 작동한다.
- HTML 명세가 변경되어도 웹은 잘 작동한다.

우리가 들어가는 웹 페이지가 변경되었다고 해서 그 웹 페이지를 접근할 때 웹 브라우저를 업데이트 할 필요 없다. 

반대로 크롬을 업데이트 했다고 해서, 웹 페이지에 안들어가지는 것도 아니다.

심지어 HTTP 명세가 바뀌어도 그렇다. 2014년 HTTP 2.0이 나오고 여러가지 웹 페이지들이 HTTP 2.0을 적용한다. 하지만 적용을 하지 않은 사이트들도 여전히 잘 된다.

HTML 5.0이 HTML 5.2로 바뀌어도 똑같이 잘 된다.

##### 3. 상호운용성(interoperability)에 대한 집착

- **Referer → 오타지만 안 고침**
- **charset → 잘못 지은 이름이지만 안 고침**
- **HTTP 상태 코드 418(I'm a teapot) 포기**
- **HTTP/0.9 아직도 지원(크롬, 파이어폭스)**

25년전 Referer 오타를 냈다. 하지만 이를 고치지 않았다. 이를 고치는 순간 웹이 깨지기 때문이다. 또 charset이란 이름은 잘못 지었다. 원래 encoding이라고 이름을 지어야하는 데 그때 encoding 개념이 부족하여, character set이랑 같은 줄 알고 지었다. 하지만 그대로 고치지 않고 갔다. 이름을 고치면 상호 운용성이 깨지기 때문에.

또한, http 상태 코드가 하나씩 추가되다가 418번을 추가할 때가 되었는데, 문제가 생겼다. 만우절 때 만들었던 이상한 spec HTCPCP 상태 코드가 있는데, 이것은 http가 아니라서 무시를 해도 됐었다. 하지만 수많은 node.js와 같은 서버들의 http 서버 구현체들이 http 상태 코드로 구현을 해버렸다. 그래서 처음에는 http 위장이 프로젝트마다 돌아다니면서 418 지원을 제거해야된다는 이슈를 올렸는데 맹비난을 받고 포기했다. 그래서 418은 http 상태코드에서 영구 결번으로 만들게 되었다. 이미 그런 구현체가 존재하고, 잘못 구현체들과 상호 운용성을 지켜주기 위해(안그러면 웹이 깨지기때문에) 이러한 결정을 한것이다.

HTTP 버전이 올라가면서 예전 버전을 빼려고 시도했지만 몇몇 프록시에서 오작동 하는 것이 발견되어서 HTTP/0.9를 유지하게 되었다.

그래서 이렇게 많은 사람들이 상호 운용성에 대한 노력 덕분에 독립적으로 진화될 수 있었다.



#### 7) REST가 웹의 독립적 진화에 도움을 주었나

그렇다. HTTP에 지속적으로 영향을 주었다.

- Host 헤더 추가
- 길이 제한 다루는 방법 명시 (414 URI Too Long 등)
- URI에서 리소스의 정의가 추상적으로 변경: "문서의 위치" → "식별하고자 하는 무언가"
- HTTP/1.1 명세 최신판에서 REST에 대한 언급이 들어감 → representaion의 개념도 있음 → 로이 필더 : HTTP와 URI 명세 저자 중 1명

##### 1. 그럼 REST는 성공 했는가?

- REST는 웹의 독립적 진화를 위해 만들어졌다.
- 웹은 독립적으로 진화하고 있다. → 성공!

##### 2. 그런데 REST API는?

- REST API는 REST 아키텍처 스타일을 따라야 한다.
- 하지만 오늘날 스스로 REST API라고 하는 API들의 대부분이 REST 아키텍처 스타일을 따르지 않는다.

##### 3. REST API도 제약 조건들을 다 지켜야 하는건가?

> "An API that provides network-based access to resources via **uniform interface of self-descriptive messages containing hypertext to indicate potential state transition** might be part of an overall system that is a RESTful application. - Roy T. Fielding

REST API라고 하는 것은 하이퍼 텍스트를 포함한 self-descriptive 메시지의 uniform interface를 통해 리소스에 접근하는 API라고 강조했다. **즉, 모든 제약 조건을 지켜야한다고 했다.**



#### 8) 원격 API가 꼭 REST API여야 하는건가?

##### 아니여도 된다.

> "REST emphasizes evolvability sustain on uncontrollable system. **If you think you have control over the system or aren't interested in evolvability, don't waste your time arguing about REST**. - Roy T. Fielding

시스템 전체를 통제할 수 있다고 생각하거나, 진화에 관심이 없다면, REST에 대해 따지느라 시간을 허비하지 마라.

**시스템 전체를 통제할 수 있다** → 회사에서 서버 개발자인데, 클라이언트 개발자를 통제 가능할 때 or 클라이언트, 서버를 다 만들 수 있을 때

**진화에 관심이 없다** → 매일 업데이트 해서 유저들에게 불만을 들어도 난 상관이 없다.

하지만 우리는 시스템 전체를 통제할 수 없기도 하고, 개발자로서 진화에 관심을 두어야 하기에 REST를 따라야 한다.



#### 9) 그럼 이제 어떻게 해야되는가?

1. REST API를 구현하고 REST API라고 부른다.
2. REST API 구현을 포기하고 HTTP API라고 부른다.
3. **REST API가 아니지만 REST API라고 부른다. (현재 상태)**

> "I am getting frustrated by the number of people calling any HTTP-base interface a REST API... Please try to adhere to them or choose some other buzzword for your API." - Roy T. Fielding

제약 조건을 따르던지 아니면 다른 조건을 쓰자



#### 10) 진짜 REST API를 구현하자

##### 1. 왜 API는 REST가 잘 안되나?

|              | 흔한 웹 페이지 |  HTTP API   |
| :----------: | :------------: | :---------: |
|   Protocol   |      HTTP      |    HTTP     |
| 커뮤니케이션 |  사람 - 기계   | 기계 - 기계 |
|  Media Type  |      HTML      |  JSON, XML  |

HTTP API는 사람이 아닌 기계가 해석한다. 미디어 타입 역시 기계가 의미를 이해할 수 있는 포맷 사용

|                  |      HTML      |  JSON  |
| :--------------: | :------------: | :----: |
|    Hyperlink     | 됨 (a 태그 등) | 정의 X |
| Self-descriptive | 됨 (HTML 명세) | 불완전 |

**self-descriptive** 측면에서 보면 **JSON은 불완**전하다. 불완전 하다는 것은 적어도 문법은 정의되어 있다. 어떻게 파싱하고 array를 어떻게 해석해라 까지는 되어있다. 안에 들어갈 수 있는 key-value에 대한 의미는 아무도 정의되지 않는다.

**즉, 문법은 해석 가능하지만 의미를 해석하려면 별도로 문서(API 문서 등)가 필요하다.**

```html
<!-- html -->
GET /todos HTTP/1.1
Host: example.org

HTTP/1.1 200 OK
Content-Type: text/html

<html>
<head> </head>
<body> 
<a href="https://todos/1"> 회사 가기 </a> 
<a href="https://todos/2"> 집에 가기 </a> 
</body>
</html>
```

- HTML → Self-descriptive
  1. 응답 메시지의 Content-Type을 보고 미디어 타입이 text/html 확인
  2. HTTP 명세에 미디어 타입은 IANA에 등록되어 있다고 함 → IANA에서 text/html 명세 찾음
  3. IANA에 따르면 text/html 명세는 http://www.w3.org/TR/html 이므로 링크를 찾아가 명세 해석
  4. 명세에 모든 태그의 해석 방법이 있으므로 이를 해석, 사용자에게 보여줄 수 있음
  5. Success
- HTML → HATEOAS
  1. a 태그를 이용해 표현된 링크를 통해 다음 상태로 전이될 수 있으므로 만족
  2. Success

```json
// json
GET /todos HTTP/1.1
Host: example.org

HTTP/1.1 200 OK
Content-Type: application/json

[
	{"id": 1, "title": "회사 가기"},
	{"id": 2, "title": "집에 가기"}
]
```

- JSON → self-descriptive
  1. 응답 메시지의 Content-Type을 보고 미디어 타입이 application/json 확인
  2. HTTP 명세에 미디어 타입은 IANA에 등록되어 있다고 함 → IANA에서 application/json 명세 찾음
  3. IANA에 따르면 application/json 명세는 draft-ietf-jsonvis-rfc7159bis-04 이므로 링크를 찾아가 명세 해석
  4. 명세에 JSON 문서 파싱 방법이 명시 → 성공적으로 파싱, 그러나 "id", "title"이 무엇을 의미하는지 알 방법 없음.
  5. FAIL
- JSON → HATEOAS
  1. 다음 상태로 전이할 링크가 없다.
  2. FAIL

##### 2. 그런데 self-descriptive, HATEOAS가 어떻게 독립적인 진화에 도움이 되는가?

###### self-descriptive → 확장 가능한 커뮤니케이션

서버나 클라이언트가 변경되더라도 오고가는 메시지는 언제나 self-descriptive 하므로 언제나 해석이 가능하다.

###### HATEOAS → 애플리케이션 상태 전이의 late binding

어디서 어디로 전이가 가능한지 미리 결정 X. 어떤 상태로 전이가 완료 되어야, 그 다음 전이될 수 있는 상태 결정 ⇒ **링크는 동적으로 변경될 수 있다.**

##### 3. 그럼 REST API로 바꿔보자

1. Self-descriptive

   1. Media type 정의
      1. 미디어 타입을 하나 정의
      2. 미디어 타입 문서 작성 → "id", "title"의 의미 정의
      3. IANA에 미디어 타입을 등록. 이때 만든 문서를 미디어 타입의 명세로 등록
      4. 이제 이 메시지를 보는 사람은 명세를 찾아갈 수 있음 → 메세시지 의미 온전히 해석 가능
      5. Success

   ```json
   GET /todos HTTP/1.1
   Host: example.org
   
   HTTP/1.1 200 OK
   Content-Type: application/vnd.todos+json
   
   [
   	{"id": 1, "title": "회사 가기"},
   	{"id": 2, "title": "집에 가기"}
   ]
   ```

   ⇒ 단점 : 매번 media type 정의

   1. Profile 이용
   2. "id", "title" 의미 정의한 명세 작성
   3. Link 헤더에 profile relation으로 명세 링크
   4. 이제 메시지를 보는 사람은 명세를 찾아갈 수 있음 → 의미 해석 가능
   5. Success

   ```json
   GET /todos HTTP/1.1
   Host: example.org
   
   HTTP/1.1 200 OK
   Content-Type: application/json
   Link: <https://exmaple.org/docs/todos>; rel="profile"
   
   [
   	{"id": 1, "title": "회사 가기"},
   	{"id": 2, "title": "집에 가기"}
   ]
   ```

   ⇒ 단점

   - 클라이언트가 Link 헤더(RFC5988)와 profile(RFC 6906)을 이해해야함.
   - **Content negotiation 불가**

2. HATEOAS

   1. data에 다양한 방법으로 하이퍼링크 표현

   ```json
   GET /todos HTTP/1.1
   Host: example.org
   
   HTTP/1.1 200 OK
   Content-Type: application/json
   Link: <https://exmaple.org/docs/todos>; rel="profile"
   
   [
   	{
   		"link": "https://exmplae.org/todos/1, 
   		"title": "회사 가기"
   	},
   	{
   		"link": "https://exmplae.org/todos/2, 
   		"title": "회사 가기"
   	},
   ]
   ```

   ```json
   GET /todos HTTP/1.1
   Host: example.org
   
   HTTP/1.1 200 OK
   Content-Type: application/json
   Link: <https://exmaple.org/docs/todos>; rel="profile"
   
   {
   	"links" : {
   		"todo" : "https://example.org/todos/{id}"
   	},
   	"data": [{
   		"id": 1, 
   		"title": "회사 가기"
   	}, {
   		"id": 2,
   		"title": "집에 가기"
   	}]
   }
   ```

   ⇒ 단점 : 링크를 표현하는 방법을 직접 정의

   1. JSON으로 하이퍼링크를 표현하는 방법을 정의한 명세들을 활용

   ```json
   GET /todos HTTP/1.1
   Host: example.org
   
   HTTP/1.1 200 OK
   Content-Type: application/vnd.api+json
   Link: <https://exmaple.org/docs/todos>; rel="profile"
   
   {
   	"data": [{
   		"type": "todo",
   		"id": 1, 
   		"attributes": {"title": "회사 가기"},
   	"links" : { "self": "https://example.org/todos/1"
   	},{
   		"type": "todo",
   		"id": 2, 
   		"attributes": {"title": "집에 가기"},
   	"links" : { "self": "https://example.org/todos/2"
   	}]
   }
   ```

   ⇒ 단점 : 기존 API를 많이 고쳐야한다. (침투적)

   1. HTTP 헤더로 → Link, Location 등

   ```json
   POST /todos HTTP/1.1
   Content-Type: application/json
   
   {
   		"title": "점심 약속"
   }
   
   HTTP/1.1 204 No Content
   Location: /todos/1
   Link: </todos/>, rel="collection"
   ```

⇒ data, 헤더 모두 사용하면 좋다.

##### 4. Media type 등록은 필수인가?

> "A REST API should be entered with no prior knowledge beyond the initial URI (bookmark) and set of standardized media types that are appropriate for **the intended audience**(i.e., expected to be understood by any client that might user the API). — Roy. T. Fielding

의도한 저자가 이해할 수만 있다면 상관 없다. 예를 들면 사내에서만 쓰는 API고 이를 이해하고 있다면 굳이 등록하지 않아도 된다.

**등록의 장점**

- 사전에 알고 있지 않은 사람도 누구나 쉽게 사용할 수 있게 됨
- 이름 충돌을 피할 수 있음
- 등록이 별로 어렵지 않다



### 3. 결론

- 오늘날 대부분 "REST API"는 사실 REST를 따르고 있지 않다.
- REST의 제약 조건 중 특히 **self-descriptive와 HATEOAS**를 잘 만족하지 못한다.
- REST **긴 시간에 걸쳐(수십년) 진화**하는 웹 애플리케이션을 위한 것
- REST를 따를 것인지는 API 설계하는 이들이 스스로 판단하여 결정
- REST를 따르겠다면 Self-decriptive와 HATEOAS를 만족시켜야한다.
  - Self-decriptive는 **custom media type, profile link relation**으로 만족 가능
  - HATEOAS는 **HTTP 헤더**나 본문에 **링크**를 담아 만족 가능