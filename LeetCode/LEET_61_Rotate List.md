# LEET/61/Rotate List

#링크드리스트#SinglyLinkedList

## Want

Linked List의 `head`와 정수 `k`가 주어진다  
Linked List를 `k`번만큼 오른쪽으로 `rotate`한 Linked List를 반환하라

### Constraints

(0 <= k <= 2 \* 109)

## INPUT && OUTPUT

INPUT: LINKED LIST NODE, NUMBER  
OUTPUT: LINKED LIST NODE

## First Understanding & Seperating

1. Linked List의 총 노드 개수 구하기 (`nodeCount`)
2. `k % nodeCount` 만큼 이동
3. 예를 들어 `k = 2`일때, 끝에서 3번째 노드 == `tail`
4. `tail.next = head;`
5. `return`

### solve 1

겹치지 않게 빠짐없이 로직을 작성했다고 생각했으나  
**_자잘한 실수_** 들로 인해 생각보다 시간이 꽤 걸렸던 문제  
직관적으로 설명하자면,
뒤에서 `k`번째 노드까지 떼서 `head` 앞으로 이어 붙이면 된다  
**_무의미한 반복_** 을 방지하기 위해 **_나머지 연산_** 을 이용하였고  
`head`, `tail` 포인터를 통해 **_Keep Singly Linked List_**  
시간 복잡도 O(N), 공간 복잡도 O(1)

### solve 1 Code

```js
// Runtime: 108 ms, faster than 43.32%
// Memory Usage: 44.6 MB, smaller than 16.78%
const rotateRight = function (head, k) {
  if (!head || k === 0) return head;

  let current = head;
  let nodeCount = 0;
  let tail;

  while (current) {
    nodeCount++;
    tail = current;
    current = current.next;
  }
  if (nodeCount === 1) return head;

  const removeCount = k % nodeCount;
  if (removeCount === 0) return head;

  current = head;
  for (let i = 0; i < nodeCount - removeCount - 1; i++) {
    current = current.next;
  }
  tail.next = head;
  head = current.next;
  tail = current;
  tail.next = null;
  return head;
};
```

## Refactoring

### solve 2

Linked List를 **_Cycle_** 형식으로 만들고  
`head`, `tail` 포인터만 새롭게 할당했더니 훨씬 문제가 **_쉽게, 직관적으로_** 해결되었다  
더하여 `solve 1` `nodeCount === 1`일 때의 조건문을 맨 위로 올려 가독성 향상시켰다  
still 시간 복잡도 O(N), 공간 복잡도 O(1)

### solve 2 Code

```js
// Runtime: 116 ms, faster than 30.25%
// Memory Usage: 43.8 MB, smaller than 90.25%
const rotateRight = function (head, k) {
  if (!head || !head.next || k === 0) return head;

  let nodeCount = 0;
  let tail;
  let current = head;

  while (current) {
    tail = current;
    current = current.next;
    nodeCount++;
  }

  tail.next = head;
  const iterCount = k % nodeCount;

  for (let i = 0; i < nodeCount - iterCount; i++) {
    tail = tail.next;
  }
  head = tail.next;
  tail.next = null;
  return head;
};
```

## 배운 점 or 주의할 점

**_사고의 유연함_** 은 어디서 나오는가?  
어? 이렇게 하면 되겠는데? 라는 생각에서 벗어나 조금 더 멀리서 지켜보아야 하나?  
풀이 이후에 항상 다른 사람들의 코드를 보면서 새로운 시각을 배워간다  
사고의 flexible **_flexin~_**
