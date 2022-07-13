# LEET/430/Flatten a Multilevel Doubly Linked List

#링크드리스트#Doubly Linked List

## Want

Doubly Linked List가 주어진다
Doubly Linked List는 `val`외에 아래 포인터 소유

1. `next`
2. `previous`
3. `child`

Doubly Linked List에서 각 `child` 노드를 제거하고 한 줄로 flatten하라  
flatten 과정에서 `child` node를 만날 시, `child` 노드를 우선하여 flatten 수행  
flatten 이후, 모든 노드의 `child`는 `null`이어야 함

### Constraints

최대 노드 수 == 1000

## INPUT && OUTPUT

INPUT: LINKED LIST NODE  
OUTPUT: LINKED LIST NODE

## First Understanding & Seperating

**_재귀_** 사용하면 편할 듯?

1. `next`가 `null`일 때까지 반복 (다음 노드가 존재하지 않을 때까지)
2. 만약 `child` 노드 존재 시, 복귀할 다음 노드 저장 후 재귀 함수 호출
3. 재귀 호출된 `child` 노드의 `flatten` 수행 후 `child` == null
4. `next`, `prev` 포인터 연결
5. `return`

### solve 1

재귀 함수 호출이 종료되었을 때 **_다음 연결 노드를 기억_** 해야 하므로  
`nextNode` 변수를 통해 `current` 변경
`child` 노드가 존재한다면 재귀 함수의 인수로 `child` 노드를 전달해 재귀 함수 호출  
`child`의 마지막 노드와 `nextNode`를 연결해야 하므로  
`lastChild` 변수를 통해 `prev`, `next` 포인터를 `nextNode`와 연결
시간 복잡도 **_O(N^2)_**

### solve 1 Code

```js
// Runtime: 65 ms, faster than 92.68%
// Memory Usage: 43.5 MB, smaller than 77.45%
const flatten = function (head) {
  let current = head;

  while (current) {
    const nextNode = current.next;

    if (current.child) {
      let lastChild;
      let childNode = flatten(current.child);
      current.next = childNode;
      childNode.prev = current;
      current.child = null;

      while (childNode) {
        lastChild = childNode;
        childNode = childNode.next;
      }
      lastChild.next = nextNode;
      current = lastChild;
    }
    if (nextNode) nextNode.prev = current;
    current = nextNode;
  }
  return head;
};
```

## Refactoring

### solve 2

첫 `while`문의 **_indentation이 3개_** 가 되어버렸다  
따라서 `getTail` 함수를 밖으로 내보내어 indentation을 하나 줄여 작성했다  
더하여 함수를 **_외부로 분리_** 해 `childNode`의 변화 가능성을 줄였기에  
`const`를 사용하여 **_오류 가능성을 최소화_** 했다  
still 시간 복잡도 O(N^2)

### solve 2 Code

```js
const getTail = function (node) {
  while (node.next) {
    node = node.next;
  }
  return node;
};
const flatten = function (head) {
  let current = head;

  while (current) {
    const nextNode = current.next || null;

    if (current.child) {
      const childNode = flatten(current.child);
      current.next = childNode;
      childNode.prev = current;
      current.child = null;

      const lastChild = getTail(childNode);
      lastChild.next = nextNode;
      current = lastChild;
    }
    if (nextNode) nextNode.prev = current;
    current = nextNode;
  }
  return head;
};
```

### solve 3

`Iteraton` 풀이  
시간 복잡도를 줄일 수 있지 않을까 고민했다  
새로운 Linked List의 Head를 작성했고 (`flattenHead`)  
**_후입 선출(LIFO) 구조_** 인 `stack`을 활용하여  
`child` 노드가 존재할 시, `current`에 `child` 노드가 먼저 할당되도록 했다  
따라서 Linked List 순회할 때 `child` 노드를 우선으로 순회하여  
평탄화(flatten)이 가능하게 하였다  
드디어 시간 복잡도 O(N)

### solve 3 Code

```js
// Runtime: 90 ms, faster than 53.59%
// Memory Usage: 44.1 MB, smaller than 6.15%%
const flatten = function (head) {
  if (!head) return head;

  let flattenHead = new Node();
  const flattenResult = flattenHead;
  const stack = [head];

  while (stack.length) {
    const current = stack.pop();

    const newNode = new Node(current.val, null, null, null);
    flattenHead.next = newNode;
    newNode.prev = flattenHead;
    flattenHead = newNode;

    if (current.next) stack.push(current.next);
    if (current.child) stack.push(current.child);
  }
  const result = flattenResult.next;
  result.prev = null;
  return result;
};
```

## 배운 점 or 주의할 점

문제와 예제를 읽고 난 후 재귀를 사용하면 좋겠다고 생각했다 (문제 풀이의 **_방향 설정이 명확_** 해서 좋았음)  
더하여 단방향이 아닌 양방향 리스트라서 `next`에 더해 `prev`까지 생각했던 부분이 꽤 재밌게 다가왔다  
처음에 문제를 이해하여 세분화했던 그대로 **_재귀_** 를 사용해 문제를 해결하면서  
시간 복잡도를 **_O(N)_** 으로 줄일 수 있는지 더 고민해봐야겠다

지금 보니 문제에 Hint가 주어졌었다  
하지만 나는 **Hint를 절대 보지 않는다**  
그것이.. `real`이니까
