# TCP/UDP

> TCP / UCP의 차이를 말해주세요
>
> 3 handshake, 4 handshake에 대해 설명해주세요

* 군사적 목적 : 핵전쟁이 나도 정상동작하는 네트워크.
* 회선교환(Sircuit Switching)은 중계소가 망가지면 통신이 끊김
* 때문에 packet switching 방식이 대두됨. 회선 교환과 다르게 경로가 정해져 있지 않고 막히면 우회함.
* 안정성이 목적이므로 네트워크 환경이 떨어짐. 중간에 유실되거나 늦게 전달됨
* 이런 문제점을 해결 한 규약이 TCP

> Q: 전송 중간에 패킷이 쥐도새도 모르게 사라지거나 훼손되면 어떡해요?
> A: **그럼 그 패킷만 다시 보내라고 해!(ARQ)**
>
> Q: 송신 측이 패킷을 쪼갠 순서를 알아야 수신 측이 재조립할 수 있겠는데요?
> A: **그럼 순서번호를 패킷이랑 같이 보내!(시퀀스 번호)**
>
> Q: 수신 측이 처리할 수 있는 속도보다 송신 측이 패킷을 빠르게 보내버리면 어떡하죠?
> A: **그럼 수신 측이 처리할 수 있는 양을 송신 측에 알려주고 그 만큼만 보내라고 해! (슬라이딩 윈도우)**

## Transport Layer

### End Point 간 신뢰성있는 데이터 전송을 담당하는 계층

* EndPoint간 통신 서비스 제공, 패킷 검사, 재전송 요구 제어
* 신뢰성 : 데이터를 순차적, 안정적인 전달 - TCP는 확인 메시지를 통해 신뢰성을 올림.
* 전송 : 포트 번호에 해당하는 프로세스에 데이터를 전송



---



### 전송계층이 없다면?

1. 데이터의 순차 전송 원활x
   * 송진자가 전달하고자했던 데이터가 안가게됨(1,2,3 -> 2,1,3)으로 바뀜.
2. Flow (흐름 문제)
   * 원인 : 송수신자 간의 데이터 처리 속도 차이 // 수신자가 처리할수있는 데이터량을 초과
3. Congestion (혼잡 문제)
   * 원인 : 네트워크의 데이터 처리 속도 (ex. 라우터) // Network 가 혼잡해서 데이터가 안오고 통신이안됨.



#### 결과 : 데이터의 손실 발생.



---



## TCP(Transmission Control Protocol)

* 전화 거는 것 처럼 상대와 연결을 설정하고 통신을 시작.

* 신뢰성있는 데이터 통신을 가능하게 해주는 프로토콜

* 특징 : Connection 연결(3 way-handshake) - 양방향 통신

* 데이터 순차 전송을 보장

* Flow control (흐름제어)

* Congestion control (혼잡 제어)

* Error Detection (오류 감지)

* 전이중 (Full-Duplex) : 전송이 양방향으로 동시에 일어날 수 있다.

* 점대점 (Point to Point) : 각 연결이 정확히 2개의 종단점을 가지고 있다.

  

---



### -세그먼트(Segment) - TCP 프로토콜의 PDU (Protocol Data Unit)

#### 	데이터를 잘라 TCP Header를 붙힘. => 패키징한게 Segment

* 발신 포트, 수신포트, 순서 번호, 오류 검출 코드
* 오류검출 코드(체크섬, 프라임 체크 시퀀스)는 전송 간 손상이 있는지 확인 하는 것. 세그먼트의 오류검출코드와 목적지에서 산출한 코드가 다르다면 전송과정에서 오류가 생긴것이므로 해당 세그먼트를 폐기하고 복구 절차를 밟는다.



