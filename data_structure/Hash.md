# Hash

- 데이터를 효율적으로 관리하기 위해 임의의 길이 데이터를 고정된 길이의 데이터로 매핑하는 것

- 해시함수를 구현하여 데이터값을 해시값으로 매핑
- 내부적으로 Array를 사용하여 데이터를 저장. 데이터 고유의 인덱스로는 key의 고유 숫자 값이 저장됨.
- 특정 데이터만의 고유한 위치를 갖기 때문에 삽입, 삭제 과정이 다른 데이터와 상관없이 진행. 즉 추가적인 비용이 없도록 만들어진 구조.

<br>

## Hash Table

- 해시 테이블은 <Key, Value>로 데이터를 저장하는 자료 구조 중 하나
- Key 값에 해시 함수를 적용해 index를 생성
- index를 활용하여 값을 저장, 검색 가능
- Key 값을 해싱하여 검색, 저장하므로 평균 시간 복잡도는 O(1)  
  (항상 O(1)이 아닌 것은 collision 해소 때문)
- data가 많아지면 서로 다른 데이터가 같은 해시값을 가져 collision 현상 발생

<br>

## Hash Collision

- 해시 충돌은 해시함수가 서로 다른 두 개의 입력값에 대해 동일한 출력값을 내는 상황을 의미
- 서로 다른 Key값이 동일한 index로 매핑될 경우, 해시 충돌이 발생하여 해시 테이블의 성능이 저하

