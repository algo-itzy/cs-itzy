## 교착 상태 (Dead Lock)

> 프로세스, 스레드가 결코 일어날 수 없는 특정 이벤트를 기다리는 상태

![image](https://user-images.githubusercontent.com/43740455/145609060-05b6b1e5-8ba0-48fb-8142-dd33c06aa1e0.png)



### 필요 조건

1. 상호 배제 조건 (Mutual Exlusion Condition)
   - 한 번에 한 개 프로세스만 공유 자원 사용 가능
2. 점유와 대기 조건 (Hold-and-Wait Condition)
   - 프로세스 : 최소 하나의 자원 점유 + 다른 프로세스에 할당되어 사용되고 있는 자원을 추가로 점유하기 위해 대기
3. 비선점 조건 (Nopreemption Condition)
   - 다른 프로세스에 할당된 자원 : 사용 종료까지 뺏을 수 없음
4. 순환 대기 조건 (Circular-Wait Condition)
   - 공유 자원, 자원을 사용하기 위해 대기하는 프로세스 : 원형으로 구성 
   - 자신에게 할당된 자원을 점유하면서 전후 프로세스의 자원 요구



### 해결책

![image](https://user-images.githubusercontent.com/43740455/145609085-697d264b-dc63-4f0d-9f5b-d72ada38616a.png)

#### 1. 예방

- 사전에 시스템 제어 : 필요 조건 중 하나를 제거 -> 자원의 낭비 심함

  |        구분        |                설명                |                단점                 |
  | :----------------: | :--------------------------------: | :---------------------------------: |
  |  상호배제 조건 X   |  공유할 수 없는 자원 사용 시 성립  |                                     |
  | 점유와 대기 조건 X |   자원 요청 시 다른 자원 요청 X    |          자원 공유 불가능           |
  |   비선점 조건 X    |    자원 할당 실패 시 자원 반납     | Starvation 발생, 자원 안전선점 불가 |
  |  순환 대기 조건 X  | 모든 프로세스에 자원할당 순서 부여 |   새로운 자원 추가 시 재구성 필요   |

#### 2. 회피

> 자원을 할당한 후에도 시스템이 항상 Safe state에 있을 수 있도록 할당을 허용

- `Safe State` : 프로세스가 요청하는 모든 자원을, 데드락을 발생시키지 않으면서도 차례로 모두에게 할당해 줄 수 있는 상태

- `Safe Sequence` : 특정한 순서로 프로세스들에게 자원을 할당, 실행 및 종료 등의 작업을 할 때 데드락이 발생하지 않는 순서

- 자원 할당 알고리즘

  ![image](https://user-images.githubusercontent.com/43740455/145609106-59d64d63-88d3-4268-8dd7-c38cd1aa89f8.png)

  - 종류별로 자원 1개 -> 한 개씩 요청 가능
  - 자원 요청 시 예약 선 : 요청 간선으로 전환

- `Banker's Algorithm` 

  - 미리 결정된 모든 자원들의 최대 가능한 할당량으로 시뮬레이션 해서 Safe state에 들 수 있는지 여부를 검사
    - OS : 자원 상태 감시, 필요 자원 수 제시 관리 역할
    - Available : 사용 가능 자원의 수
    - Max : 각 프로세스 최대 요구
    - Need : 프로세스 마치기 위한 남은 요구
    - Allocation : 각 프로세스에 할당된 자원 수
  - 제약 조건 : 미리 최대 자원 요구량 알아야 함, 할당할 수 있는 자원 수 일정
  - 단점 : 자원 이용도 하락

#### 3. 탐지 및 복구

- 교착상태 자주 발생하는 시스템에서 일반적으로 사용

- 탐지

  - 교착상태 발견 알고리즘, 자원 할당 그래프 사용

    ![image](https://user-images.githubusercontent.com/43740455/145609127-0157bb39-6936-4c3b-be22-754002f5575f.png)

- 복구

  - 프로세스 종료

    |   방법    |                       세부 기법                       |           장단점           |
    | :-------: | :---------------------------------------------------: | :------------------------: |
    | 전체 종료 |             교착상태인 프로세스 모두 종료             | 구현 간단/종료에 따른 cost |
    | 부분 종료 | 종료 시 cost가 낮은 process : 우선순위, 처리시간 기준 |   종료 프로세스 수 작음    |

  - 자원 선점법

    |      절차       |                          세부 기법                           |
    | :-------------: | :----------------------------------------------------------: |
    |   희생자 선택   |                      최소 피해 프로세스                      |
    | 롤백 (Rollback) |                       이전 상태로 롤백                       |
    |    기아방지     | 계속 선점되어 기아 상태 방지<br />우선순위 -> 선점 될때마다 우선순위 높이기 |

    

#### 4. 무시

- 교착생태가 드물게 발생하는 시스템에서 일반적으로 사용



