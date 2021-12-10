## Web Server vs Web Application Server

### 1. Static Pages vs Dynamic Pages

![ws_was](https://gmlwjd9405.github.io/images/web/static-vs-dynamic.png)

* 정적 페이지

  - image, html, css, javascript 파일과 같이 컴퓨터에 저장되어 있는 파일들을 의미

  - 웹 서버에서 요청에 알맞은 파일을 반환하며, 항상 동일한 정해진 페이지를 반환
  - 웹 서버에서 제공

* 동적 페이지

  * 들어온 요청에 맞게 동적으로 만들어진 컨텐츠
  * 데이터베이스, 서버내 로직 등을 활용해 생성한 컨텐츠를 반환
    * 비즈니스 로직과 관련한 내용이 담김
  * 웹 어플리케이션 서버에서 제공

### 2. WAS의 개념적 구조

![was](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FtmkV7%2FbtqUhAga0Wj%2FiwmIrkS2BqYhiWESjTk5m1%2Fimg.png)

![ws](https://gmlwjd9405.github.io/images/web/webserver-vs-was2.png)

좌: Web Server 우: Web Application Server

### 3. 웹 서버

* Web Server의 기능
  * HTTP 프로토콜을 기반으로 하여 클라이언트(웹 브라우저 또는 웹 크롤러)의 요청을 서비스 하는 기능을 담당
* 정적인 컨텐츠 제공
  * WAS를 거치지 않고 바로 자원을 제공한다.
* 동적인 컨텐츠 제공을 위한 요청 전달
  * 클라이언트의 요청(Request)을 WAS에 보내고, WAS가 처리한 결과를 클라이언트에게 전달(응답, Response)한다.
  * 클라이언트는 일반적으로 웹 브라우저를 의미한다.

### 4. 웹 어플리케이션 서버

* DB 조회나 다양한 로직 처리를 요구하는 동적인 컨텐츠를 제공하기 위해 만들어진 Application Server
  HTTP를 통해 컴퓨터나 장치에 애플리케이션을 수행해주는 미들웨어(소프트웨어 엔진)이다.

* Web Server + Web Container

  * “웹 컨테이너(Web Container)” 혹은 “서블릿 컨테이너(Servlet Container)”라고도 불린다.
    Container란 JSP, Servlet을 실행시킬 수 있는 소프트웨어를 말한다.

  * 그럼 컨테이너(Container) 는 무엇인가?

    \- 웹 서버가 보낸 JSP, PHP, ASP.net 등의 파일들을 실행하고 수행결과를 다시 웹 서버로 보내주는 역할을 한다.

    결국, 웹 어플리케이션 서버는 웹 서버에서 요청을 받고, 이를 웹 컨테이너로 보내 로직(알고리즘, DB 연결 등)을 수행하고 그 결과를 다시 웹 서버로 보내 최종적으로 클라이언트에게 보내주는 것이다.

* 현재는 WAS가 가지고 있는 Web Server도 정적인 컨텐츠를 처리하는 데 있어서 성능상 큰 차이가 없다.

* 정리
  프로그램 실행 환경과 DB 접속 기능 제공
  여러 개의 트랜잭션(논리적인 작업 단위) 관리 기능
  업무를 처리하는 비즈니스 로직 수행

### 5. 왜 WAS를 쓰는건가요?

* 웹 페이지는 정적 컨텐츠와 동적 컨텐츠가 모두 존재
  * 사용자의 요청에 맞게 적절한 동적 컨텐츠를 만들어서 제공해야
  * 이때, Web Server만을 이용한다면 사용자가 원하는 요청에 대한 결과값을 모두 미리 만들어 놓고 서비스를 해야함
  * 하지만 이렇게 수행하기에는 자원이 절대적으로 부족하다.
  * 따라서 WAS를 통해 요청에 맞는 데이터를 DB에서 가져와서 비즈니스 로직에 맞게 그때 그때 결과를 만들어서 제공함으로써 자원을 효율적으로 사용할 수 있음

### 5. 그냥 WAS만 쓰면 되는거 아닌가요 그럼?

* Web Server에서는 정적 컨텐츠만 먼저 처리하도록 기능을 분배하여 서버의 부담을 줄이고, 사용자 경험 개선
* 다수의 WAS를 연결하는 로드밸런싱 역할을 Web Server가 수행하기도 함
* 물리적 분리를 통한 보안 강화
* :star: `자원 이용의 효율성 및 장애 극복, 배포 및 유지보수의 편의성 `

## Proxy

### 1. Proxy 서버란

* 대리인. 쉽게 말해 Client와 Server 사이를 이어주는 대리인 역할을 하는 서버를 Proxy Server라고 합니다.
* 클라이언트와 서버간의 통신을 중계하여 대리 수행하는 서버.

### 2. 왜 프록시를 사용하나요?

* 웹 서비스에서 제공하는 데이터는 정적인 데이터와 동적인 데이터 두가지 종류가 있습니다.
* 동적 데이터의 경우 응답하는 내용이 매번 달라지므로 중복되지 않는 요청이며, DB를 읽거나 계산등으로 인해서 정적 데이터에 비해서 응답하는 시간이 적게는 2배에서 많게는 10배 정도 오래 걸립니다.
* **중복되는 요청을 매번 처리하기에는 너무 많은 자원 낭비가 발생**
* 따라서 수십, 수백만 클라이언트의 요청이 매번 서버로 가지 않도록 중복되는 요청을 제공하는 캐시의 역할을 수행하는 것이 프록시입니다.

### 3. Forward Proxy vs Reverse Proxy

* 두 프록시의 공통된 특징 - 캐싱
* 자주 사용하는 데이터를 확보해 두고, 이를 캐싱 (재활용)해서 클라이언트에게 제공
  1. 전송시간 절약
  2. 불필요한 외부 전송 X
  3. 외부 요청 감소
     * 네트워크 Bottleneck 감소

#### 3-1. Forward Proxy

![fw](https://www.cloudflare.com/img/learning/cdn/glossary/reverse-proxy/forward-proxy-flow.svg)

* 클라이언트와 웹 사이에 있는 프록시

##### 특징

1. 익명성 (IP 우회)
   * 서버는 요청 주체를 Forward Proxy로 알고 있고, 원래 클라이언트의 정보를 모르게 됨
   * 통신사 IP를 생각하면 쉽습니다.

#### 3-2 Reverse Proxy

![rv](https://www.cloudflare.com/img/learning/cdn/glossary/reverse-proxy/reverse-proxy-flow.svg)

* 웹과 서버 사이에 있는 프록시

##### 특징

1. 보안
   * 클라이언트들은 리버스 프록시를 서버라고 생각. 실제 서버의 IP를 제공하지 않기 때문에 실제 서버를 보호할 수 있음.
   * 실제 서버에 접근할 수 없기 때문에 다소 취약한 http 프레임 워크를 숨겨서 보호할 수가 있다.
2. 로드 밸런싱 (Optional)
   * 승주 설명 참고
   * 클라이언트의 요청(load)을 각 서버들에게 분배 (balancing)
   * 최근 웹 서버 확장의 트렌드인 Scale Out (병렬 확장)된 웹 서버들에게 L4, L7 방식으로 부하를 분배

### 4. 프록시 캐시는 어떻게 관리해요?

* 캐시 만료 기한을 두고 만료 기한 도래시 마다 캐시를 갱신하는 방법
* 만료 기한 이후에 다시 클라이언트의 요청이 있으면, 프록시 대신 서버에서 로드하고, 프록시를 갱신



## Reference

webserver vs was

https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html