![image](https://user-images.githubusercontent.com/43740455/145608187-bb22317d-a4be-451f-9cd9-f69f763cda0c.png)

위 그림에서 John Smith와 Sandra Dee라는 키의 해시가 서로 충돌을 일으킨다.

<br>

### 충돌 해결 방법

- #### 분리 연결법 (Separate Chaining)

  - 동일한 버킷의 데이터에 대해 추가 메모리와 자료구조를 사용하여 다음 데이터의 주소를 저장하는 것
  - 보통 linked list와 tree를 사용하여 구현
  - 일반적으로 개방주소법보다 빠름  
    (개방주소법은 밀도가 높아질수록 Worst Case 발생 빈도가 높아지기 때문)
  - Java 7은 분리 연결법으로 Hash Map을 구현하고 있음

  - 보조 해시 함수(supplement hash function)을 함께 사용하여 충돌 가능성을 낮추고 Worst Case에 가까워지는 경우를 줄인다.

    - Linked List

      - 각각의 버킷을 linked list로 만들어 충돌이 발생하면 해당 버킷의 list에 추가하는 방식
      - 연결리스트를 사용하므로 삽입과 삭제가 간단하나 작은 데이터들을 저장할 때 연결리스트 자체의 오버헤드가 부담됨
      - Open Address 방식보다 테이블 확장을 늦출 수 있다.

    - Tree ( [Red-Black Tree](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree) )

      - linked list 대신 tree를 사용하며, 하나의 버킷에 할당된 데이터(key-value)쌍의 수가 많을 때 사용
      - 데이터가 적으면 Linked List가 적합 (tree는 기본적으로 메모리 사용량이 크기 때문)

      - 하나의 버킷에 할당된 data의 개수가 6개인지 8개인지를 기준으로 결정  
        (변경하는데 소요되는 비용을 줄이기 위함)

        - 기준이 6, 7이라면 버킷이 6개에서 1개가 늘어날 때 링크드리스트에서 트리로 자료구조를 변경해야 함. 하나가 삭제되면 다시 자료구조를 변경해야 함. 즉 Switching cost가 과다하게 발생하기 때문에 2라는 여유를 잡아 8개를 기준으로 둔 것

          ![image](https://user-images.githubusercontent.com/43740455/145608300-30c58fb0-6309-404e-a87e-1bdbbc80436d.png)

- #### 개방 주소법 (Open Address)

  - 해시함수로 얻은 주소가 아닌 다른 주소에 데이터를 저장할 수 있도록 허용
  - 기존의 해시 테이블의 비어있는 공간을 활용하는 방법
  - 최악은 비어있는 버킷을 찾지 못하고 탐색을 시작한 위치로 돌아오는 경우
  - 비어있는 공간을 찾는 방법은 크게 3가지 방법이 존재

    - Linear Probing (선형 탐사)

      - 충돌이 난 지점부터 정해진 고정 폭으로 순차적 탐색하며 비어있는 버킷을 탐색

    - Quadratic Probing (제곱 탐사)

      - 2차 함수를 사용하여 비어있는 버킷을 탐색

    - Double Hashing Probing

      - 충돌이 발생하면 2차 해시 함수를 사용하여 새로운 주소를 할당
      - 연산량이 위의 두 가지보다 많이 요구된다.

### 비교

- 두 방식 모두 Worst Case에서 O(M)
- 하지만 개방주소법은 연속된 공간이기때문에 분리연결법에 비해 캐시 효율이 높음  
  데이터가 적다면 개방주소법의 성능이 좋음
- 하지만 분리연결법은 버킷을 계속 사용하지 않기때문에 테이블 확장을 늦출 수 있다는 장점

> 충돌이 일어나도 해시 테이블을 쓰는 이유는?
>
> 하드디스크나, 클라우드에 존재하는 무한한 데이터들을 유한한 개수의 해시값으로 매핑하면 작은 메모리로도 프로세스 관리가 가능하기 때문.
>
> - 빠른 데이터 검색.
> - hash-table O(1), 이진탐색트리 O(logN)

<br>

#### Resizing

- 해시 테이블은 Key, Value쌍이 늘어날수록 해시 충돌 확률이 올라가며 성능이 떨어짐
- 충돌을 줄여 성능 손실 문제를 해결하기 위해 임계점을 지나면 버킷의 수를 두 배로 늘림
- 기준 임계점은 빈공간 대비 75%(0.75=load factor)가 사용될 때
- java의 버킷 개수 기본값은 16. 임계점 후 2배씩 최대 2^30까지 확장.

동적확장(https://d2.naver.com/helloworld/831311)

<br>

## Hash Function (Hash Method)

- 해시 함수는 임의의 길이의 데이터를 고정된 길이의 데이터(hashcode)로 매핑하는 함수로 고유한 인덱스 값을 만들어낸다.
- 저장되는 key의 값들을 작은 범위의 값으로 변환하기 위함
- 해시 함수의 성능은 입력 영역에서 해시 충돌 확률로 결정

  - 해시 충돌 확률이 높을수록 서로 다른 데이터를 구별하기 어려워지고 검색하는 비용이 증가
  - Collision이 많아질수록 시간복잡도는 O(1)에서 O(n)에 가까워짐

- 좋은 해시함수는 무조건 1:1 매칭을 만드는 것이 아니라 Collision을 최소화 하는 방향으로 설계된 함수
- 무조건적인 1:1매칭은 과다한 메모리를 소모
- 해시 테이블에서는 크게 4가지 해시 함수가 사용된다.
  - **Division Method**
    - 모듈러 연산을 이용하는 방법으로 입력값을 테이블의 크기로 나누어 계산한다.
    - 주소 = 입력값 % 테이블 크기
  - **Digit Folding Method**
    - 각 Key의 문자열을 ASCII 코드로 바꾼 뒤, 그 값의 합을 주소로 사용하는 방법이다.
  - **Multiplication Method**
    - 숫자로 된 Key와 0과 1 사이의 실수, 보통 2의 제곱수를 사용하여 해시값을 계산한다.
    - Hash(Key) = (Key _ R(0..1) % 1) _ 2^n
  - **Universal Hashing**
    - 다수의 해시 함수를 만들어 집합 H에 넣어두고, **무작위**로 해시함수를 선택해 해시값을 만든다.

<br>

<br>

## 퍼즐조각 채우기 hash funcion으로 풀이

```python
from collections import defaultdict

block_hashs = [0] * 4
block_size = 0

def solution(game_board, table):
    N = len(game_board)
    row_base = 1 << 1
    column_base = 1 << N
    dx = [1, 0, -1, 0]
    dy = [0, 1, 0, -1]
    global block_hashs

    def dfs(board, x, y):
        visited[x][y] = 1
        global block_size, block_hashs
        block_size += 1
        row_exponent = [x, N - y, N - x, y]
        column_exponent = [y, x, N - y, N - x]
        for k in range(4):
            block_hashs[k] += ((row_base ** row_exponent[k]) *
                               (column_base ** column_exponent[k]))
        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]
            if not (0 <= nx < N and 0 <= ny < N):
                continue
            if not board[nx][ny] or visited[nx][ny]:
                continue
            dfs(board, nx, ny)

    def process_hash(hash_values):
        for i, hash_value in enumerate(hash_values):
            for hash_base in [row_base, column_base]:
                while hash_value % hash_base == 0:
                    hash_value //= hash_base
            hash_values[i] = hash_value
        return min(hash_values)

    visited = [[0] * N for _ in range(N)]
    game_board_inverted = [[1 - x for x in row] for row in game_board]
    game_board_hash = defaultdict(int)
    block_size_by_hash = {}
    for x in range(N):
        for y in range(N):
            if not game_board_inverted[x][y] or visited[x][y]:
                continue
            block_hashs = [0] * 4
            global block_size
            block_size = 0
            dfs(game_board_inverted, x, y)
            block_hash = process_hash(block_hashs)
            game_board_hash[block_hash] += 1
            block_size_by_hash[block_hash] = block_size

    visited = [[0] * N for _ in range(N)]
    answer = 0
    for x in range(N):
        for y in range(N):
            if not table[x][y] or visited[x][y]:
                continue
            block_hashs = [0] * 4
            dfs(table, x, y)
            block_hash = process_hash(block_hashs)
            if game_board_hash[block_hash] > 0:
                game_board_hash[block_hash] -= 1
                answer += block_size_by_hash[block_hash]
    return answer

```

<br>

## Hash Table vs Hash Map vs Hash Set

- 3개의 자료구조는 모두 해시함수를 사용
- 그러나 다음과 같은 차이가 존재
- 맵은 set보다 메모리를 더 쓰기 때문에 키 존재유무만 궁금하면 set을 쓰는게 적합

|            | Hash Table | Hash Map | Hash Set |
| ---------- | ---------- | -------- | -------- |
| 동기화     | o          | x        | x        |
| 값의 중복  | o          | o        | x        |
| 인터페이스 | Map        | Map      | Set      |

<br>

## Python의 dict, set

- 파이썬의 hash function, table 구현

  - https://davinci-ai.tistory.com/19

- 간단한 구현
  - https://idm101.tistory.com/3

#### dictobject entry의 struct

![image](https://user-images.githubusercontent.com/43740455/145608395-7e4ce604-c265-4ffc-a9ed-08eb7bec7be4.png)

#### setobject entry의 struct

![image](https://user-images.githubusercontent.com/43740455/145608422-bc6bfb1f-4151-4e94-8c31-10236418af04.png)

- 언뜻 보면 비슷해 보이지만 큰 차이가 있음
- dictentry에는 value를 가르키는 주소값이 추가로 저장되어 있고 setentry에는 value없이 key만 저장하고 있음을 알 수 있음
- 실제로 c 코드를 보니 set에는 value가 없고 dict에는 key, value로 매핑되어 있는지 확실히 알 수 있음

<br>

## 그럼 set에서 pop하면?

- hash table을 구현해보면 알겠지만 hash table을 순회하면 첫번째 bucket부터 안의 키를 순회하고 다음 bucket으로 가는 식으로 순회함
- 이렇게 순회를 하니 pop을 한다고 가장 뒤에 있는 값까지 순회해서 내뱉는 것이 아닌 가장 처음에 나온 값을 뱉게되는 것

#### setobject.c에서 pop method

![image](https://user-images.githubusercontent.com/43740455/145608452-bd06789d-552a-4e9e-85fd-b8f28e6a1cc9.png)
간단히 말하면, while에서 존재하는 key가 나올 때까지 순회를 하고 나오면 해당 key를 return한다.

<br>

## Reference

- [cpython dict 구현체](https://hg.python.org/cpython/file/ec91ee7d9d8d/Objects/dictobject.c)
- [cpython set 구현체](https://hg.python.org/cpython/file/ec91ee7d9d8d/Objects/setobject.c)
- [Python hash_table class](https://www.geeksforgeeks.org/hash-map-in-python/)

<br>

## 더 알고싶다면?

- [파이썬의 메모리 관리 기법](https://velog.io/@jewelrykim/Managed-Language-%ED%8C%8C%EC%9D%B4%EC%8D%AC%EC%9D%98-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B4%80%EB%A6%AC%EA%B8%B0%EB%B2%95)
- [c++ map, set, multi](https://modoocode.com/224)
- [javascript의 Hashmap 구현](https://devguna.com/entry/Java-HashMap-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A1%9C-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0)
- [ES6 collections](http://hacks.mozilla.or.kr/2015/12/es6-in-depth-collections/)
- [js의 Map괴 set객체](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Keyed_collections)
- [js의 Map괴 set객체2](https://velog.io/@yesdoing/JavaScript-Collections)
- [Java의 해시 충돌 회피](https://d2.naver.com/helloworld/831311)
