## Blocking I/O & Non-Blocking I/O

> I/O 작업 : 유저 레벨에서 수행 X, Kernel Level에서 수행
>
> Process, Thread : Kernel에 I/O 요청

### Blocking I/O

- User Process가 Kernel에 I/O 요청 함수 호출 -> Kernel 작업 완료 시 함수가 작업 결과 반환
- I/O 작업 진행 중 : User Process 작업 중단, 대기 -> CPU 작업 거의 쓰지 않아서 리소스 낭비

![image](https://user-images.githubusercontent.com/43740455/145610377-5202e7e5-f050-4370-8aa4-43bc33905a44.png)

### Non-Blocking I/O

- Blocking 비효율성 극복
- I/O 작업 진행 중 : User Process 작업 중단 X
- User Process가 Kernel에 I/O 요청 함수 호출 -> 함수 : I/O 요청 후 진행 상황과 상관 없이 바로 결과 반환

![image](https://user-images.githubusercontent.com/43740455/145610387-a28ed15d-7cf3-4fd0-acfd-86b4faf568e4.png)

