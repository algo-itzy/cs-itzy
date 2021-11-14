# RESTful API

## 1. REST API 기본 규칙

### 1.1 RESTful API의 URL은 자원을 표시해야 한다.

- resource는 동사보다는 명사를, 대문자보다는 소문자를 사용한다.
- resource의 스토어 이름으로는 복수 명사를 사용해야 한다.

<br/>

### 1.2 자원에 대한 행위는 HTTP Method (GET, POST, PUT, DELETE)로 표현해야 한다.
- HTTP Method나 동사표현이 URI에 들어가면 안됩니다.

    Bad
    ```
    GET /chatrooms/get/:id
    POST /chatrooms/create
    GET /chatrooms/delete/:id
    POST /chatrooms/update/:id
    ```

    Good
    ```
    GET /chatrooms/:id
    POST /chatrooms
    DELETE /chatrooms/:id
    PUT /chatrooms/:id
    ```

<br/>

### 1.3 HTTP Methods

- 주요 Method
    - **GET**: 조회 (받겠다)
    - **POST**: 리소스 생성 (보내겠다)
    - **PUT**: 리소스 전체 갱신(놓겠다/넣겠다)
    - **PATCH**: 리소스 부분 갱신(붙이겠다)
    - **DELETE**: 리소스 삭제 (지정한 서버의 파일을 삭제하겠다)

- 나머지 Method
    - **HEAD**: GET과 유사하나 HTTP 메시지 헤더만 반송하고 데이터 내용을 돌려보내지 않음(헤더만 보겠다)
    - **OPTION**: 통신 옵션을 통지/조사
    - **TRACE**: 리퀘스트 라인과 헤더를 그대로 클라이언트에 반송(리퀘스트 치환상태를 조사할 때 사용)
    - **CONNECT**: 암호화된 메시지를 프록시로 전송

<br/>

## 2. URL 디자인 가이드

### 2.1 마지막에 `/`를 포함하지 않는다.
Bad
```
http://api.test.com/users/
```

Good
```
http://api.test.com/users
```

<br/>

### 2.2 자원을 표현하는 리소스 형식

REST API는 리소스 를 중심으로 디자인되며, 클라이언트에서 액세스할 수 있는 모든 종류의 개체, 데이터 또는 서비스가 리소스에 포함된다.

```
http:// restapi.example.com/sports/soccer
```

위 URI를 보면, sports라는 컬렉션과 soccer라는 도큐먼트로 표현되고 있다. 좀 더 예를 들면

```
http:// restapi.example.com/sports/soccer/players/13
```

sports, players 컬렉션과 soccer, 13(13번인 선수)를 의미하는 도큐먼트로 URI가 이루어진다. 여기서 중요한 점은 **컬렉션은 복수**로 사용하고 있다는 점입니다. 좀 더 직관적인 REST API를 위해서는 컬렉션과 도큐먼트를 사용할 때 단수 복수도 지켜준다면 좀 더 이해하기 쉬운 URI를 설계할 수 있다.

<br/>

### 2.3 `_`(underbar) 대신 `-`(hyphen)를 사용한다.
-(dash)의 사용도 최소한으로 설계한다. 정확한 의미나 표현을 위해 단어의 결합이 불가피한 경우 혹은 가독성을 높이기 위한 경우 반드시 -(dash) 사용한다.

Bad
```
http://api.test.com/users/post_commnets
```

Good
```
http://api.test.com/users/post-commnets
```


<br/>

### 2.4 소문자를 사용한다.

Bad
```
http://api.test.com/users/postCommnets
```

Good
```
http://api.test.com/users/post-commnets
```

<br/>

### 2.5 행위(method)는 URL에 포함하지 않는다.

Bad
```
POST http://api.test.com/users/1/delete-post/1
```

Good
```
DELETE http://api.test.com/users/1/posts/1
```

<br/>

### 2.6 컨트롤 자원을 의미하는 URL 예외적으로 동사를 허용한다.
Http Method를 제외한 특정 기능을 요구하는 컨트롤 리소스의 경우 동사를 사용할 수 있다.

