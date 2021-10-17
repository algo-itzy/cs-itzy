# Load Balancing

* 부하분산 : 해야 할 작업을 나눠서 서버의 부하를 분산시키는 서비스.

- request를 각각 서버들이 원하는 대로 나눠주는 것.

* 서비스 이용자가 많아지며 트래픽을 현재의 서버로 감당하기 힘들어져 나온 개념.(Scale Out)
  * 서비스 트래픽 증가로 인해 하나의 서버로 감당이 불가능

    => Scale UP!(서버 성능을 올리자!)

  * 그래도 안 돼?

    => Scale Out!(여러대의 서버로 나누자!)

미리 측정된 웹 서버의 능력에 따라 분배 or 서버의 부하상태에 따라 분배.

<br>

<br>

## 부하 할당 알고리즘

- Round Robin
  - 단순히 Round Robin으로 분산하는 방식입니다.
- Least Connections
  - 연결 개수가 가장 적은 서버를 선택하는 방식입니다.
  - 트래픽으로 인해 세션이 길어지는 경우 권장하는 방식입니다.
- Source
  - 사용자의 IP를 Hashing하여 분배하는 방식입니다.
  - 사용자는 항상 같은 서버로 연결되는 것을 보장합니다.

<br>

<br>

## Load balancer 종류

* OSI 7 Layer 기준으로 L2, L3, **L4, L7**을 기준으로 나눔.

* (L2는맥주소, L3은 IP주소)

<br>

### L4(Transport Layer)

* IP & Port Level에서 로드 밸런싱(TCP/UDP)

* 그냥 포트 80으로 오는 트래픽을 서버A, B로 나눠줌. 

[![No Image](https://nesoy.github.io/assets/posts/20180602/5.png)](https://nesoy.github.io/assets/posts/20180602/5.png)

<br>

### L7(Application Layer)

* User Request Level에서 로드 밸런싱(HTTPS/HTTP/FTP)
* URL, Query, Route에 따라 로드 밸런싱.

[![No Image](https://nesoy.github.io/assets/posts/20180602/6.png)](https://nesoy.github.io/assets/posts/20180602/6.png)



<br>

### Client - Proxy - WAS 간 식별 

[![No Image](https://nesoy.github.io/assets/posts/20180602/4.png)](https://nesoy.github.io/assets/posts/20180602/4.png)

- X-Forwarded-For

  - HTTP 또는 HTTPS 로드 밸런서를 사용할 때 클라이언트의 IP 주소를 식별하는 데 도움을 줍니다.

- X-Forwarded-Proto

  - 클라이언트가 로드 밸런서 연결에 사용한 프로토콜(HTTP 또는 HTTPS)을 식별하는 데 도움을 줍니다.

- X-Forwarded-Port

  - 클라이언트가 로드 밸런서 연결에 사용한 포트를 식별하는 데 도움을 줍니다.

<br>

<br>

## Load Balancer 장애 시 전략

- Load Balancer를 이중화(Active, Passive)

[![No Image](https://nesoy.github.io/assets/posts/20180602/7.png)](https://nesoy.github.io/assets/posts/20180602/7.png)



[![No Image](https://nesoy.github.io/assets/posts/20180602/8.gif)](https://nesoy.github.io/assets/posts/20180602/8.gif)

- Health Check : 초당 몇 회 요청, 응답을 반복해 기능이 살아있나 확인함.
- Main Load Balancer가 동작하지 않으면 가상IP(VIP, Virtual IP)는 여분의 Load Balancer로 변경

