# 프로세스 동기화(Process Synchronization)

여러 개의 프로세스, 혹은 스레드가 공유된 자원, 데이터를 사용하는 경우, 스케쥴링 시스템에 의해 데이터의 일관성(Data Consistency) 를 지키지 못하는 경우가 생긴다. 이를 해결하기 위해 협력하는 프로세스 사이에서 실행 순서 규칙을 정하여 공유 자원의 일관성을 보장하는 것을 **프로세스 동기화**라고 한다.

<br/>

## 1. 경쟁 상태 (Race Condition)

**경쟁 상태**(Race Condition)는 여러 프로세스들이 동시에 데이터에 접근하는 상황에서, 어떤 순서로 데이터에 접근하느냐에 따라 결과 값이 달라질 수 있는 상황을 말한다. 

공유 데이터의 동시 접근(Concurrent access)은 **데이터의 불일치 문제**를 발생시킬 수 있다. 따라서, 경쟁 상태를을 막고 일관성을 유지하기 위해서는 협력 프로세스 간의 실행 순서를 정해주는 메커니즘인 **동기화**(Synchronization)가 필요하다. 

대표적으로 다음 세 경우에서 경쟁 상태가 발생할 수 있다. 

<br/>

### (1) 커널 모드로 수행 중 인터럽트가 발생하는 경우

