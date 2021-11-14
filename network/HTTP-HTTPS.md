## HTTP (Hypertext Transfer Protocol)

- 브라우저와 서버 간에 데이터를 주고받기 위한 방식
- 상태가 없는(stateless) 프로토콜
  - = 각각의 데이터 요청이 서로 독립적으로 관리가 된다
  - = 이전 데이터 요청과 다음 데이터 요청이 서로 관련이 없다
- 일반적으로 TCP/IP 방식
- 기본 포트 - 80

### URL

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6c43b630-b769-4e89-b201-74fcab7553f6/Untitled.png)

## HTTP Request Methods

- **GET** : 존재하는 자원에 대한 **요청**
- **POST** : 새로운 자원을 **생성**
  - POST 메서드로 PUT, DELETE의 동작도 수행 가능
- **PUT** : 존재하는 자원에 대한 **변경**
- **DELETE** : 존재하는 자원에 대한 **삭제**

## HTTP Status Code

- 서버에서 응답하는 정보

### **2xx - 성공**

- 200 : GET 요청에 대한 성공
- 204 : No Content. 성공했으나 응답 본문에 데이터가 없음
- 205 : Reset Content. 성공했으나 클라이언트의 화면을 새로 고침하도록 권고
- 206 : Partial Conent. 성공했으나 일부 범위의 데이터만 반환

### 3xx - 리다이렉션

- 301 : Moved Permanently, 요청한 자원이 새 URL에 존재
- 303 : See Other, 요청한 자원이 임시 주소에 존재
- 304 : Not Modified, 요청한 자원이 변경되지 않았으므로 클라이언트에서 캐싱된 자원을 사용하도록 권고

### 4xx - 클라이언트 에러

- 400 : Bad Request, 잘못된 요청
- 401 : Unauthorized, 권한 없이 요청. Authorization 헤더가 잘못된 경우
- 403 : Forbidden, 서버에서 해당 자원에 대해 접근 금지
- **404** : Not Found, 요청한 자원이 서버에 없다
- 405 : Method Not Allowed, 허용되지 않은 요청 메서드
- 409 : Conflict, 최신 자원이 아닌데 업데이트하는 경우. ex) 파일 업로드 시 버전 충돌

### 5xx - 서버 에러

- 501 : Not Implemented, 요청한 동작에 대해 서버가 수행할 수 없는 경우
- 503 : Service Unavailable, 서버가 과부하 또는 유지 보수로 내려간 경우

## HTTP Versions

### HTTP 0.9

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/316d2e71-0d3b-477b-9b3e-1f9e08d2b507/Untitled.png)

- 원라인 프로토콜
- GET 방식만 존재
- 내용은 파일 자체로만 구성

### HTTP 1.0

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ae07ee1c-65c1-42d3-b596-8f97a145843d/Untitled.png)

- 단기 커넥션(Short-lived connections)
  - Request를 날릴 때마다 Connection을 새로 생성
  - Connection 하나당 Request 하나만 붙일 수 있다
- 헤더에 버전이 붙음
- 응답엔 상태 코드, 타입 등이 붙음
  - HTML 말고 다른 파일 전송도 가능

### **HTTP 1.1**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0413261d-ae8c-4b0e-b9f3-d7c2644e2fdc/Untitled.png)

- 지속 커넥션(Persistent connection)
  - 지정한 Timeout 동안 커넥션이 열려 있어 커넥션 재사용 가능 (여러 요청 가능)
- HTTP 파이프라이닝(Pipelining)
  - Request를 미리 여러개 서버에 날릴 수 있음
  - 그러나 요청 처리는 순차적 → But, 첫 순서가 오래걸리면 이후 요청이 다 밀린다

### **HTTP 2.0**

- Multiplexing
  - 프레임 단위로 나눠서 전송,관리 가능
  - 다수의 요청과 응답이 가능한 구조
- 데이터 전송
  - 바이너리로 인코딩하여 전송
  - 텍스트 방식보다 속도 상승, 오류 감소
- ServerPush 사용
  - 브라우저에서 필요한 리소스들을 서버가 알아서 찾아다가 내려줌

# HTTPS (임시)

- HTTP 에 암호화와 인증, 그리고 완전성 보호를 더한 HTTPS
  - HTTP 통신하는 소켓 부분을 `SSL(Secure Socket Layer)` or `TLS(Transport Layer Security)`라는 프로토콜로 대체
  - HTTP 는 원래 TCP 와 직접 통신했지만, HTTPS 에서 HTTP 는 SSL 과 통신하고 **SSL 이 TCP 와 통신**
  - SSL 을 사용한 HTTPS 는 암호화와 증명서, 안전성 보호를 이용 가능

### **모든 웹 페이지에서 HTTPS를 사용해도 될까?**

- 암호화 통신은 리소스를 더 많이 요구한다
- 하드웨어의 발달로 인해 HTTPS를 사용하더라도 속도 저하가 거의 일어나지 않음
- 새로운 표준인 HTTP 2.0을 함께 이용한다면 오히려 HTTPS가 HTTP보다 더 빠르게 동작
- 따라서 현재 모든 웹 페이지에서 HTTPS를 적용하는 방향으로 변화

### **HTTPS 통신 흐름**

- 애플리케이션 서버(A)를 만드는 기업은 HTTPS를 적용하기 위해 공개키와 개인키를 만든다.
- 신뢰할 수 있는 CA 기업을 선택하고, 그 기업에게 내 공개키 관리를 부탁하며 계약을 한다.
- **_CA란?_** : Certificate Authority로, 공개키를 저장해주는 신뢰성이 검증된 민간기업
