## Heap

### 정의

##### min heap: 모든 노드가 자신의 자식 노드보다 값이 작거나 같음(min tree) + complete binary tree

##### max heap: 모든 노드가 자신의 자식 노드보다 값이 크거나 같음(max tree) + complete binary tree



##### 기능

Create, HeapFull, Insert, HeapEmpty, Delete

=> Heap은 Priority queue를 구현할 때 사용



### Priority queue란?

queue: FIFO

priority queue: key값이 높은 것(낮은 것)부터 삭제

ex1 )

insert 1  5  3   => delete 5  3  1

insert 1  3  5   => delete 5  3  1

ex2 ) Job queue (OS)

| CPU  | MEMORY | key  |
| ---- | ------ | ---- |
|      | P1     | 10   |
|      | P2     | 30   |
|      | P3     | 20   |



### Priority queue 구현

#### array로 구현 / linked list로 구현

- Unordered array, Unordered linked list

  | 5    | 3    | 1    | 2    | 4    | 6    |
  | ---- | ---- | ---- | ---- | ---- | ---- |

  8 추가하면,

  | 5    | 3    | 1    | 2    | 4    | 6    | 8    |
  | ---- | ---- | ---- | ---- | ---- | ---- | ---- |

  - Insertion : **O(1)**

  - Deletion : **O(n)**

    

- Sorted array, Sorted linked list

  | 1    | 2    | 3    | 4    | 5    | 6    |
  | ---- | ---- | ---- | ---- | ---- | ---- |

  4 삭제하면, 

  | 1    | 2    | 3    | 5    | 6    |
  | ---- | ---- | ---- | ---- | ---- |

  - Insertion : **O(n)**

  - Deletion : **O(1)**

    

#### heap으로 구현

- Insertion : reorganization 필요
- Deletion : reorganization 필요



### heap 구현 코드

```c
#define MAX_ELEMENTS 200 /* maximum heap size + 1 */
#define HEAP_FULL(n) (n == MAX_ELEMENTS – 1)
#define HEAP_EMPTY(n) (!n)
typedef struct {
	int key;
} element;
element heap[MAX_ELEMENTS];
int n = 0;
```

#### 1. Insertion from a Max Heap (C code)

```c
void insert_max_heap(element item, int *n) {
	/* insert item into a max heap of current size *n */
	int i;
	if(HEAP_FULL(*n)) {
		fprintf(stderr, "The heap is full.\n");
		exit(1);
	}
	i = ++(*n);
	while((i != 1) && (item.key > heap[i/2].key)) {
		heap[i] = heap[i/2];
		i /= 2;
	}
	heap[i] = item;
}
```



depth(height)만큼 while문을 돎 => complete binary tree이므로 n개의 node를 갖는 tree의 height = log2n

#### Insertion : O(log2n)



#### 2. Deletion from a Max Heap (C code)

```c
element delete_max_heap(int *n) {
	/* delete element with the highest key from the heap */
	int parent, child;
	element item, temp;
	if(HEAP_EMPTY(*n)) {
		fprintf(stderr, "The heap is empty");
		exit(1);
	}
	/* save value of the element with the largest key */
	item = heap[1];
	/* use the last element in the heap to adjust heap */
	temp = heap[(*n)--];
	parent = 1;
	child = 2;
    while(child <= *n) {
		/* find the larger child of the current parent */
		if((child < *n) && (heap[child].key < heap[child+1].key)) child++;
		if(temp.key >= heap[child].key) break;
		/* move to the next lower level */
		heap[parent] = heap[child];
		parent = child;
		child *= 2;
	}
	heap[parent] = temp;
	return item;
}
```





#### 3. Python Code (Max Heap)

```python
class MaxHeap(object):
 
    def __init__(self):
        self.queue = [None]
 
    def insert(self, n):
        # 삽입할 원소를 맨 마지막에 추가
        self.queue.append(n)
        last_index = len(self.queue) - 1
        # 부모를 타고 올라가면서 크기를 비교해준다.
        while 0 <= last_index:
            parent_index = self.parent(last_index)
            if 0 < parent_index and self.queue[parent_index] < self.queue[last_index]:
                self.swap(last_index, parent_index)
                last_index = parent_index
            else:
                break
 
    def delete(self):
        last_index = len(self.queue) -1
        if last_index <= 0:
            return -1
        self.swap(1, last_index)
        maxv = self.queue.pop()
        self.max_heapify(1) # root에서부터 재정렬
        print(maxv)
        return maxv
 
 
    # 임시 root 값부터 자식들과 값을 비교해나가며 재정렬하는 함수
    def max_heapify(self, i):
        last_index = len(self.queue) - 1
        left_index = self.leftchild(i)
        right_index = self.rightchild(i)
        max_index = i # 더 큰 값의 index를 넣어준다
 
        if left_index <= last_index and self.queue[max_index] < self.queue[left_index]:
            max_index = left_index
        if right_index <= last_index and self.queue[max_index] < self.queue[right_index]:
            max_index = right_index
 
        # 만약 자신이 가장 큰게 아니면 heapify
        if max_index != i:
            self.swap(i, max_index)
            self.max_heapify(max_index)
 
 
    def swap(self, i, parent_index):
        self.queue[i], self.queue[parent_index] = self.queue[parent_index], self.queue[i]
 
 
    def parent(self, index):
        return index // 2
 
 
    def leftchild(self, index):
        return index*2
 
 
    def rightchild(self, index):
        return index*2 + 1
 
 
    def printHeap(self):
        print(self.queue)

```



depth(height)만큼 while문을 돎 => complete binary tree이므로 n개의 node를 갖는 tree의 height = log2n

#### Deletion : O(log2n)



#### Priority Queue 구현 했을 때 시간복잡도 정리

| Representation        | Insertion    | Deletion     |
| --------------------- | ------------ | ------------ |
| unordered array       | O(1)         | O(n)         |
| unordered linked list | O(1)         | O(n)         |
| sorted array          | O(n)         | O(1)         |
| sorted linked list    | O(n)         | O(1)         |
| **max heap**          | **O(log2n)** | **O(log2n)** |
| **min heap**          | **O(log2n)** | **O(log2n)** |


#### 원래 시간 복잡도 (Worst Case)

| Representation        | Insertion    | Deletion     | 탐색   |
| --------------------- | ------------ | ------------ | ------ |
| array                 | O(n)         | O(n)         | O(1)   |
| linked list           | O(n)         | O(n)         | O(n)   |


- 어떤 자료구조가 좋은가?

**heap : insertion과 deletion이 자주 일어나는 자료일 때 유용**
