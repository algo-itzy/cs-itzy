## 정의

- 데이터를 **데이터그램** 단위로 처리하는 프로토콜
- 순차 전송 X, 흐름 제어 X, 혼잡 제어 X
    - TCP보다 신뢰성은 떨어지지만 전송속도가 빠름
- 비연결성(Connectionless) - 3 way-handshake X
- 비교적 데이터 신뢰성이 중요하지 않을 때 사용 (ex: 스트리밍)

## UDP Header

![image](https://user-images.githubusercontent.com/43740455/137590910-22613ebd-b5c3-4789-84f0-cc7fe82234af.png)

- Source port : 시작 포트
- Destination port : 도착지 포트
- Length : 헤더와 데이터를 합한 총 길이
- Checksum : 오류 검출 비트

### DNS는 왜 TCP가 아닌 UDP를 사용할까?

- TCP를 사용하면 Protocol overhead가 커짐
- DNS request 크기는 UDP 세그먼트에 들어갈 정도로 작음
- UDP의 비신뢰성을 DNS의 Application Layer에서 보완할 수 있음 (imeout, resend..)

### 결론

- 특성에 맞춰 TCP, UDP 적절한 프로토콜을 사용할 수 있어야 한다
- TCP, UDP 헤더를 파악하고 성능 개선에 이용할 수 있어야 한다
