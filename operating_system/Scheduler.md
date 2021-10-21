# Scheduler

### **State transition diagram**



![2](C:\Users\ksk\Desktop\새 폴더 (5)\2.svg)

- new: 프로세스가 만들어짐

- ready: 메모리에 올라옴, 수행가능한 상태, 최근에는 메모리가 충분히 크기 때문에 메모리에 로딩되지 않는 것이 없다고 한다. (즉, new와 ready가 합쳐져있다고 볼 수 있다.)

- running: 스케쥴러가 ready에 있는 프로세스들 중 하나를 골라 running 함(scheduler dispatch)

- waiting: running 도중 다른 프로세스를 돌리기 위해 waiting으로 옮김, 끝났다고 알려주면(interrupt) ready 상태로 바꿈

- Terminated: 프로세스 끝남

  

### **Process Scheduling**

1. CPU utilization을 높여준다. (CPU를 최대로 사용하기) - **multiprogramming, 동시에 여러 개의 프로세스를 메모리에 올려 놓고 cpu가 놀지 않게 돌려주기**
2. Time-sharing : cpu 할당 switch (사용자가 각 프로그램이 실행되는 동안 서로 상호작용할 수 있도록 만드는 것) - **multitasking, 한 프로세스가 돌다가 다른 프로세스가 돌도록 cpu 할당을 switch**

메모리에 여러 개의 process가 올라와 있는 환경에서 어떤 process를 돌릴지

I/O-bound process와 CPU-bound process를 적당히 섞는 것

CPU에서의 프로그램을 실행을 위해 사용 가능한 프로세스를 선택하며, 어떤 프로세스를 프로세서에 할당할 것인가 결정하는 일을 프로세스 스케줄링이라고 한다.



### **Degree of multiprogramming**

메모리에 존재하는 현재 process의 수 → 많을수록 좋긴 하지만 지나친 수는 좋지 않음

### **Process 종류**

1. I/O-bound process : computation보다 I/O를 더 많이 하는 프로세스 Ex) 디스크 읽기, 네트워크를 이용해 데이터 보내기

2. CPU-bound process : CPU 사용이 I/O 사용보다 많은 프로세스

   

### Scheduling Queues

- Job queue 메인 메모리가 이미 찼을 때 대기하는 장소 요즘은 많이 없고, batch system이 예시
- **Ready queue** 메인 메모리에 올라올 때 레디 큐에 있게 됨 CPU에게 할당되는 것을 기다리는 장소
- Device queues (I/O wait queue)

즉, 이런 큐들을 옮겨가게 하는 것이 스케쥴링 (PCB를 큐에 넣음)



### Schedulers

1. Job scheduler (Long-term 스케줄러) 레디 큐에 프로세스를 옮기는 것은 잡 스케줄러

   

2. **CPU scheduler (Short-term 스케줄러)** 프로세스를 프로세서에 할당하는 것은 CPU 스케줄러 몇 milliseconds씩 수행(자주 수행)

   

3. Medium-term scheduling 레디 큐에 있는 프로세스보다 다른 프로세스 먼저 돌려야 할 때 **swapping** 하는 것,

   

- 참고) CPU scheduler + swapping 합쳐서 CPU scheduler라고 부르기도 함.

