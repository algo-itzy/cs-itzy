# Memory management

## Swapping

![image](https://user-images.githubusercontent.com/43740455/138339332-791b3124-1c01-4352-8967-0ac13c635974.png)

- 주기억장치에 적재된 끝난 프로세스를 보조기억장치의 다른 프로세스와 교체하는 기법
- 큰 리소스가 필요하기 때문에 **메모리 공간이 부족할 때**만 사용

## Fragmentation(단편화)

### 외부 단편화

- 반복된 프로세스 작업으로 인해 물리 메모리 곳곳에 잉여 공간이 생기는 현상

![image](https://user-images.githubusercontent.com/43740455/138339346-30f00ec1-cda2-420b-9c11-6d9913f0d728.png)

### 내부 단편화

- 메모리 내의 프로세스 할당 과정에서 잉여 공간이 생기는 현상

![image](https://user-images.githubusercontent.com/43740455/138339368-55b23e62-f242-48c2-893d-c258351ca06c.png)

### 압축

![image](https://user-images.githubusercontent.com/43740455/138339379-dae9f853-212c-4940-9f3b-4066be8f6e20.png)

- 외부 단편화 해소 위해 공간을 몰아넣는 기법.
- 오버헤드로 인해 효율이 좋진 않다. → **페이징** 기법이 대안

# 메모리 관리

한정된 메모리에 여러 프로세스를 사용하기 위한 기법

## 종류

### Paging

![image](https://user-images.githubusercontent.com/43740455/138339396-5909e996-4d25-4e1f-bf04-a9dd364ab65b.png)

- 프로세스를 **물리적**으로 **일정**한 크기로 나눠서 메모리에 할당
- 프로세스를 작은 크기로 분할하여 외부 단편화를 해결하는 기법
  - 프로세스를 나눈 단위 - Page
  - 메모리를 나눈 단위 - Frame
- 페이지 테이블을 거쳐 논리 주소를 물리 주소로 변경
  - 페이지 테이블 - MMU의 재배치 레지스터를 여러개 둔 것
- 외부 단편화는 해결하나 내부 단편화가 발생

### Segmentation

![image](https://user-images.githubusercontent.com/43740455/138339407-63eb0777-d054-4028-b173-35add01a9d38.png)

- 프로세스를 **논리적** 내용에 기반해 **가변** 크기로 나눠 메모리에 할당
- 세그먼트 테이블 - 세그먼트 번호, 시작 주소(base), 크기(limit)
- 단점 : 외부 단편화 - 크기가 다양한 세그먼트를 메모리에 적재하는 과정에서..
