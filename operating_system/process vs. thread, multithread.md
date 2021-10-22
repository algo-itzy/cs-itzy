## **프로세스와 스레드의 차이**

### **프로세스(Process)**

- 실행중인 프로그램
- 디스크에 저장된 수동적 파일(실행 파일)이 **메모리에 적재**될 때, 비로소 **프로세스**가 됨!

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/6ccc318c-de92-4462-aad6-b8408e428cb9/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211022T025644Z&X-Amz-Expires=86400&X-Amz-Signature=89efb9a7f2b2b83575ac54782d05925335a283af9918cf92f4dfd42db4dc6d14&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

프로그램

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0b4295e5-239b-4d83-87ea-334219d11ccf/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211022T025708Z&X-Amz-Expires=86400&X-Amz-Signature=200ae6a4efbc4665deda09f828fec0bf7cb2a7d725b4de642e6330925d359ab5&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

프로세스

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3a28cdd3-4435-444c-a780-e96fe0bb49dc/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211022T025727Z&X-Amz-Expires=86400&X-Amz-Signature=bb7812d09e8878dc0030c6985789d87c4bb6353fcea42fb6854f2f95b43091ba&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- **텍스트 영역**: 실행 코드를 저장한다. 텍스트 영역은 프로세스 실행 중 크기가 변하지 않는 고정 영역에 속한다.
- **데이터 영역** : 프로그램에서 정의한 전역 변수를 저장한다. 전역 변수는 프로그램을 작성할 때 크기가 고정되므로 고정영역에 할당된다.
- **힙(heap)**: 프로그램 실행 중에 동적으로 메모리를 요청하는 경우에 할당되는 영역으로, 빈 영역→할당→할당 해제 처럼 상태가 변하는 가변 영역이다.
- **스택(stack)**: 프로그램에서 정의한 지역 변수를 저장하는 메모리 영역으로, 지역 변수를 정의한 부분에서 할당해 사용한다.
    - 예) 함수 매개변수, 반환주소, 지역변수
- **빈 공간**: 스택이나 힙과 같이 가변적인 메모리 할당을 위해 유지하고 있는 빈 메모리 영역이다. 프로세스에 할당된 빈 메모리 영역이 모두 소진되면 메모리 부족으로 프로그램의 실행이 중단될 수도 있다.

### 프로세스 상태 별 설명

|상태|설명|
|:--:|:--:|
|새로운 (New)|프로세스가 생성 중|
|실행 (Running)|명령어들이 실행 중|
|대기 (Waiting)|프로세스가 어떤 사건(입출력 완료 또는 신호의 수신 같은)이 일어나기를 기다림|
|준비 완료 (Ready)|	프로세스가 처리기에 할당되기를 기다림|
|종료 (Terminated)|프로세스 실행이 종료|

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/10516c4b-a443-4650-bff3-ba691fb3da9d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211022T025853Z&X-Amz-Expires=86400&X-Amz-Signature=2d2da2a4650424c4095c69fb757b4a71dda886d0e94e56a6958ae6ce687e64c6&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

### **프로세스 제어 블록(Process Control Block, PCB)**

- 각 프로세스는 운영체제에서 프로세스 제어 블록 (Process Control Block)에 의해 표현
- PCB 는 특정 **프로세스에 대한 중요한 정보를 저장** 하고 있는 운영체제의 자료구조
- 운영체제는 프로세스를 관리하기 위해 **프로세스의 생성과 동시에 고유한 PCB 를 생성**
- 프로세스는 CPU 를 할당받아 작업을 처리하다가도 프로세스 전환이 발생하면 진행하던 작업을 저장하고 CPU 를 반환해야 하는데, 이때 작업의 진행 상황을 모두 PCB 에 저장하게 된다.
- 그리고 다시 CPU 를 할당받게 되면 PCB 에 저장되어있던 내용을 불러와 이전에 종료됐던 시점부터 다시 작업을

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/636f7127-2aa7-46d6-a50e-d1e47ff3c3b9/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211022T025903Z&X-Amz-Expires=86400&X-Amz-Signature=49de8eecb44b571959dd4cec11d2488d1ec79d0c12816ef41573834f339a635c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

