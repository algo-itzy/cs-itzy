## Tree

![image](https://user-images.githubusercontent.com/43740455/145623641-d5c1a24e-e568-4d97-8285-ad486db2ae98.png)

- 트리는 그래프의 한 종류로, '최소 연결 트리'라고도 부름
- 비선형구조는 선형구조(데이터들을 순차적으로 나열시킨 형태)와 다르게 데이터가 계층적으로 구성
- linked list의 단점인 검색 시 노드의 처음부터 찾아가야 하는 점을 보완하여 검색을 중간부터 시작하여 좌우 중 하나로 분기, 1/2씩 검색 대상을 줄여나가 이진검색의 효과

### 구현

```pseudocode
class Tree {
  constructor(value) {
		// constructor로 만든 객체는 트리의 Node가 됩니다.
    this.value = value;
    this.children = [];
  }

	// 트리의 삽입 메서드를 만듭니다.
  insertNode(value) {
    const childNode = new Tree(value);
    this.children.push(childNode);
  }

	// 트리 안에 해당 값이 포함되어 있는지 확인하는 메서드를 만듭니다.
  contains(value) {
     if(this.value === value){
       return true;
     }
	   for(let i =0;i<this.children.length;i++){
      const childNode = this.children[i];
      if(childNode.contains(value)){
        return true;
      }
    }
		// 전부 탐색했음에도 불구하고 찾지 못했다면 false를 반환합니다.
    return false;
  }
}
```

### 종류

#### 이진 트리 (Binary Tree)

- 각 노드가 최대 두 개의 자식을 갖는 트리

  ![image](https://user-images.githubusercontent.com/43740455/145623812-dc5c23de-bfb0-43a4-9ed0-7703f6f1927d.png)

##### 정 이진 트리 (Full Binary Tree)

- 모든 레벨이 꽉 찬 이진 트리

- 모든 노드가 0개 또는 2개의 자식 노드를 갖는 트리

  ![image](https://user-images.githubusercontent.com/43740455/145623862-26a03513-bbd2-4c1b-8d7a-7ce7c15e77c7.png)

##### 완전 이진 트리 (Complete Binary Tree)

- 포화 이진 트리처럼 모든 레벨이 꽉 찬 상태는 아니지만, 자식 노드가 왼쪽에서부터 차례로 채워져있는 이진 트리.

- 마지막 레벨을 제외하고 모든 레벨이 완전히 채워져 있다.

- 노드가 왼쪽에서 오른쪽으로 채워져야 한다.

  ![image](https://user-images.githubusercontent.com/43740455/145623977-843db35d-852e-4e40-9cff-0292c115c8f1.png)

##### 포화 이진 트리 (Perfect BInary Tree)

- 모든 내부 노드가 두개의 자식 노드를 가진다.

- 모든 말단 노드는 같은 높이에 있어야 한다.

  ![image](https://user-images.githubusercontent.com/43740455/145624019-71e2ca2b-a838-4a52-95f0-a76ea757fea6.png)

#### 이진 검색 트리 (Binary Search Tree)

![image](https://user-images.githubusercontent.com/43740455/145624047-c99c503c-2a4c-4f4e-9b94-637618b7e27d.png)

- 기준이 되는 노드의 왼쪽 자식 노드는 기준 노드보다 크기가 작아야하고, 기준 노드의 오른쪽 자식 노드는 기준 노드보다 커야한다.
- 각 노드의 값은 중복되지 않는다.
- 위와 같은 이유로 일반 이진 트리보다 값을 찾기가 쉽다.
- 주요 연산 : 검색(retreive), 삽입(insert), 삭제(delete)

##### 구현

```pseudocode
class BinarySearchTree {
  constructor(value) {
		// constructor로 만든 객체는 이진 탐색 트리의 Node가 됩니다.
    this.value = value;
    this.left = null;
    this.right = null;
  }
	// 이진 탐색 트리의 삽입하는 메서드를 만듭니다.
  insert(value) {
		// 입력값을 기준으로, 현재 노드의 값보다 크거나 작은 것에 대한 조건문이 있어야 합니다.
		// 보다 작다면, Node의 왼쪽이 비어 있는지 확인 후 값을 넣습니다.

    if (value < this.value) {
      if (this.left === null) {
        this.left = new BinarySearchTree(value);
      } else {.
        this.left.insert(value);
      }
		// 보다 크다면, Node의 오른쪽이 비어 있는지 확인 후 값을 넣습니다.
    } else if (value > this.value) {
      if (this.right === null) {
        this.right = new BinarySearchTree(value);
      } else {
        this.right.insert(value);
      }
		//그것도 아니라면, 입력값이 트리에 들어 있는 경우입니다.
    } else {
      // 아무것도 하지 않습니다.
    }
  }

	// 이진 탐색 트리 안에 해당 값이 포함되어 있는지 확인하는 메서드를 만듭니다.
  contains(value) {
    if (this.value === value) {
      return true;
    }
		// 입력값을 기준으로 현재 노드의 값보다 작은지 판별하는 조건문이 있어야 합니다.
    if (value < this.value) {
	   if(this.left !== null && this.left.contains(value)){
        return true;
       }else{
         return false;
       }
    }
		// 입력값을 기준으로 현재 노드의 값보다 큰지 판별하는 조건문이 있어야 합니다.
    if (value > this.value) {
     if(this.right !== null && this.right.contains(value)){
        return true;
       }else{
         return false;
       }
    }
		// 없다면 false를 반환합니다.
    return false;
  }

	// 이진 탐색 트리를 전위 순회하는 메서드를 만듭니다.
  preorder(callback) {
		callback(this.value);
    if (this.left) {
      this.left.preorder(callback);
    };
    if (this.right) {
      this.right.preorder(callback);
    };
  }

	// 이진 탐색 트리를 중위 순회하는 메서드를 만듭니다.
  inorder(callback) {
   if (this.left) {
      this.left.inorder(callback);
    };
    callback(this.value);
    if (this.right) {
      this.right.inorder(callback);
    };
  }

	// 이진 탐색 트리를 후위 순회하는 메서드를 만듭니다.
  postorder(callback) {
		//TODO: 전위 순회를 바탕으로 후위 순회를 구현하세요.
     if (this.left) {
      this.left.postorder(callback);
    };
    if (this.right) {
      this.right.postorder(callback);
    };
    callback(this.value);
  }

}
```
