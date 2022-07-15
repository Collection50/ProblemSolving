# LEET/138/Copy List with Random Pointer

#링크드리스트#SinglyLinkedList

## Want

- n개 노드를 가진 Linked List가 주어진다 (original)
- 각 노드는 추가적인 `random` pointer를 소유하며
- random pointer는 list의 노드 or `null`을 가리킨다
- Linked List의 **_deep copy_** 를 생성하고 첫 노드(head)를 반환하라
- 단, deep copy된 노드들은 original 리스트의 속성들을 전달받아야 한다 (값, random pointer)
- 예를 들어 original 노드 `X.random => Y`라면
- deep copy 노드 `x.random => y`가 되어야 한다

### Constraints

(0 <= n <= 1000)

## INPUT && OUTPUT

INPUT: LINKED LIST NODE  
OUTPUT: LINKED LIST NODE

## First Understanding & Seperating

(엇 이거 재귀, `Map`를 사용하면 괜찮겠는걸? => 풀이 실패)  
반복문 사용하는 방식으로 분석

1. original 노드가 가리키는 `random`을 파악하여 deep copy list에 그대로 적용시키는 것이 관건
2. **_배열과 반복문_** 사용, `random`이 가리키는 node의 인덱스를 파악하여 순서 형태로 기억 (`getRandomNodeIndex`)
3. 새로운 리스트를 만들어 복사할 때, 배열에 저장된 숫자만큼 반복하여 `random` 노드를 찾아 `random` 노드 값 할당 (`getRandomNode`)
4. `return`

### solve 1

구조화한 로직을 토대로 순서대로 코드를 작성하였다  
처음에 생각했던 **_재귀, Map_** 을 사용한 방식으로 풀지 못한 이후  
**_포기하지 말고_** 일단 문제를 풀어보자고 생각했고 내가 생각했던 방법은 **_시간 복잡도 O(N^2)_** 가 걸리는 방식이었다  
중첩 반복문을 사용할 수 밖에 없기에 indentation을 줄이기 위해 **외부 함수**를 따로 작성하였다  
**_로직, 가독성, 효율성 모두 아쉬움_** 이 많이 드는 코드지만 **_포기하지 않고 해결_** 해 낸 것에 좋은 점수를 주고 싶다  
시간 복잡도 O(N^2), 공간 복잡도 O(N)

### solve 1 Code

```js
// Runtime: 74 ms, faster than 80.70%
// Memory Usage: 43.6 MB, smaller than 95.97%
const getRandomNodeIndex = function (head, targetNode) {
  if (!targetNode) return null;

  let current = head;
  let count = 0;

  while (current) {
    if (current === targetNode) return count;
    current = current.next;
    count++;
  }
  return null;
};

const getRandomNode = function (head, indexArray, index) {
  if (indexArray[index] === null) return null;

  const size = indexArray[index];
  let current = head;

  for (let i = 0; i < size; i++) {
    current = current.next;
  }
  return current;
};

const copyRandomList = function (head) {
  if (!head) return;

  const randomIndexArr = [];
  let randomNodeIndex;
  let current = head;

  const newHead = new Node();
  let pointer = newHead;

  while (current) {
    randomNodeIndex = getRandomNodeIndex(head, current.random);
    randomIndexArr.push(randomNodeIndex);

    pointer.next = new Node();
    pointer = pointer.next;
    pointer.val = current.val;
    current = current.next;
  }
  pointer.next = null;

  pointer = newHead.next;
  let i = 0;

  while (pointer) {
    pointer.random = getRandomNode(newHead.next, randomIndexArr, i++);
    pointer = pointer.next;
  }
  return newHead.next;
};
```

## Refactoring

### solve 2

재귀, `Map`을 활용해 문제를 다시금 해석했다 (다른 사람의 풀이 참조)  
**_시간 복잡도 O(N)_**, 공간 복잡도 O(N)

분석한 **_핵심 로직_**

1. original 리스트(`head`)와 `newNode`를 **_한 쌍_** 으로 묶어 Map 객체에 전달하여 newNode의 `next`, `random` pointer를 효율적으로 할당
2. 재귀, `Map`을 사용하여 시간 복잡도 O(N)으로 해결 가능
3. `helper` 함수 활용하여 `map`의 반복적 선언 없이(공간 복잡도 낭비 없이) 해결

### solve 2 Code

```js
// Runtime: 116 ms, faster than 13.09%
// Memory Usage: 43.7 MB, smaller than 91.36%
const copyRandomList = function (head) {
  const map = new Map();

  function helper(node) {
    if (!node) return null;
    if (map.get(node)) return map.get(node);

    const newNode = new Node(node.val, null, null);
    map.set(node, newNode);

    newNode.next = helper(node.next);
    newNode.random = helper(node.random);
    return newNode;
  }
  return helper(head);
};
```

## 배운 점 or 주의할 점

**_C언어식_** 문제 풀이 방식이 아직도 많이 남아있는 모습을 볼 수 있다  
**_함수형, 객체지향형_** 언어를 사용하는만큼 직관적이고 효율적인, 언어가 제공하는 **_강력한 도구_** 들을 이용할 줄 알아야 하는데  
아직 C언어로 문제를 푸는 것처럼 사고가 확장된다

내가 생각한 원인

1. 문법 미숙달
2. 함수형, 객체 지향형 사고의 미숙달

1, 2번 모두 **_잡아야 할 토끼_** 들이다.

그리고 여전히 LeetCode의 Runtime 계산은 믿을 게 못 된다
