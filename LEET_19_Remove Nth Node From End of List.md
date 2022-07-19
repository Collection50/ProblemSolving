# LEET/19/Remove Nth Node From End of List

#링크드리스트#SinglyLinkedList

## Want

- Linked List와 n이 주어진다
- Linked List의 뒤에서 `n`번째 노드를 삭제한 Linked List를 반환하라

### Constraints

- (1 <= nodes <= 30)
- (1 <= n <= nodes)

## INPUT && OUTPUT

- INPUT: LINKED LIST NODE, NUMBER
- OUTPUT: LINKED LIST NODE

## First Understanding & Seperating

1. `nodeCount` 변수 활용해 총 노드 개수 찾기
2. `nodeCount - n - 1`번째까지 이동 후
3. 제거 노드의 **전** 노드를 제거 노드의 다음 노드와 연결

### solve 1

1. 만약 `nodeCount == n`이라면 `head = head.next` (예외처리)
2. 만약 노드가 1개라면 `head = null` (예외처리)
3. [위](#first-understanding--seperating)]에서 생각한 대로 실행

### solve 1 Code

```js
// Runtime: 76 ms, 76.30%
// Memory Usage: 43.2 MB, smaller than 36.69%
const removeNthFromEnd = function (head, n) {
  let current = head;
  let nodeCount = 0;

  while (current) {
    nodeCount++;
    current = current.next;
  }

  if (nodeCount === 1) return null;
  if (nodeCount === n) return head.next;

  current = head;
  for (let i = 0; i < nodeCount - n - 1; i++) {
    current = current.next;
  }
  current.next = current.next.next;
  return head;
};
```

## Refactoring

### solve 2

문제의 권장 사항에 Linked List를 **한 번만 순회**하여 문제를 풀어보라는 문장이 존재했었다  
내가 풀었던 방법은 반복문을 2번 사용하기에 **2번 이상 순회**할 수밖에 없었는데  
Refactoring을 통해 반복문을 1번만 사용하여 풀어보았다

쉽게 설명하자면,  
`fast`를 이용하여 제거하고 싶은 노드 순서만큼(n) **미리** 이동한다  
이후 `slow`, `fast`를 함께 한 칸씩 이동시키면 fast는 Linked List 끝에 다다르게 되고  
`slow`는 미리 이동하지 못했기에 끝에서 `n + 1`의 위치에 이르게 된다  
뒤에서 `n + 1`번째 노드와 `n - 1`번째 노드 연결한다

### solve 2 Code

```js
// Runtime: 97 ms, faster than 42.51%
// Memory Usage: 42.7 MB, smaller than 64.21%
const removeNthFromEnd = function (head, n) {
  let fast = head;
  let slow = head;

  for (let i = 0; i < n; i++) {
    fast = fast.next;
  }
  if (!fast) return head.next;

  while (fast.next) {
    fast = fast.next;
    slow = slow.next;
  }
  slow.next = slow.next.next;
  return head;
};
```

## 배운 점 or 주의할 점

내가 생각한 방식으로 문제를 푸는 것과  
권장사항(challenge)대로 문제를 푸는 것은 생각보다 꽤 큰 차이가 존재한다  
조금 **더 생각**하게 되고, **다양한 방식**으로 문제를 풀 수 있게 하며, **더 효율적인** 방법을 찾게 하기 때문이다  
잼밌다