Bad
```
http://api.test.com/posts/duplicating
```

Good
```
http://api.test.com/posts/duplicate
```


<br/>

### 2.7 파일 확장자는 URI에 포함시키지 않는다.

Bad
```
http://api.example.com/device-management/managed-devices.xml
```

Good
```
http://api.example.com/device-management/managed-devices
```

<br/>

### 2.8 리소스 간의 관계를 표현하는 방법
REST 리소스 간에는 연관 관계가 있을 수 있고, 이런 경우 다음과 같은 표현방법으로 사용한다.
```
/리소스명/리소스 ID/관계가 있는 다른 리소스명

ex)    GET : /users/{userid}/devices (일반적으로 소유 ‘has’의 관계를 표현할 때)
```

만약에 관계명이 복잡하다면 이를 서브 리소스에 명시적으로 표현하는 방법이 있다. 예를 들어 사용자가 ‘좋아하는’ 디바이스 목록을 표현해야 할 경우 다음과 같은 형태로 사용될 수 있다.
```
GET : /users/{userid}/likes/devices (관계명이 애매하거나 구체적 표현이 필요할 때)
```

<br/>

### 2.9 API 버전 관리
버전관리를 위해 URI는 다음과 같은 형태로 정의할 것을 권장한다
```
{servicename}/{version}/{REST URL}
예)api.server.com/account/v2.0/groups
```
이는 서비스의 배포 모델과 관계가 있는데 자바 애플리케이션의 경우 account.v1.0.war, account.v2.0.war와 같이 다른 war로 각각 배포하여 버전 별로 배포 바이너리를 관리할 수 있고 앞단에 서비스명을 별도의 URL로 떼어 놓는 것은 이후에 서비스가 확장되었을 때 account 서비스만 별도의 서버로 분리해서 배포하는 경우를 대비하기 위함이다.

외부로 제공되는 URL은 `api.server.com/account/v2.0/groups`로 하나의 서버를 가리키지만, 내부적으로 HAProxy 등의 Reverse Proxy를 이용해서 이런 URL을 맵핑할 수 있는데, `api.server.com/account/v2.0/groups`를 내부적으로 `account.server.com/v2.0/groups`로 맵핑하도록 하면 외부에 노출되는 URL 변경 없이 향후 확장되었을 때 서버를 물리적으로 분리해내기가 편리하다.


<br/>

### 2.10 페이징
페이스북 API가 직관적이기 때문에 페이스북 스타일을 사용할 것을 권장한다. 

ex) 100번째 레코드부터 125번째 레코드를 받는 API 정의하기
```
페이스북 스타일 : /record?offset=100&limit=25
```
100번째 레코드에서부터 25개의 레코드를 출력한다는 의미이다.

<br/>

## 3. Use HTTP methods

### 3.1 POST, GET, PUT, DELETE 4가지 methods는 반드시 제공한다.

|  methods   |    POST     |        GET        |            PUT             |      DELETE       |
| :--------: | :---------: | :---------------: | :------------------------: | :---------------: |
|   /users   | 사용자 추가 | 사용자 전체 조회  | 사용자 추가 or 사용자 수정 | 사용자 전체 삭제  |
| /users/hak |  405 ERROR  | 사용자 ‘hak’ 조회 |     사용자 ‘hak’ 수정      | 사용자 ‘hak’ 삭제 |

- `PUT /users` 경우 표에선 사용자 추가 or 사용자 수정으로 했지만, 보통 `Collection`에 PUT 요청은 지원하지 않으므로 **405 ERROR** 응답하기도 한다.
    - Collection: /users/hak 에서 users, 집합
    - Document: /users/hak 에서 hak, 집합에 속한 자원

<br/>

### 3.2 OPTIONS, HEAD, PATCH를 사용하여 완성도 높은 API를 만든다.

#### 3.2.1 OPTIONS

https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/OPTIONS
현재 End-point가 제공 가능한 API method를 응답한다.

```
OPTIONS /users/hak
```

