# 3 way handshake & 4 way handshake



## TCP(Transmission Control Protocol)란

TCP는 네트워크 계층 중 **전송 계층에서 사용하는 프로토콜**로서, 장치들 사이에 논리적인 접속을 성립(establish)하기 위해 연결을 설정하여 신뢰성을 보장하는 **연결형 서비스**이다.



<br/>



## TCP와 UDP의 차이점

TCP/IP에는 TCP와 UDP가 존재하는데 TCP는 신뢰성이 있는 연결을 지향하며, UDP는 빠른 전송을 지향하는 것에서 차이점이 있다.

즉, TCP는 내가 보낸 데이터가 확실히 상대방에게 전달이 되었는지 포커를 맺고, UDP는 일방적으로 전송을 한다. UDP의 한 예로 스트리밍 방송이 있는데, 방송을 하다가 중간에 신호가 끊어져도 다음으로 그냥 이어서 방송을 하는 것처럼 UDP는 일방적인 데이터 전송을 하는 반면, TCP는 데이터 하나라도 놓치지 않고 완벽히 보내는 것이 목표이기 때문에 방향성이 완전히 다르다.

UDP는 알고리즘이 간단한 반면, TCP는 연결부터 끊는 과정까지 상대적으로 복잡하다.



<br/>



## **3 way handshake - 연결 성립**

TCP는 정확한 전송을 보장해야 한다. 따라서 통신하기에 앞서, 논리적인 접속을 성립하기 위해 3 way handshake 과정을 진행한다.

![https://media.geeksforgeeks.org/wp-content/uploads/TCP-connection-1.png](https://media.geeksforgeeks.org/wp-content/uploads/TCP-connection-1.png)

1. 접속 요청 프로세스가 연결요청 메시지 전송(SYN)
2. 서버가 SYN(x)을 받고, 클라이언트로 받았다는 신호인 ACK와 SYN 패킷을 보냄 (SYN + ACK)
3. 마지막으로 접속 요청 프로세스가 수락 확인을 보내 연결을 맺음(ACK)

- **SYN(Synchronization)** : 연결요청, 세션을 설정하는데 사용되며 초기에 시퀀스 번호를 보냄
- **ACK(Acknowledgement)** : 보낸 시퀀스 번호에 TCP 계층에서의 길이 또는 양을 더한 것과 같은 값을 ACK에 포함하여 전송



<br/>



### 굳이 숫자를 왜 보낼까?

- 서버가 클라이언트의 요청을 제대로 받았는지, 클라이언트가 응답을 제대로 받았는지 확인하기 위함
- x+1이 오지 않고 다른 숫자가 오면 연결이 성립하지 않았음을 확인



<br/>



### SYN Flooding

3-Way Handshake에서 Server는 처음 **클라이언트의 요청**을 받고, 그 작업을 수행하기 위해 **일정 부분의 메모리를 할당**

그런데 수많은 클라이언트가 동시에 처음 SYN 메세지만 보내고, 서버는 SYN+ACK를 보냈는데 마지막 ACK를 하지 않으면? 서버는 클라이언트들의 응답을 계속 기다리며 메모리를 할당하다가 메모리가 바닥나 다운될 수 있다.

이는 흔히 디도스라고 많이 부르는 **분산서비스거부공격(DDoS : Distributed Denial of Service)의 일종**



<br/>



### SYN Flooding 방어책

1. 백로그 큐 늘리기
   - 메모리를 더 확충시켜 대기 수용 인원을 늘리기 (근본적인 해결책은 아님)
2. Anti-DDos
   - 특정 IP에서 갑자기 수십개의 SYN 메세지가 오면 접속을 차단 (임계치 기반 방어)
3. SYN Cookie 사용
   - 서버와 사용자 간에 방화벽을 하나 두고, 방화벽이 정상적인 사용자인지 먼저 검사
   - 사용자가 SYN을 보내면, 방화벽이 먼저 syn_cookie가 포함된 SYN + ACK 전송. 이에 대해 적절한 답변이 왔을 때만 서버와의 연결을 수립
   - 사용자는 결과적으로 3 Way Handshake 2회 수행
4. SYN Proxy 사용하기
   - SYN Cookie 와 비슷하지만 더 간소한 버전
   - 방화벽에서 정상적인 사용자임이 판단되면 서버에 재현
   - 사용자는 결과적으로 3 Way Handshake 1회 수행



<br/>



## **4 way handshake - 연결 해제**

연결 성립 후, 모든 통신이 끝났다면 해제해야 한다.

![img](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f4ebef93-e05c-4c9c-8c3f-078820b26d0c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211014T161851Z&X-Amz-Expires=86400&X-Amz-Signature=69fb476c192fafdce582a8f4fd8faa183614cb48709819879fcecee6c4da901c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

1. 클라이언트는 서버에게 연결을 종료한다는 FIN 플래그를 보낸다.
2. 서버는 FIN을 받고, 확인했다는 ACK를 클라이언트에게 보낸다. (이때 모든 데이터를 보내기 위해 CLOSE_WAIT 상태가 된다)
3. 데이터를 모두 보냈다면, 연결이 종료되었다는 FIN 플래그를 클라이언트에게 보낸다.
4. 클라이언트는 FIN을 받고, 확인했다는 ACK를 서버에게 보낸다. (아직 서버로부터 받지 못한 데이터가 있을 수 있으므로 TIME_WAIT을 통해 기다린다.)

- 서버는 ACK를 받은 이후 소켓을 닫는다 (Closed)
- TIME_WAIT 시간이 끝나면 클라이언트도 닫는다 (Closed)

이렇게 4번의 통신이 완료되면 연결이 해제된다.

> **FIN(Finish)** : 세션을 종료시키는데 사용되며 더 이상 보낸 데이터가 없음을 표시