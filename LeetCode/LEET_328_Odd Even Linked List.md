# LEET/328/Odd Even Linked List

#링크드리스트#SinglyLinkedList

## Want

- Linked List가 주어진다
- 짝수번째에 존재하는 노드를 모두 제거한 후
- Linked List의 끝에 이어 붙인 새로운 Linked List를 반환하라
- 단, 시간 복잡도 O(N), 공간 복잡도 O(1)을 만족하라

### Constraints

노드 개수: [0, 104]

## INPUT && OUTPUT

INPUT: LINKED LIST NODE  
OUTPUT: LINKED LIST NODE

## First Understanding & Seperating

1. 짝수번째 노드라면 앞 뒤의 홀수번째 노드끼리 연결
2. 이후 짝수번째 노드(`removedNode`)를 List 끝에 이어 붙이기
3. `return`

### solve 1

1. 예외처리 수행: 노드가 0~2개인 경우 `head`를 그대로 반환하였다
2. 짝수번째 노드의 추가를 위한 `tail`을 구하면서 총 노드 개수를 의미하는 `nodeCount` 계산하였다
3. 무한 반복을 막기 위해 `iterCount`가 `nodeCount`와 같지 않다면 반복하라는 조건을 부여했다
4. 만약 짝수번째 노드라면 앞 뒤의 홀수번째 노드를 연결 + `tail` 뒤로 이어 붙였다
5. 홀수번째 노드라면 `preNode`를 `current`로 초기화 하였다
6. `head` 반환

- `nodeCount`가 1부터 시작하는 이유: 첫번째 `while`문에서 `tail.next`까지 구하므로 노드 한 개가 덜 세어졌기 때문이다
- `nextNode` 활용하여 짝수번째 노드를 `tail`에 이어붙이더라도 홀수번째 노드끼리 연결 가능하게 하였다

### solve 1 Code

```js
// Runtime: 119 ms, faster than 32.16%
// Memory Usage: 45.1 MB, smaller than 13.06%
const oddEvenList = function (head) {
  if (!head || !head.next || !head.next.next) return head;

  let current = head;
  let tail = head;
  let preNode = null;
  let nodeCount = 1;

  while (tail.next) {
    tail = tail.next;
    nodeCount++;
  }

  let iterCount = 0;
  while (nodeCount !== iterCount) {
    const nextNode = current.next;
    iterCount++;

    if (iterCount % 2 === 0) {
      const removedNode = current;
      preNode.next = removedNode.next;
      tail.next = removedNode;
      tail = tail.next;
      tail.next = null;
    } else {
      preNode = current;
    }
    current = nextNode;
  }
  return head;
};
```

## Refactoring

### solve 2

반복문을 **_한 번만 사용_** 하는 훨씬 더 간단한 방법이 존재했다  
쉽게 설명하자면, **_홀수번째 노드는 홀수번째 노드끼리_** 연결하고  
**_짝수번째 노드는 짝수번쨰 끼리 연결_** 한다  
이후 홀수번째 노드들의 마지막 노드와 짝수번째 노드들의 첫 노드를 연결하면 된다

- `while`문의 기준이 짝수번째 노드인 이유: 홀수번째 노드일 경우 총 노드의 개수가 짝수일 때 에러 발생 (`null`의 프로퍼티 참조)

### solve 2 Code

```js
// Runtime: 72 ms, faster than 95.28%
// Memory Usage: 44.3 MB, smaller than 78.27%
const oddEvenList = function (head) {
  if (!head || !head.next || !head.next.next) return head;

  const oddHead = head;
  let oddTail = head;
  const evenHead = head.next;
  let evenTail = evenHead;

  while (evenTail && evenTail.next) {
    oddTail.next = oddTail.next.next;
    evenTail.next = evenTail.next.next;
    oddTail = oddTail.next;
    evenTail = evenTail.next;
  }
  oddTail.next = evenHead;
  return oddHead;
};
```

## 배운 점 or 주의할 점

너무 정석적으로 풀이하려고 하지 않아도 될 것 같다  
**_잔머리와 꼼수_** 를 이용하면서 효과적으로 풀이하는 것이 제일 컴퓨터답게 풀이하는 방법인 것 같다