```
HTTP/1.1 200 OK
Allow: GET,PUT,DELETE,OPTIONS,HEAD
```

<br/>

#### 3.2.2 HEAD

https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/HEAD
요청에 대한 Header 정보만 응답한다. body가 없다.

```
HEAD /users
```

```
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 120
```

<br/>

#### 3.2.3 PATCH

https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PATCH
자원의 일부를 수정할 때는 PUT 대신 PATCH method를 사용한다.

PUT 요청 시 요청을 일부분만 보낸 경우 나머지는 default 값으로 수정되는 게 원칙이다. 그러나, 대부분 PUT 요청에서 이와 같이 개발하진 않는다.


<br/>

## 4. Use HTTP status

### 4.1 의미에 맞는 HTTP status를 리턴한다

Bad
```
HTTP/1.1 200 OK
{
    "result" : false,
    "status" : 400
}
```
- status는 200으로 성공인데 body 내용엔 실패에 관한 내용을 리턴하고 있다.
- 모든 응답을 200으로 처리하고 body 내용으로 성공|실패를 판단하는 구조에서 사용된다. 잘못된 설계다.

Good
```
HTTP/1.1 400 Bad Request
{
    "msg" : "check your parameter"
}
```

<br/>

### 4.2 HTTP status만으로 상태 에러를 나타낸다
세부 에러 사항은 응답 객체에 표시하거나, 해당 에러를 확인할 수 있는 link를 표시한다.
http 상태 코드를 응답 객체에 중복으로 표시할 필요 없다.

Bad
```
HTTP/1.1 404 Not Found
{
    "code" : 404,
    "error_code": -765
}
```

Good
```
HTTP/1.1 404 Not Found
{
    "code" : -765,
    "more_info" : "https://api.test.com/errors/-765"
}
```

<br/>

### 4.3 에러 처리
에러 처리의 기본은 HTTP 응답 코드를 사용한 후 HTTP 응답 코드를 사용한 후 응답 보디(Response Body)에 에러에 대한 자세한 내용을 서술하는 것이다. 다음과 같은 응답 코드만 사용하는 것을 권장한다. 
```
 200 - 성공 
 400 Bad Request - field validation 실패 시 
 401 Unauthorized - API 인증, 인가 실패
 404 Not found - 해당 리소스가 없음
 500 Internal Server Error - 서버 에러 
```
에러에는 에러 내용에 대한 구체적인 내용을 HTTP 보디에 정의해서 상세한 에러의 원인을 전달하는 것이 디버깅에 유리하다. 

```
HTTP Status Code: 401
{
    "status":"401", "message":"Authenticate","code":200003, "more in-fo":"http://www.twillo.com/docs/errors/20003"
}
```

에러 발생 시에 선택적으로 에러에 대한 스택 정보를 포함 시킬 수 있다. 에러 메세지에서 에러 스택 정보를 출력하는 것은 대단히 위험한 일이다. 내부적인 코드 구조와 프레임워크 구조를 외부에 노출함으로써, 해커들에게 해킹을 할 수 있는 정보를 제공하기 때문이다. 일반적인 서비스 구조에서는 에러 스택 정보를 API 에러 메세지에 포함 시키지 않는 것이 바람직하다.

<br/>

# 번외) GraphQL
GraphQL은 Graph 이론을 기반으로 만든 API를 만들 때 사용하는 SQL 언어이다. 데이터에 대한 스키마 정의를 한 기준으로 쿼리를 실행하여 데이터를 불러오는 방식이다.

<br/>

## GraphQL 특징
### 데이터 형태 정의
데이터 형태를 미리 정의 함으로써 쿼리가 반환하는 데이터 내용을 미리 예측할 수 있고 앱에서 필요한 데이터에 대한 쿼리를 작성할 수 있다.

<br/>

### 계층적
GraphQL은 자연스럽게 객체와의 관계를 따르게 된다. 필드 안에 다른 필드가 중첩될 수 있는 것이다. 그렇기에 GraphQL은 그래프 구조의 인터페이스와 잘 어울린다.