*PCB 에 저장되는 정보*

- 프로세스 식별자(Process ID, PID) : 프로세스 식별번호
- 프로세스 상태 : new, ready, running, waiting, terminated 등의 상태를 저장
- 프로그램 카운터 : 프로세스가 다음에 실행할 명령어의 주소
- CPU 레지스터 : 누난기, 인덱스 레지스터, 스택 레지스터, 범용 레지스터, 상태 코드 정보 포함
- CPU 스케쥴링 정보 : 프로세스의 우선순위, 스케줄 큐에 대한 포인터 등
- 메모리 관리 정보 : 기준 레지스터와 limit 레지스터의 값, 페이지 테이블 또는 세그먼트 테이블 등과 같은 정보를 포함
- 입출력 상태 정보 : 프로세스에 할당된 입출력 장치들과 열린 파일 목록
- 어카운팅 정보 : 사용된 CPU 시간, 시간제한, 계정번호 등

### **스레드(Thread)**

- CPU 활용의 기본 단위
- 프로세스 내에서 프로그램 명령을 실행하는 기본 단위
- 스레드ID, 프로그램 카운터, 레지스터 집합, 스택으로 구성
- 같은 프로세스에 속한 다른 스레드와 코드, 데이터 섹션, open files 등의 운영체제 자원 공유

### 단일 스레드 (Single Thread)

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/700b2f7b-36ef-49ff-8889-cc4f9ac42795/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211022T025915Z&X-Amz-Expires=86400&X-Amz-Signature=a482608685cf65f82fafda26389db52e4d4c1f40f13921e705875c9b3a404034&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- 1개의 레지스터, 1개의 스택

[장점] 

- 자원을 공용화 하지 않으므로 공용 자원 접근 통제 불필요
- Context Switch 작업 불필요

[단점]

- 다수개의 CPU 활용이 불가

### 멀티 스레드 (Multi Thread)

하나의 프로세스를 다수의 실행 단위로 구분하여 자원을 공유하고 자원의 생성과 관리의 중복성을 최소화하여 수행 능력을 향상

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/186fac64-71b6-4bc6-a68a-bbf0437bcc68/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211022T025925Z&X-Amz-Expires=86400&X-Amz-Signature=800ccafacb88a5c5730ec8902ad5bc25b84fdbff33447ac00d0d86ad12ac5b17&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

- 프로그램을 다수의 실행 단위로 나누어 실행
- 각각의 스레드가 고유의 레지스터와 스택으로 표현

[장점]

- 대응적 측면 : 일부가 차단되거나 장시간 작업 수행시에도 계속해서 실행 가능
- 자원 공유성 : 프로세스의 자원과 상태를 공유. 효율적인 운영
- 경제성 : 자신이 속한 프로세스의 자원을 공유하기 때문에, 스레드를 만들고 context switch를 진행하는 것이 더 경제적
- 확장성 : 다수 개의 코어에서 병렬로 프로세스 실행

### **스택을 스레드마다 독립적으로 할당하는 이유**

스택은 함수 호출 시 전달되는 인자, 되돌아갈 주소값 및 함수 내에서 선언하는 변수 등을 저장하기 위해 사용되는 메모리 공간이므로 스택 메모리 공간이 독립적이라는 것은 독립적인 함수 호출이 가능하다는 것이고 이는 독립적인 실행 흐름이 추가되는 것이다. 따라서 스레드의 정의에 따라 독립적인 실행 흐름을 추가하기 위한 최소 조건으로 독립된 스택을 할당한다.

### **PC Register 를 스레드마다 독립적으로 할당하는 이유**

PC 값은 스레드가 명령어의 어디까지 수행하였는지를 나타나게 된다. 스레드는 CPU 를 할당받았다가 스케줄러에 의해 다시 선점당한다. 그렇기 때문에 명령어가 연속적으로 수행되지 못하고 어느 부분까지 수행했는지 기억할 필요가 있다. 따라서 PC 레지스터를 독립적으로 할당한다.