![img](https://blog.kakaocdn.net/dn/bCenX6/btqZP2MEqAq/PDyksDOf5nBnuY1et5Y2v0/img.png)

---





### TCP Header

![img](https://t1.daumcdn.net/cfile/tistory/215E874552342E7319)

* 순차전송 ( Sequence Number) - 32 bits로 4294967296까지는 순서 보장. 그이상은 다시 0부터 시작.
* 승인번호 (Acknowledgement Number) - 수신자가 예상하는 다음 시퀀스 번호. handshake는 받은 시퀀스 + 1지만 실 데이터 전송에서는 상대방이 보낸 시퀀스 번호 + 자신이 받은 데이터의 bytes ( 보낸 데이터의 길이 )로 만들어 낸다.

[![ack](https://evan-moon.github.io/static/4c5dc85683ae837d23500d773f219316/6af66/ack.png)](https://evan-moon.github.io/static/4c5dc85683ae837d23500d773f219316/05244/ack.png)

* Data offset - 세그먼트 중 header가 아닌 실 데이터의 시작 위치. 최소값은 20bytes.

* Control Flags 필드(6bit)

  : 6개의 서로 다른 제어 비트 또는 플래그를 나타낸다. 동시에 여러 개의 비트가 1로 설정될 수 있다. 현 세그먼트의 속성을 알려줌.

  

  \- **CWR : Congestion Window Reduced)** – 혼잡 윈도우 크기 감소

  \- **ECN : Explicit Congestion Notification)** – 혼잡을 알림

  \- **URG(Urgent)** : Urgent Pointer 필드가 가리키는 세그먼트 번호까지 긴급 데이터를 포함되어 있다는 것을 뜻한다.

   이 플래그가 설정되지 않았다면 Uregent Pointer 필드는 무시되어야 한다.

  \- **ACK(Acknowledgment)** : 확인 응답 메시지

  \- **PSH(Push)** : 데이터를 포함한다는 것을 뜻한다.

  \- **RST(Reset)** : 수신 거부를 하고자 할때 사용

  \- **SYN(Synchronize)** : 가상 회선이 처음 개설될 때 두 시스템의 TCP 소프트웨어는 의미 있는 확인 메시지를 전송하기 위해   일련 번호를서로 동기화해야 한다.

  \- **FIN(Finish)** : 작업이 끝나고 가상 회선을 종결하고자 할 때 사용

  

  ---

  

  ### 3 way handshake

  

![img](https://blog.kakaocdn.net/dn/bqWzBI/btqZV6NmcLk/KdR7yXGbKwWoH6b15jhLP0/img.png)

1. SYN(Synchronize Sequence Number) 비트를 1로 설정해 패킷 송신 ( 상대에 통신 하고싶다고 메세지 보냄 )

2. SYN. ACK(Acknowledgement) 비트를 1로 설정해 패킷 송신 ( 상대의 응답 + 나도 통신 준비가 되었다)

3.  ACK 비트를 1로 설정해 패킷 송신. ( 2번의 메세지에 응답을 보내줌. )

   => 1은 ok의 신호.(연결하자!)
   
4. 이를 통해 상대와 통신이 연결 되어 있음을 보장하게 된다. (Crircuit Switching과 유사하지만 여기선 단순 연결 여부만 보장)

<br>

---

<br>

### TCP통신방식

![img](https://blog.kakaocdn.net/dn/cxeJc9/btqZV5gCL1d/wbdeS5Stgr8nQ9mSfWdpEK/img.png)

1. client가 패킷 송신
2. server가 ACK송신
3. ACK를 수신하지 못하면 재전송(서버 응답 올때까지 )

=> 신뢰성 확보



---



#### 4 way -handchake(Connection close) 연결해제

1. 데이터를 전부 송신한 A가 FIN 비트를 송신(데이터 다 보냈다.)
2. B가 ACK 비트를 A로 송신(알았다.)
3. B에서 A로 남은 패킷 송신 ( 패킷 보낸다 기다려라 일정시간 대기)
4. B가 FIN 비트를 A로 송신(패킷 다 보냈다)
5. A가 ACK 비트를 B로 송신(알았다)
6. CLOSED (연결 해제됨)



---



### TCP의 문제점

**TCP의 문제점은 없을까?**



* 전송의 신뢰성은 보장하지만 다음과 같은 문제점이 있다. ( 현재 HTTPS는 TCP + TLS)

- 매번 connection을 연결해서 시간적 손실이 발생한다. (3-way-handshaking) - 최소 100ms 소요.
- 패킷을 조금만 손실해도 재전송한다.



---



## UDP( **User Datagram Protocol**)



* TCP보다 신뢰성이 떨어지지만 **전송 속도가 TCP에 비해 빠른** Transport Layer의 protocol

  (순차전송x, 흐름제어x, 혼잡제어x)

- 비연결형 프로토콜 (connectionless). 연결 요청-해제 과정이 필요 없다.(연결 과정 x일방적으로 보냄.), 각 패킷은 독립적 
- 에러 검출 기능만 있다.
- 비교적 데이터 신뢰성이 중요하지 않을 때 사용한다. (ex. 영상 스트리밍)

 ![img](https://blog.kakaocdn.net/dn/qj356/btqZTT815ff/t9bsCUehRXAVZbEQbr19DK/img.png)

* 7 Layer로 부터 받은 메세지를 세그먼트단위로 쪼개지 않고 UDP 헤더만 붙여 데이터그램단위로 처리. 단위(datagram) - 유저 데이터그램. 쪼개려면 직접 7계층에서 쪼개야함.

### **UDP Header**

[![image](https://user-images.githubusercontent.com/43740455/137590910-22613ebd-b5c3-4789-84f0-cc7fe82234af.png)](https://user-images.githubusercontent.com/43740455/137590910-22613ebd-b5c3-4789-84f0-cc7fe82234af.png)

- 출발지의 포트번호
- 목적지의 포트번호
- checksum: 오류 검출용 비트



### UDP의 데이터 전송방식

### ![img](https://blog.kakaocdn.net/dn/q0hsX/btqZYlXx6E9/gFN23XdAnKLhY4pATjCvk1/img.png)

- **Connectionless**: Client는 패킷을 확인 안하고 무조건 송신. Server는 소캣 무조건 열어두고 있음



### 특징

- 순차전송 ❌
- Flow Control ❌
- Congestion Control ❌
- Error detection: UDP헤더의 CheckSum 필드를 통해 최소한의 오류만 검출한다.
- 비교적 데이터의 신뢰성이 중요하지 않을 때 사용(ex. 영상 스트리밍)



> 왜 DNS는 UDP를 사용할까?
>
> - Request의 양이 작아서 UDP Request에 담을 수 있다.
>
> - TCP는 데이터를 송신할 때까지 세션 확립을 위한 처리를 하고, 송신한 데이터가 수신되었는지 점검하는 과정이 필요하지만 UDP는 3-way handshaking으로 연결을 유지할 필요가 없어 오버헤드가 작아진다.
>
> - Request에 대한 손실은 Application Layer에서 제어가 가능하다.
>
> - 하지만 Zone Transfer를 사용하거나 데이터가 512 bytes를 넘는 경우, 응답을 못 받은 경우는 TCP를 사용한다.
>
>   Zone Transfer : DNS 서버 간의 요청을 주고 받을 때 사용하는 transfer



> #### QUIC(Quck UDP Internet Connections)
>
> TCP의 성능을 개선하고자 UDP를 채택한 기술이다. 또한, UDP는 데이터 전송에만 초점을 맞추고 설계되어 헤더에 아무것도 없기 때문에 커스터마이징이 용이하다. 따라서 개발자의 구현에 따라 신뢰성을 높일 수 있고, 기능 확장이 쉽다.
>
> - 전달 속도의 개선
> - 클라이언트와 서버의 연결수 최소화
> - 대역폭을 예상하여 패킷 혼잡(congestion)을 피함
>
> reference: https://evan-moon.github.io/2019/10/08/what-is-http3/

---



## TCP/UDP 비교

![img](https://blog.kakaocdn.net/dn/sv1cS/btqZTU1bcxs/ui7egay5SRnhTUOKjVy2v1/img.png)



---



## 추가(TCP/IP 5계층 모델)

![img](https://blog.kakaocdn.net/dn/xe8Bq/btqZQHnXcry/zSWHllQe87UjrpAxGF9UP1/img.png)

![img](https://t1.daumcdn.net/cfile/tistory/995EFF355B74179035)



## Reference

- https://evan-moon.github.io/2019/11/10/header-of-tcp/
- https://github.com/GumiMobile/CS-Study/tree/main/Network#tcp%EC%99%80-udp%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90

