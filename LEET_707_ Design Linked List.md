# LeetCode/707/Design Linked List

#링크드리스트

## Want
단방향 링크드리스트 구현  

## Understanding & Seperating
1. `MyLinkedList():` 링크드 리스트 초기화
2. `get(index):` `index`번째의 `value` 반환, `index`가 유효하지 않다면 `-1` 반환
3. `addAtHead(value):` 링크드 리스트의 맨 앞에 `value`값을 가지는 노드 추가
4. `addAtTail(value):` 링크드 리스트의 맨 뒤에 `value`값을 가지는 노드 추가
5. `addAtIndex(index, value):` `index`번째에 `value`값을 가지는 노드 추가  
  `index === this.length`라면 맨 뒤에 노드를 추가  
  `index > this.length`라면 추가 안 함
6. `deleteAtIndex(index):` `index`번째의 노드 삭제

### solve 1
단방향 링크드 리스트의 구현(Singly Linked List)  
`class`가 아닌 프로토타입을 사용하여 문제 해결

```js
// Runtime: 143 ms, faster than 85.44%
// Memory Usage: 50.2 MB, smaller than 80.86%
const Node = function (value) {
  this.value = value;
  this.next = null;
};

const MyLinkedList = function () {
  this.head = null;
  this.tail = null;
  this.length = 0;
};

MyLinkedList.prototype.get = function (index) {
  if (index < 0 || index >= this.length) return -1;

  let current = this.head;
  for (let i = 0; i < index; i++) {
    current = current.next;
  }
  return current.value;
};

MyLinkedList.prototype.addAtHead = function (value) {
  const newNode = new Node(value);

  if (!this.head) {
    this.head = newNode;
    this.tail = newNode;
  } else {
    newNode.next = this.head;
    this.head = newNode;
  }
  this.length++;
};

MyLinkedList.prototype.addAtTail = function (value) {
  const newNode = new Node(value);

  if (!this.head) {
    this.head = newNode;
    this.tail = newNode;
  } else {
    this.tail.next = newNode;
    this.tail = newNode;
  }
  this.length++;
};

MyLinkedList.prototype.addAtIndex = function (index, value) {
  if (index > this.length) return;
  if (index === this.length) this.addAtTail(value);
  else if (index === 0) this.addAtHead(value);
  else {
    const newNode = new Node(value);
    let preNode = this.head;
    for (let i = 0; i < index - 1; i++) {
      preNode = preNode.next;
    }
    newNode.next = preNode.next;
    preNode.next = newNode;
    this.length++;
  }
};

MyLinkedList.prototype.deleteAtIndex = function (index) {
  if (index < 0 || index >= this.length) return;
  if (index === 0) this.head = this.head.next;
  else {
    let preNode = this.head;
    for (let i = 0; i < index - 1; i++) {
      preNode = preNode.next;
    }
    const deletedNode = preNode.next;
    preNode.next = deletedNode.next;
    if (preNode.next === null) this.tail = preNode;
  }
  this.length--;
  if (this.length === 0) this.tail = null;
};
```

## 배운 점 or 주의할 점
링크드 리스트에 굉장히 자신감이 있어서 문제를 척척 풀어나가고 있었는데 막히는 부분이 존재했다  
머리를 싸매면서 고민했었는데 자만함에 빠져 문제를 제대로 읽지 않고 문제를 풀고 있었다  
~~예를 들어 `get`함수의 경우 노드의 `value`를 반환해야 하는데 노드를 반환하고 있었다..~~  
문제를 풀 때 꼼꼼히 읽고 자만하지 말자  
사자는 토끼를 잡을 때도 전력을 다하는 법
