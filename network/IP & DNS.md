[TOC]



## IP & DNS

### IP

- Internet Protocol address
- 인터넷에 접속할 때 접속자에 대한 최소한의 정보가 공개되는 **인터넷 신분증**



### DNS

- Domain Name System
- 숫자로 구성된 네트워크 주소인 IP주소를 사람이 이해하기 쉬운 명칭인 도메인 이름으로 매칭해주는 시스템

#### 구조

![img](https://media.vlpt.us/images/cmin95/post/b3fb2909-67c2-4ddf-a3af-52b00832aa2e/2316A93F51C462940C.gif)



- 최상위 : .(Root), 인터넷도메인의 시작점
- 다음 단계 : 최상위도메인(TLD, Top Level Domain)
- 하위 단계로 여러 도메인 존재

#### 원리

![img](https://media.vlpt.us/images/cmin95/post/f591bf5b-a368-4565-8ab1-7dbe8b71dc24/Netmanias.2011.12.12-DNS_Basic.gif)

1. 웹 브라우저에 `www.naver.com`을 입력하면, 단말에 미리 설정되어 있는 Local DNS에게 `www.naver.com이라는 hostname`에 대한 IP 주소를 요청. 이 때 이에 대한 데이터가 존재한다면 IP주소를 제공
2. 관련된 데이터가 없다면 다른 DNS 서버들과 통신을 시작하고, 제일 먼저 Root DNS 서버에 위 주소에 대한 정보를 요청. Root DNS는 DNS의 루트 존으로, 전 세계에 13대만 구축되어 있는 서버이다. (우리나라는 미러서버를 운영)
3. Root DNS 서버는 Local DNS 서버에게 다른 DNS 서버(TLD, 여기서는 com DNS)에게 관련된 데이터를 요청하도록 유도, 응답
4. Local DNS는 TLD(com DNS)에 데이터를 요청
5. TLD는 다시 Local DNS에게 또 다른 DNS 서버(`naver.com DNS`)에게 관련된 데이터를 요청하도록 응답
6. Local DNS는 마지막으로 `naver.com`을 관리하는 DNS서버에 hostname에 대한 IP 주소를 요청
7. `naver.com DNS`는 Local DNS에게 IP주소 "222.122.195.6" 응답
8. Local DNS는 빠른 응답을 위해 `www.naver.com`에 대한 IP주소를 캐싱하고, 그 정보를 단말(PC)에 전달



#### DNS Round Robin

- 별도의 SW,HW 로드벨런싱 장비 없이 오직 DNS만을 이용하여 도메인 레코드 정보를 조회하는 시점에서 트래픽을 분산하는 기법

  - 로드밸런싱

    ![img](https://blog.kakaocdn.net/dn/yY2sc/btq4VBpjmAM/kSvBRPxrUUwGKVObTd08k1/img.png)

    - 여러대의 서버를 준비하여 들어오는 트래픽을 여러 서버로 분산

- 웹 뿐만 아니라, 도메인을 사용하는 모든 서비스(FTP,SMTP 등)에 사용 가능



##### 원리

- 웹 서비스를 담당할 여러 대의 웹 서버는 자신의 공인 IP를 가지고 있음
- 사이트 접속을 위해 사용자가 해당 도메인 주소를 브라우저에 입력하면 DNS는 도메인의 정보를 조회
  - IP주소를 여러 대의 서버 IP리스트 중에서 라운드 로빈 형태로 랜덤하게 하나 혹은 여러개를 선택하여 사용자에게 알려준다.
- 실제로는 복수의 웹 서버에 나뉘어 접속하여 자연스럽게 서버의 부하가 분산

**라운드 로빈 DNS는 여러개의 IP주소를 결과로 돌려준다.**



##### 장점

- 중간 장비(로드밸런서) 없이도 서비스가 가능
- 비용적인 부분이 줄어들고 간편

 

##### 단점

- 서버의 수 만큼 공인 IP 주소가 필요
- 균등하게 분산되지 않음
  - 로드밸런서 : 클라이언트와 상관없이 들어오는 요청 분산 
  - DNS Round Robin : 한 클라이언트의 요청이 보낼 때 마다 다른 서버에 보내지도록 만드는 방법. 한 사람의 요청은 계속 분산되지만, 여러 사람이 요청을 보내는 경우 균등하게 부하를 분산시킨다고 보장 X
  - DNS에서 제공한 정보를 캐싱할 경우(웹 브라우저 및 모바일) 다른 IP주소를 제공해도 캐싱한 주소로 연결
- 서버에 장애 발생시 감지가 힘들다.
  - 로드밸런서 : 트래픽을 담당 서버의 상태를 관찰하며 분산 조절
  - Round Robin : 서버의 상태를 실시간으로 관찰할 수 없기 때문에 장애 발생시 대처가 미흡

 

##### 보완

- 가중치 편성 방식 (Weighted round robin, WRR)
  - 각각의 웹 서버에 가중치를 가미해서 분산 비율을 변경
  - 가중치가 큰 서버일수록 빈번하게 선택되므로 처리능력이 높은 서버는 가중치를 높게 설정
  - `좋은 성능 서버는 더욱 가중치 높여서 처리 많이 할 수 있게 해줘라.`
- 최소 연결 방식 (Least connection)
  - 접속 클라이언트 수가 가장 적은 서버를 선택
  - 로드밸런서에서 실시간으로 connection 수를 관리하거나 각 서버에서 주기적으로 알려주는 것이 필요
- 다중화 구성 방식 (Synchronous Time-Division Multiplexing)
  - 서버에 Virtual IP(가상 아이피)를 부여하고 Health Check후 이상이 감지되면 VIP를 정상 AP 서버로 인계
  - DNS Server Table 에 실시간으로 AP 서버의 상태를 확인할 수 있는 칼럼 및 함수를 추가하여 요청될 경우 서버 상태를 확인하여 우회루트를 제공하거나 에러를 전송

 