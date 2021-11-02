## Array

- **논리적 저장순서와 물리적 저장순서가 일치 → Random Access 가능**

  - **Sequential Access (순차 접근)**  
    원하는 데이터를 찾기 위해 현재 위치부터 차례대로 탐색 (연결 리스트)
    
  - **Random Access (임의 접근)**  
    원하는 위치로 바로 접근 가능
   
- **데이터들이 메모리 공간 상에서 연속적으로 배치**
  - 메모리 공간 활용에 제약
  - 삭제시 빈자리가 생김 → 해결 방안 : 리스트 (데이터가 연속적으로 위치)

<br />

## 시간 복잡도

### 탐색

- O(1) - Random Access 방식

### 추가

- **Best** : 추가 데이터가 맨 뒤 - `O(1)`
- **Worst** : 추가 데이터가 맨 앞 - `O(n)`

### 삭제

- **Best** : 삭제 데이터가 맨 뒤 - `O(1)`
- **Worst** : 삭제 데이터가 맨 앞 - `O(n)`

<br />

## Array vs ArrayList vs Linked List

### Array

- 고정 길이의 배열
- 탐색이 빠르다. - `O(1)`

### ArrayList

- JAVA의 자료구조
- 가변 길이의 배열 (default : 10)
  - capacity를 넘으면 배열 크기 1.5배 증가
- 탐색이 빠르다. - `O(1)`
- 배열보다 추가 속도가 느리다. (추가시 메모리 재할당)

### Linked List

- 탐색이 느리다. - `O(n)`
- 삽입 삭제가 빠르다. - `O(1)` (But, 탐색은 `O(n)`)
  - 그럼 어떤 쓸모? → 트리 구조의 근간

### 결론

- **삽입, 삭제**가 빈번하면 **연결 리스트**
- 데이터 **접근**이 빈번하다면 **배열**

---

## Pointer

- 특정한 데이터가 저장된 **주소 값**을 보관하는 변수
- **Call by Reference**
  - C언어에서 제공하지 않지만 인자로 포인터를 넘겨서 메모리에 직접 접근할 수 있다.
- **자료형이 필요**
  - 시작 주소로부터 얼마만큼의 크기를 읽어야 하는지 지정
- 연속적으로 값을 저장하는 배열과 연계될때 유용
- 포인터를 통해 연결 리스트 구현

![image](https://user-images.githubusercontent.com/43740455/139869757-74b53afa-7112-40f5-b915-4129e6d0ecad.png)

<br />

## 문법

```c
char c = 'A';
float f = 46.1;
double d = 2.73

char *pc = &c;
float *pf = &f;
double *pd = &d;

// * : 포인터 수식어
// & : 변수의 메모리 주소
```

```c
// 실제 값 변경 X
void swap (int a, int b){
    int temp;
    if (a > b){
        temp = a;
        a = b;
        b = temp;
    }
}

// 실제 값 변경 O
void swap (int *a, int *b){
    int temp;
    if (*a > *b){
        temp = *a;
        *a = *b;
        *b = temp;
    }
}
```

<br />

### (부록) 정처기 실기

```c
#include <stdio.h>

main()
{
	int a = 50;
	int *b;
	b = &a;
	*b = *b + 20;
	printf("%d, %d", a, *b);
}

// 결과 : 70, 70
```

```c
#include <stdio.h>

main()
{
	int a[5];
	int *p;

	for(i = 0; i < 5; i++)
		a[i] = i+10;

	p = a;

	for(i = 0; i < 5; i++)
		printf("%d ", *(p+i));
}

// 결과 : 10 11 12 13 14
```