<br/>

### 엄격한 타입
GraphQL은 스키마 정의로 엄격하게 타입을 정의한다. 이로 인해 유효성 검사를 하게되며 오류 메시지도 제공한다.

<br/>

### 저장소가 아닌 프로토콜
GraphQL은 저장소 계층을 사용하지 않는다. 저장소에서 데이터를 불러오는 비즈니스 로직이 구현된 어플리케이션 계층을 사용하여 데이터를 제공한다.

<br/>

## GraphQL vs REST
- GraphQL은 하나의 Endpoint를 갖는다. (클라이언트는 한 군데로만 요청을 보내면 된다.)

![](https://i.imgur.com/24cvgie.png)

- REST에서는 Resource의 크기와 형태를 서버에서 결정하지만, GraphQL에서는 Resource에 대한 정보만 정의하고, 필요한 크기와 형태는 client단에서 요청 시 결정한다.

<br/>

### REST의 단점
#### Overfetching
특정 인물에 대한 이름, 키, 몸무게를 불러와야된다고 가정해보자.
기존 Rest 방식으로는 아래와 같은 데이터를 받을 수 있다.

```
{
  "name": "Luke Skywalker",
  "height": "172",
  "mass": "77",
  "hair_color": "blond",
  "skin_color": "fair",
  "eye_color": "blue",
  "birth_year": "19BBY",
  "gender": "male",
  "homeworld": "http://swapi.dev/api/planets/1/",
  "films": [
    "http://swapi.dev/api/films/1/",
    "http://swapi.dev/api/films/2/",
    "http://swapi.dev/api/films/3/",
    "http://swapi.dev/api/films/6/"
  ],
  "species": [],
  "vehicles": [
    "http://swapi.dev/api/vehicles/14/",
    "http://swapi.dev/api/vehicles/30/"
  ],
  "starships": [
    "http://swapi.dev/api/starships/12/",
    "http://swapi.dev/api/starships/22/"
  ],
  "created": "2014-12-09T13:50:51.644000Z",
  "edited": "2014-12-20T21:17:56.891000Z",
  "url": "http://swapi.dev/api/people/1/"
}
```

불필요한 데이터가 너무 많이 포함되어 전송됬다. GraphQL을 이용하여 데이터를 불러와보자

```
query {
  person(personID: 1) {
    name
    height
    mass
  }
}
```

```
{
  "data": {
    "person": {
      "name": "Luke Skywalker",
      "height": 172,
      "mass": 77
    }
  }
}
```

Rest와 달리 필요한 필드만 불러올 수 있다. 여기서 서버에서 전송하는 데이터 양을 줄여 성능을 높일 수 있다.

<br/>

#### Underfetching
특정 인물이 등장한 영화 이름들이 필요하다고 가정해보자.
Rest의 경우 특정 인물에 대해 요청하고 그 결과에 있는 영화들을 다시 불러와야 된다.

```
{
  "name": "Luke Skywalker",
  "height": "172",
  "mass": "77",
  "hair_color": "blond",
  "skin_color": "fair",
  "eye_color": "blue",
  "birth_year": "19BBY",
  "gender": "male",
  "homeworld": "http://swapi.dev/api/planets/1/",
  "films": [
    "http://swapi.dev/api/films/1/",
    "http://swapi.dev/api/films/2/",
    "http://swapi.dev/api/films/3/",
    "http://swapi.dev/api/films/6/"
  ],
  "species": [],
  "vehicles": [
    "http://swapi.dev/api/vehicles/14/",
    "http://swapi.dev/api/vehicles/30/"
  ],
  "starships": [
    "http://swapi.dev/api/starships/12/",
    "http://swapi.dev/api/starships/22/"
  ],
  "created": "2014-12-09T13:50:51.644000Z",
  "edited": "2014-12-20T21:17:56.891000Z",
  "url": "http://swapi.dev/api/people/1/"
}
```

```
// 결과 4개
{
  "title": "A New Hope",
  "episode_id": 4,
  "opening_crawl": "It is a period of civil war.\r\nRebel spaceships, striking\r\nfrom a hidden base, have won\r\ntheir first victory against\r\nthe evil Galactic Empire.\r\n\r\nDuring the battle, Rebel\r\nspies managed to steal secret\r\nplans to the Empire's\r\nultimate weapon, the DEATH\r\nSTAR, an armored space\r\nstation with enough power\r\nto destroy an entire planet.\r\n\r\nPursued by the Empire's\r\nsinister agents, Princess\r\nLeia races home aboard her\r\nstarship, custodian of the\r\nstolen plans that can save her\r\npeople and restore\r\nfreedom to the galaxy....",
  "director": "George Lucas",
  "producer": "Gary Kurtz, Rick McCallum",
  "release_date": "1977-05-25",
  "characters": [
    "http://swapi.dev/api/people/1/",
    "http://swapi.dev/api/people/2/",
    "http://swapi.dev/api/people/3/",
    "http://swapi.dev/api/people/4/",
    "http://swapi.dev/api/people/5/",
    "http://swapi.dev/api/people/6/",
    "http://swapi.dev/api/people/7/",
    "http://swapi.dev/api/people/8/",
    "http://swapi.dev/api/people/9/",
    "http://swapi.dev/api/people/10/",
    "http://swapi.dev/api/people/12/",
    "http://swapi.dev/api/people/13/",
    "http://swapi.dev/api/people/14/",
    "http://swapi.dev/api/people/15/",
    "http://swapi.dev/api/people/16/",
    "http://swapi.dev/api/people/18/",
    "http://swapi.dev/api/people/19/",
    "http://swapi.dev/api/people/81/"
  ],
  "planets": [
    "http://swapi.dev/api/planets/1/",
    "http://swapi.dev/api/planets/2/",
    "http://swapi.dev/api/planets/3/"
  ],
  "starships": [
    "http://swapi.dev/api/starships/2/",
    "http://swapi.dev/api/starships/3/",
    "http://swapi.dev/api/starships/5/",
    "http://swapi.dev/api/starships/9/",
    "http://swapi.dev/api/starships/10/",
    "http://swapi.dev/api/starships/11/",
    "http://swapi.dev/api/starships/12/",
    "http://swapi.dev/api/starships/13/"
  ],
  "vehicles": [
    "http://swapi.dev/api/vehicles/4/",
    "http://swapi.dev/api/vehicles/6/",
    "http://swapi.dev/api/vehicles/7/",
    "http://swapi.dev/api/vehicles/8/"
  ],
  "species": [
    "http://swapi.dev/api/species/1/",
    "http://swapi.dev/api/species/2/",
    "http://swapi.dev/api/species/3/",
    "http://swapi.dev/api/species/4/",
    "http://swapi.dev/api/species/5/"
  ],
  "created": "2014-12-10T14:23:31.880000Z",
  "edited": "2014-12-20T19:49:45.256000Z",
  "url": "http://swapi.dev/api/films/1/"
}
```

위 데이터에서 title만 필요한 상황이다.

만약 특정 인물은 5명이 필요하고 특정 인물당 10번 영화에 등장했다고 하면 50(5*10)번의 요청이 필요하다. 여기서 영화 데이터에서는 title만 필요하니 불필요한 데이터가 너무 많기도 하다.

이번엔 GraphQL을 이용하여 요청해보자.

```
query {
  person(personID: 1) {
    filmConnection {
      films {
        title
      }
    }
  }
}
```

```
{
  "data": {
    "person": {
      "filmConnection": {
        "films": [
          {
            "title": "A New Hope"
          },
          {
            "title": "The Empire Strikes Back"
          },
          {
            "title": "Return of the Jedi"
          },
          {
            "title": "Revenge of the Sith"
          }
        ]
      }
    }
  }
}
```

Rest와 확연한 차이가 보인다. Rest는 여러 번 요청한 것에 비해 GraphQL은 한 번의 요청으로 원하는 데이터를 불러오고 불필요한 데이터는 제외시켰다.