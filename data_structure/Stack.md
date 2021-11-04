# Stack

* Last In First Out. 너무나도 잘 알고 있는 후입 선출 구조

## 프로그래밍에서의 Stack

## 구현

1. 배열
   * 고정된 크기의 배열에 구현
   * 파이썬 리스트는 배열이 아님
     * 파이썬의 배열은 주솟값을 저장한 주소.
     * 실제 원솟값의 주소를 저장하는게 아니라 몇 번째 원소인지 정도만 저장
     * 이중참조를 해야하기 때문에 속도가 느리다는 단점

```c
#include <iostream> using namespace std; 
const int MAX_NUM = 100;
typedef int element; 
class Stack{ 
    public: 
    element data[MAX_NUM]; 
    int top; //마지막 원소를 가리키는 인덱스 
    Stack(){ 
        top=-1; //초기화
    } 
    bool is_empty(){ 
        //if(top == -1) return true; 
        //else return false; 
        return(top == -1); 
    } bool is_full(){ 
        //if (top == MAX_NUM-1) return true; 
        //인덱스는 0~99까지니까 -1 해주어야 한다. 
        //else return false; 
        return (top == MAX_NUM-1); } 
    void push(element value){ 
        if(is_full()){ 
            out << "STACK OVERFLOW!!!" << endl;
            exit(-1); 
        }
        else{ 
            //top++; //data[top] = value; 
            data[++top] = value; 
        } 
    } 
    element pop(){ 
        if (is_empty()){ 
            cout<<"STACK UNDERFLOW!!!"<<endl; 
            exit(-1); 
        } 
        else{ 
            //element x = data[top]; 
            //top--; //return(x); 
            return(data[top--]); 
        } 
    } 
    element peek(){ 
        if(is_empty()){
            cout<< "STACK UNDERFLOW!!!" <<endl; 
            exit(-1); } else{ return(data[top]); 
         } 
    }
}; 
```

배열의 형태는 다른 마크다운 참조

2. 연결 리스트

* 크기가 고정되어 있지 않음.
* 연결리스트의 노드에 다음 노드의 메모리를 저장하는 방식으로 구현

```java
public class MyStack {
	private static class StackNode {
    	private T data;
        private StackNode next;
        public StackNode(T data) {
        	this.data = data
    	}    
   	}
    
    private StackNode top;
    public T pop() {
    	if (top == null) throw new EmptyStackException();
        T item = top.data;
        top = top.next;
        return item;
    }
    
    public void push(T item) {
    	StackNode t = new StackNode(item);
        t.next = top;
        top = t;
    }
    
    public T peek() {
    	if(top == null) throw new EmptyStackException();
        return top.data;
    }
    
    public boolean isEmpty() {
    	return top == null;
    }
}
```

## Stack as a memory

* 파이썬의 리스트는 동적 메모리이지만, 운영체제에서는 크기를 고정해서 사용한다.
  * 크기를 넘어선다. --> Stack Overflow

## 메모리 상에서의 Stack

![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmR69E%2Fbtqyzv7SzFs%2FGkt85LgH9hlLUff3JfsqN0%2Fimg.jpg)

* 프로세스가 메모리에 올라올 때 운영체제가 크게 4가지 메모리 영역을 할당
* Stack, Heap, 데이터, 코드 영역
* 스택은 가장 높은 메모리 주소를 할당하고, 프로세스에 메모리가 로드될 때 고정된 크기의 메모리만을 받습니다. 그리고 할당받은 크기는 변경 불가.
* 높은 주소값에서 낮은 주소값 순으로 할당. 
* 이 스택 메모리 영역은 프로그램이 자동으로 사용되는 임시 메모리 영역으로 지역변수, 매개변수, 리턴 값 등과 같이 잠시 사용 되었다가 사라지는 데이터를 저장하는 영역이다. 
* 메모리 작업이 종료되면 할당된 메모리 공간은 반환되어 없어진다.
* 메모리 관리에 신경쓸 필요가 없는 장점을 갖지만( 운영체제가 알아서 할당, 반환처리를 해줌. ) 크기에 제약이 있다는 단점이 있다.
  런타임시 크기 변경은 불가!
* 스택 명령 사용시 메모리 번짓값을 기준으로 push (감소) / pop (증가)하는 형태로 수행된다고 합니다.

## 실제 사용례

![recur](https://dojang.io/pluginfile.php/648/mod_page/content/22/unit67-1.png)

* 특히 재귀 알고리즘의 구현에서 많이 사용.
* 위에서 언급한 임시데이터들을 스택에 계속해서 넣어주고, 재귀함수를 탈출해서 (마지막 지점) 백트래킹이 시작될 때는 스택에 넣었던 임시 데이터를 빼줘야 합니다. 
* 재귀는 연산속도에서 불리한 점이 발생한다. 한 번 재귀 호출이 일어날 때 마다 계속 스택을 쌓아나가므로 최종 결과를 되돌려 받으려면 쌓인 스택을 전부 다시 빼내야 하기 때문이다. 따라서 신중하게 사용하는 것이 좋다.

