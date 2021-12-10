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

- `*` : 포인터 수식어
- `&` : 변수의 메모리 주소

```c
#include <stdio.h>

int main() {
		int *numPtr;      // 포인터 변수 선언
		int num1 = 10;    // int형 변수를 선언하고 10 저장
		numPtr = &num1;   // num1의 메모리 주소를 포인터 변수에 저장


		char c = 'A';
		char *pc = &c;    // 또는 선언과 동시에 할당도 가능

		printf("%p\n", numPtr);    // 0055FC24: 포인터 변수 numPtr의 값 출력
		printf("%p\n", &num1);     // 0055FC24: 변수 num1의 메모리 주소 출력

    return 0;
}
```

- 수식어의 위치는 자유롭다.

```c
// 셋 다 같은 의미

int* numPtr;     // 자료형 쪽에 *을 붙임
int * numPtr;    // 자료형과 변수 가운데 *를 넣음
int *numPtr;     // 변수 쪽에 *을 붙임
```

- 활용 - swap 함수

```c
// 실제 값 변경 X - Call by value
void swap (int a, int b){
    int temp;
    if (a > b){
        temp = a;
        a = b;
        b = temp;
    }
}

// 실제 값 변경 O - Call by Reference
void swap (int *a, int *b){
    int temp;
    if (*a > *b){
        temp = *a;
        *a = *b;
        *b = temp;
    }
}
```

---

## [참고] 캐시 지역성(Locality)

프로세스가 시/공간적으로 유사한 데이터를 집중적으로 참조하는 성질

- **시간적 지역성 (Temporal)**
  - ex) Loop, SubRoutine, LRU
- **공간적 지역성 (Spatial)**
  - 메모리 상 인접 데이터의 재 이용률이 높음
  - ex) 데이터베이스 파티션
- **순차적 지역성 (Sequential)**
  - 기억장치에 저장된 순서대로 이용될 가능성이 높음
  - ex) 배열 데이터