![](https://i.imgur.com/1MWaexh.png)

의도된 동작은 `count++`과 `count--`가 모두 반영되어 count가 초기값을 유지하는 것이지만, 만약 Load를 한 후에 인터럽트가 발생하는 경우 인터럽트의 결과는 반영되지 않고 `count++`만 반영된다.

이는 ++커널 모드의 수행이 끝나기 전에는 인터럽트를 받지 않도록 하는 방법(disable/enable)으로 문제를 해결할 수 있다.++

<br/>

> **커널 모드**(kernel mode) : 커널 모드는 운영체제가 CPU의 제어권을 가지고 운영체제 코드를 실행하는 모드로서, 이 모드에서는 모든 종류의 명령을 다 실행할 수 있다. 시스템에 중요한 영향을 미치는 연산은 커널 모드에서만 실행 가능하다. (↔ 사용자 모드)

<br/>

### (2) 프로세스가 시스템 콜을 호출해서 커널 모드로 수행 중인데 Context switch가 발생하는 경우

두 프로세스의 주소 공간에서는 데이터를 공유하지 않지만, 시스템 콜을 수행하는 동안에는 둘 다 커널 주소 공간의 데이터를 접근한다. 따라서 커널 주소 공간에서 작업을 수행하는 도중에 CPU를 빼앗으면 경쟁 상태가 발생한다.

![](https://i.imgur.com/c9Vh5vi.png)

이는 ++커널 모드를 수행 중일 땐 CPU가 preempt 되지 않도록 하고, 커널 모드에서 유저 모드로 돌아갈 때 preempt 되도록++ 함으로써 해결할 수 있다. 

<br/>

> **시스템 콜**(system call) : 운영체제에서 프로그램이 구동될 때 많은 부분에 커널 모드가 사용된다. 시스템 콜은 이러한 커널 영역의 기능을 사용자 모드가 사용 가능하게, 즉 프로세스가 하드웨어에 직접 접근해서 필요한 기능을 사용할 수 있게 해준다.

<br/>

### (3) 멀티 프로세서에서 공유 메모리 내의 커널 데이터에 접근하는 경우

![](https://i.imgur.com/QdWFjTC.png)

어떤 CPU가 마지막으로 Count를 저장했는지에 따라 결괏값이 달라진다.

싱글 프로세서인 경우 1번에서와 같이 인터럽트 disable/enable 방법으로는 해결할 수 있지만 멀티 프로세서인 경우 인터럽트 제어로는 해결할 수 없다. 

한 번에 한 CPU만 커널에 들어갈 수 있도록 하는 방법이 있으나 이는 비효율적이다. 만약 두 프로세서가 서로 다른 데이터에 접근하여 Race condition의 가능성이 없음에도 불구하고 한 번에 한 CPU만 커널에 들어갈 수 있기 때문이다.

따라서, ++커널 내부에 있는 각 공유 데이터에 접근할 때마다 그 데이터에 대해서만 lock/unlock을 하는 방식++으로 해결할 수 있다. 

<br/>

## 2. Critical Section

**임계 구역**(Critical Section)은 코드 상에서 경쟁 상태가 발생할 수 있는 특정 부분을 말한다. 즉, 공유 데이터를 접근하는 코드 부분을 말한다.

Critical Section으로 인해 발생하는 문제들을 해결하기 위해서는 다음 조건들을 만족해야 한다. 

<br/>
 
### (1) Mutual Exclusion (상호 배제) 

 - 이미 한 프로세스가 Critical Section에서 작업 중이면 다른 모든 프로세스는 Critical Section에 진입하면 안 된다.

<br/>
 
### (2) Progress (진행)

 - Critical Section에서 작업 중인 프로세스가 없다면, Critical Section에 진입하고자 하는 프로세스가 존재하는 경우 진입할 수 있어야 한다. 

<br/>

### (3) Bounded Waiting (한정 대기)

 - 프로세스가 Critical Section에 들어가기 위해 요청한 후부터 그 요청이 허용될 때까지 다른 프로세스들이 Critical Section에 들어가는 횟수에 한계가 있어야 한다. 쉽게 말해, Critical Section에 진입하려는 프로세스가 무한정 기다려서는 안 된다. 

<br/>

## 3. Synchronization Algorithms

Critical Section Problem을 해결하기 위한 알고리즘 들을 살펴보자. 두 프로세스 Pi와 Pj가 있고, 공유하는 공통 변수가 존재한다고 가정하자.

<br/>

### (1) Algorithm 1

현재 Critical Section에 들어갈 프로세스가 어떤 프로세스인지를 한 변수로 나타내어 일치하는 프로세스만 진입하도록 하는 단순한 방식이다. 
즉, 변수를 통해 임계영역에 들어갈 프로세스를 정해준다.

```c
// int turn : Pi can enter its critical section if (turn == i)

do {
    while (turn != 0)
    critical section
    turn = 1;
    remainder section
} while (1);
```

이 방식은 **Mutual Exclusion은 만족하지만 Progress는 만족하지 못한다.**

예를 들어 Pi의 remainder section 수행 시간이 매우 길어서 여전히 remainder section에서 수행 중이고, Pj가 수행을 한번 마치고 다시 Critical Section으로 진입하려고 한다면 turn == i이기 때문에 진입할 수 없다. 

따라서 Critical Section에 아무런 프로세스가 존재하지 않게 된다. 

<br/>

### (2) Algorithm 2

다른 방식으로는, **특정 프로세스가 Critical Section에 진입할 준비가 되었다는 것을 나타내는 변수**(flag)를 두어, 다른 프로세스가 Critical Section에 진입하려고 한다면 현재 프로세스는 기다리는 방법이다. 

```c
// boolean flag[i] : Pi is ready to enter its Critical Section if (flag[i] == true)
do {
    flag[i] = true;
    while(flag[j]);
    critical section
    flag[i] = false;
    remainder section
} while(1);
```

이 방식도 마찬가지로 Mutual Exclusion은 만족하지만 Progress는 만족하지 못한다. 

두 프로세스가 flag = true를 수행하고 나면 두 프로세스 모두 무한히 Critical Section에 진입하지 못하고 기다리는 상황이 발생하게 된다.

<br/>

### (3) Algorithm 3 (Peterson's Algorithm)

Peterson's Algorithm은 이전의 알고리즘 1과 2를 합쳐놓은 개념이다. turn 변수와 flag 변수를 동시에 사용한다. 

```c
do {
    flag[i]= true;
    turm = j;
    while(flag[j] && turn == j);
    critical section
    flag[i] = false;
    remainder section
} while(1);
```

Pi 프로세스에 대해서, Pi는 flag[i] = true로 바꾸면서 Critical Section에 진입하려고 한다. 그리고 turn = j로 바꿔주면서 상대방이 들어가게 한다. 만약 상대방이 들어가고 싶고 (flag[j] == true), 현재 상대방이 Critical Section에 들어가 있으면 (turn == j) 기다린다. 그렇지 않으면 Pi가 들어간다. 

그리고나서 Critical Section을 빠져나오면 들어가고 싶지 않다고 flag[i] = false로 바꿔준다. 

이 방식으로는 **Mutual Exclusion, Progress, Bounded waiting 모두 만족**한다. 

하지만, ++Critical Section 진입을 기다리면서 계속 CPU와 메모리를 사용하는 Busy Waiting의 문제점++이 있다. 

이제, 이 복잡한 코드 구현들이 하드웨어/소프트웨어 측면에서 어떤 지원들이 제공되는지 알아보자. 

<br/>

## 4. Synchronization Hardware

하드웨어적으로 현재 상태를 확인하고 변경하는 Test & modify를 **atomic** 하게 수행할 수 있도록 지원하면 Critical Section Problem은 간단히 해결할 수 있다. 많은 시스템들은 이러한 하드웨어적인 지원을 제공한다. 

현대 기계들은 특별한 atomic hardware instruction을 제공한다. (Test_and_Set 등)

이전까지의 알고리즘들은 데이터를 읽고 쓰는 것을 하나의 명령어로 처리할 수 없기 때문에 복잡해졌다.

만약 메모리에서 데이터를 읽으면서 쓰는 것까지 하나의 명령어로 동시에 수행이 가능하다면 코드가 훨씬 간결해지고, 이는 Test_and_Set을 이용하여 아래와 같이 구현된다. 

```c
Synchronization variable;
    boolean lock = false;

do {
    while(Test_and_Set(losk));
    critical section
    lock = false;
    remainder section
}
```

하지만 이러한 하드웨어적인 명령어는 **bounded waiting 조건을 만족하지 못하는 단점**이 있다. 이 문제를 해결하기 위해서 waiting array라는 배열을 추가로 사용할 수 있다.

<br/>

## 5. Mutex Locks

Critical Section Problem을 해결하기 위한 소프트웨어 도구 중 가장 단순한 방법으로 **Mutex(MUTual EXclusion, 상호 배제) locks**이 있다. Mutex locks 에서는 available이라는 변수를 가지고 Lock의 가용 여부를 판단한다.

프로세스는 Critical Section에 들어가기 전에 Lock을 획득하고, 나올 때는 Lock을 반환해야 한다.

만약 Lock이 가용하다면 `acquire()`를 호출해서 Lock을 획득하고, 다른 프로세스가 접근하지 못하도록 Lock은 사용불가가 된다. 그리고 Lock을 반환할 때는 `release()를` 호출한다.

Lock을 걸고 푸는 동작은 하드웨어 방식처럼 atomic 하게 수행된다. 

```c
acquire(){
    while (!available); // busy wait
    available = false;
}
release() {
    available = true;
}
```
```c
do {
    acquire lock
    critical section
    release lock
} while(1);
```

이러한 방식은 앞의 Peterson's Algorithm 설명에서도 언급했듯이, **Busy Waiting의 단점**이 있다.

Critical Section에 프로세스가 존재할 때, 다른 프로세스들은 Critical Section에 `acquire()`를 계속해서 호출하며 진입하려고 시도하기 때문에 CPU를 낭비하게 된다.

이렇게 lock이 반환될 때까지 계속 확인하면서 프로세스가 기다리는 것을 다른 용어로 **Spin Lock**이라고 한다. 

<br/>

## 6. Semaphores

**세마포어**(Semaphore)는 ++Busy Waiting이 필요 없는 동기화 도구++이며, 여러 프로세스나 스레드가 Critical Section에 진입할 수 있는 Locking 메커니즘이다. 

세마포어는 카운터(Counter)를 이용하여 동시에 자원에 접근할 수 있는 프로세스를 제한한다. 

주로 S라는 정수형 변수로 나타내며, 이는 사용 가능한 자원의 개수를 의미한다.

또 세마포어 변수는 오직 두 개의 atomic 한 연산을 통해서 접근할 수 있다. 한 프로세스가 세마포어 변수를 수정할 때 다른 프로세스가 동시에 같은 세마포어 변수를 수정할 수 없다. 

```c
wait (S) {           // P(S)
    while (S <= 0);  // busy wait
    S-- ;
}

signal (S) {         // V(S)
    S++;
}
```

<br/>

세마포어에는 두 종류가 있다.

1. **Counting Semaphore** : 정수 값의 범위가 0 이상으로 제한이 없다. 주로 자원의 개수를 세는 데 사용한다.
    - 세마포어(S)는 프로세스가 임계 구역에 들어가려 할 때 (`wait()`을 호출할 때) 값이 감소하고, 임계 구역의 작업이 끝나고 lock을 반납할 때 (`signal()`을 호출할 때) 값이 증가한다. 
    - 세마포어(S)가 0이 된다면 모든 자원들이 프로세스에 의해 모두 사용중이라는 것을 의미한다.
    - 이후 자원을 사용하려면 세마포어(S)가 0보다 커지기를 기다려야 한다.

2. **Binary Semaphore** : 정수 값이 오직 0 또는 1이다. Mutex lock과 동일한 역할을 갖는다. (Mutex lock과의 차이점)


<br/>

세마포어는 다음과 같이 사용된다.

```c
// int mutex; (initially mutex = 1)

do {
    wait(mutex);
    critical section
    signal(mutex);
    reminder section
}
```

하지만, 이 방식은 Busy Waiting이 발생하므로 비효율적이다. 따라서 **Block & Wakeup 방식**을 사용한다. 

Block & Wakeup 방식은 ++Critical Section으로의 진입에 실패한 프로세스를 기다리게 하지 않고 Block 시킨 뒤 Critical Section에 자리가 나면 다시 깨워줌으로써 Busy waiting에서의 CPU 낭비 문제를 해결++해준다. 

일반적으로 Busy Waiting이 비효율적이지만, Critical Section이 매우 짧은 경우 Block & Wakeup의 오버헤드가 더 커질 수도 있다. 

Block & Wakeup 구현을 위해 세마포어를 다음과 같이 정의한다. 

```c
typedef struct
{
    int value;
    struct process *L;
} semaphore;
```

<br/>

**value는 세마포어 변수를 의미하고, L은 block 된 프로세스들이 기다리는 Queue**다. 따라서 wait와 signal 함수도 아래와 같이 구현된다. 

```c
void wait(semaphore S) {
    S.value--;
    if (S.value < 0) {
        add this process to S.L;
        block();
    }
}
```

```c
void signal(semaphore S) {
    S.value++;
    if (S.value <= 0) {
        remove a process P from S.L;
        wakeup(P);
    }
}
```

Block을 수행하면, 커널은 block을 호출한 프로세스를 suspend 시키고 해당 프로세스의 PCB를 wait queue에 넣어준다. 

Wakeup을 수행하면 block 된 프로세스 P를 깨운 다음, 이 프로세스의 PCB를 Ready Queue로 이동시킨다. 

signal 함수에서 S.value가 0 '이하'인 이유는, 이전에 S.value++를 통해 자원을 내놓았는데 아직 0 이하라는 의미가 기다리고 있는 프로세스가 존재한다는 의미이기 때문이다.

wait나 signal함수는 같은 세마포어에 대해 두 프로세스가 동시에 실행할 수 없으며, 만약 wait나 signal을 수행하는 도중에 preempt 된다면 Critical Section Problem에 빠지게 된다. 

<br/>

### Deadlocks and Starvation

두 프로세스가 서로 종료될 때까지 대기하는 프로그램을 실행한다고 생각해보자. 프로세스 A는 B가 종료될 때까지, 프로세스 B는 A가 종료될 때까지 작업을 하지 않기 때문에 프로그램은 어떤 동작도 하지 못할 것이다. 이처럼 두 프로세스가 리소스를 점유하고 놓아주지 않거나, 어떠한 프로세스도 리소스를 점유하지 못하는 상태가 되어 프로그램이 멈추는 현상을 데드락(Deadlock)이라고 한다. 운영체제도 결국 소프트웨어이기 때문에 데드락에 빠질 수 있다.

기아 상태(Starvation)란 특정 프로세스의 우선순위가 낮아서 원하는 자원을 계속 할당받지 못하는 상태이다.
기아상태는 자원 관리 문제이다. 이 문제에서 대기 중인 프로세스는 리소스가 다른 프로세스에 할당되어 있기 때문에 오랫동안 필요한 리소스를 얻지 못한다.

<br/>

## 7. Classical Problems of Synchronization

고전적인 동기화 문제로 유명한 세 문제가 있다. 

<br/>

### (1) Producer-Consumer Problem (Bounded-Buffer Problem)


- 한정 버퍼 문제(Bounded Buffer Problem)라고도 한다.
- 생산자는 물건을 생산하여 창고(버퍼)에 넣는다. 소비자는 창고에서 물건을 꺼내서 소비한다.
- 창고가 가득 차면 생산자는 물건을 넣을 수 없고, 창고가 비어 있으면 소비자는 물건을 소비할 수 없다.
- 크리티컬 섹션 : **창고** (버퍼)

<br/>

생산자 스레드들과 소비자 스레드들이 있고, 사이에 공유 버퍼가 있다고 가정하자. 

만약 둘 이상의 생산자가 비어있는 버퍼를 동시에 보고 데이터를 만들어 넣는다면 문제가 발생할 수 있다. 마찬가지로 둘 이상의 소비자가 동시에 동일한 버퍼의 데이터를 사용한다면 문제가 발생할 수 있다. 따라서, **동시에 버퍼에 접근할 수 없도록 락을 걸어줘야 한다.**

또, 버퍼가 꽉 찬 상태에서 생산자는 반드시 소비자가 버퍼의 데이터를 사용하기를 기다려야 하고, 버퍼가 빈 상태에서 소비자는 생산자가 버퍼를 채우기를 기다려야 한다.

그러므로 **락을 걸고 푸는 용도, 자원의 개수를 카운팅 하는 용도로 세마포어 변수를 사용한다.** 

<br/>

![](https://i.imgur.com/MKmI4Ce.png)

<br/>

### (2) Readers-Writers Problem
- 독자는 책(공유 데이터베이스)에 쓰여있는 글을 읽는다. 저자는 책에 글을 써서 추가한다.
- 독자가 글을 읽고 있다면, 독자는 추가적으로 글을 읽을 수 있지만, 저자는 글을 쓸 수 없다.
- 저자가 글을 쓰고 있다면, 독자는 글을 읽을 수 없으며, 저자 또한 추가적으로 글을 쓸 수 없다.
- 크리티컬 섹션 : **책** (공유 데이터베이스)

<br/>

Readers-Writers Problem은 **한 프로세스가 데이터를 수정하는 작업을 수행할 때 다른 프로세스가 접근하면 안 되고, 읽는 작업은 여러 프로세스가 동시에 수행 가능하도록 하는 문제**이다. 

만약 데이터에 대해 하나의 lock을 사용하게 되면 데이터의 일관성은 지킬 수 있으나, 여러 프로세스가 동시에 읽을 수 있음에도 불구하고 한 프로세스만 읽도록 강제하는 것이므로 비효율적이다. 

따라서 현재 수정하려고 하는 프로세스가 없다면 모든 대기 중인 Reader들의 접근을 허용하고, Writer는 대기 중인 Reader가 하나도 없는 경우 접근하도록 한다. 그리고 Writer가 접근 중이면 Reader들은 접근할 수 없도록 한다. 

<br/>

![](https://i.imgur.com/cmEtle0.png)

<br/>

만약 n개의 reader가 기다리고 있다면, 제일 처음 reader만 wrt 세마포어에 넣고 나머지 n-1개의 reader는 mutex 세마포어 큐에 넣어둠으로써 효율성을 높여준다. 

Readers-Writers Problem의 해결책은 Starvation의 가능성이 있다. 계속해서 reader가 들어오거나 writer가 들어오는 경우 한쪽이 계속 수행되지 않을 수 있다. 이는 큐에 우선순위를 부여한다거나, 일정 시간이 지나면 자동으로 write or read가 되도록 하여 해결할 수 있다. 

<br/>

### (3) Dining-Philosophers Problem


- 원형 테이블에 철학자들이 앉아있고 철학자의 수만큼 젓가락이 철학자 사이에 하나씩 놓여있다.
- 철학자들이 식사를 하기 위해서는 양쪽에 하나씩 놓여있는 젓가락을 둘 다 들어서 사용해야 한다.
- 어떤 철학자가 젓가락을 사용 중이라면, 다른 어떤 철학자는 식사를 할 수 없다.
- 크리티컬 섹션 : **젓가락**

<br/>

5명의 철학자가 원탁에 둘러앉아있고, 젓가락도 5개가 놓여있다.

![](https://i.imgur.com/aQykFgt.png)

각 철학자는 식사를 하기를 원하고, 철학자가 식사를 하기 위해서는 양쪽 젓가락을 모두 집어야 한다. 

![](https://i.imgur.com/mndl6v6.png)

위와 같이 구현하면 이웃한 두 철학자가 동시에 먹는 경우가 없다는 것이 보장된다.

하지만 이 방식은 만약 모든 철학자가 식사를 하기를 원해서 각자 왼쪽에 있는 젓가락을 들었다면, 더 이상 아무런 작업도 수행할 수 없는 Deadlock 상태에 빠져버리게 된다. 

Deadlock 상태를 방지하기 위한 여러 방법이 있는데, ++4명의 철학자만이 테이블에 동시에 앉도록 하거나++, ++젓가락을 두 개 집을 수 있을 때만 젓가락을 집도록 하는 방법++, 또는 ++짝수/홀수 철학자는 왼쪽/오른쪽 젓가락을 먼저 집도록 하는 방법++ 등이 있다. 

 ex) 각 철학자가 양쪽 젓가락을 다 들 수 있을 때만 들도록 하는 방법


V(self[i]) : 양쪽 젓가락을 들 수 있는 권한을 얻는 부분. 만약 얻지 못하면 P(self[i])에서 기다리게 된다. 양쪽 철학자가 밥을 다 먹은 경우에 기다림이 끝난다.

<br/>

![](https://i.imgur.com/o5Tr8Qt.png)

<br/>

## 8. Monitor

이전의 세마포어의 문제점은 코딩하기가 힘들고 실수하기 쉬우며, 정확성을 입증하기가 어렵다. V(mutex)와 P(mutex)의 순서에 따라 Deadlock이 생기거나 Mutual Exclusion이 깨질 수 있기 때문이다. 이러한 단점을 보완할 수 있는 구조로 Monitor가 있다. 

**Monitor**(모니터)는 ++동시 수행 중인 프로세스 사이에서 추상 데이터의 안전한 공유를 보장하기 위한 High-level 동기화 구조++이다. 공유 데이터를 접근하기 위해서는 모니터의 내부 Procedure를 통해서만 접근할 수 있도록 한다. 

세마포어와의 가장 큰 차이점은, ++모니터는 락(Lock)을 걸 필요가 없다.++ 세마포어는 프로그래머가 직접 wait와 signal을 사용하여 Race condition을 해결해야 하지만 모니터는 자체적인 기능으로 해결할 수 있게 된다. 

<br/>

![](https://i.imgur.com/cQqqbN8.png)

<br/>

위 그림은 Race condition이 발생할 수 있는 경우의 예시이다. P1이 B와 C를 사용하고 있을 때 P2가 C에 접근한다면 Race condition이 발생할 수 있다. 

우리는 이를 방지하기 위해 여러 알고리즘이나 세마포어 같은 도구들을 배웠고, 이를 사용하면 다음과 같이 작용한다. 

<br/>

![](https://i.imgur.com/2usco0l.png)

<br/>

반면, 모니터는 아래와 같이 이루어져 있다.

<br/>

![](https://i.imgur.com/ODhqwoO.png)

<br/>

모니터는 공유 데이터 구조, 공유 데이터에 대한 연산을 제공하는 프로시저(Procedure), 현재 호출된 프로시저간의 동기화를 캡슐화한 모듈(module)이다. 

프로세스가 공유 데이터를 사용하기 위해서는 반드시 모니터 내의 Procedure를 통해야 한다. 그리고 동일한 시간엔 오직 한 프로세스나 스레드만 모니터에 들어갈 수 있다. 

<br/>

![](https://i.imgur.com/Ak9Lvpk.png)

<br/>

모니터에 진입하려고 했지만 내부에 프로세스가 들어가 있어 진입하지 못한 프로세스들은 모니터 큐(Monitor Queue)에서 기다린다. 프로세스가 모니터 내에서 기다릴 수 있게 하도록 조건 변수(Condition variable)가 사용된다. 

 

조건 변수는 오직 wait와 signal 연산만으로 사용될 수 있다. 세마포어 변수와 유사하지만 변수가 어떤 값을 가지진 않고, 자신의 큐에 프로세스를 매달아서 sleep 시키거나 큐에서 프로세스를 깨우는 역할만 한다. 

<br/>

![](https://i.imgur.com/EjubCsV.png)

<br/>

## 면접 질문

### 뮤텍스와 세마포어
이 둘의 궁극적인 목표는 ‘다수의 프로세스나 스레드가 공유 자원에 동시에 접근하는 것을 제어하는 것’이다.

- **뮤텍스**: 한 스레드, 프로세스에 의해 소유될 수 있는 Key를 기반으로 한 상호배제 기법이다. 한 스레드가 임계 영역에 들어갈 때 lock을 걸어 다른 스레드가 접근하지 못하도록 하고, 임계 영역에서 나올 때 unlock한다.
- **세마포어**: 현재 공유 자원에 접근할 수 있는 스레드, 프로세스의 수를 나타내는 값을 두는 상호 배제 기법이다. 그 값만큼 동시에 스레드가 해당 공유 자원에 접근할 수 있다.

<br/>

### 뮤텍스와 세마포어의 차이?

- 가장 큰 차이는 동기화 대상의 갯수이다. 뮤텍스는 동기화 대상이 하나뿐이고, 세마포어는 동기화 대상이 하나 이상일 때.
- 뮤텍스는 Locking 메커니즘으로 락을 걸은 쓰레드만이 임계 영역을 나갈때 락을 해제할 수 있다. 하지만 세마포어는 Signaling 메커니즘으로 락을 걸지 않은 쓰레드도 signal을 사용해 락을 해제할 수 있다. 세마포어의 카운트를 1로 설정하면 뮤텍스처럼 활용할 수 있다.
