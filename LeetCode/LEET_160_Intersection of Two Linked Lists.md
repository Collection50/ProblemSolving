# LEET/160/Intersection of Two Linked Lists

#링크드리스트#투포인터

## Want
Linked List 2개가 주어졌을 때 (`headA`, `headb`)  
두 Linked List가 연결되는 노드를 반환하라  
쉽게 설명하자면 두 Linked List가 하나의 Linked List로 합쳐지는 노드를 구하여라  
단, 만나는 노드가 없다면 `null` 반환하며 Cycle은 존재하지 않는다  

## INPUT && OUTPUT
INPUT: Linked List Node  
OUTPUT: Linked List Node  

## First Understanding & Seperating
- 토끼와 거북이 알고리즘의 활용 문제인가?
- 그렇다면 `headA`, `headB의` 포인터가 null을 만났을 때 다시 `headA`, `headB` 값을 대입해줘보자

### solve 1
토끼와 거북이를 변형해서 사용할 수 있지 않을까?  
끝 노드에 도달 == 다시 처음으로 되돌려서 만날 때까지 해볼까?  
(_**실패**_) 연결이 없을 때 무한 루프에 빠짐  
브루트 포스를 사용하여 문제를 푼 후 리팩토링 해보자  
`시간 복잡도 O(N ** 2)`  

### solve 1 Code
```js
// Runtime: 3022 ms, OUT OF RANGE
// Memory Usage: 49.8 MB, smaller than 68.25%
const getIntersectionNode = function (headA, headB) {
  let firstListNode = headA;
  let secondListNode = headB;

  while (secondListNode) {
    while (firstListNode) {
      if (firstListNode === secondListNode) return firstListNode;
      firstListNode = firstListNode.next;
    }
    secondListNode = secondListNode.next;
    firstListNode = headA;
  }
  return null;
};
```

## Refactoring

### solve 2
문제에서 시간 복잡도 O(N)와 공간 복잡도 O(1)을 만족할 수 있는지 도발했다  
기꺼이 도발에 응하며 고민했는데, `Set`을 활용해보자고 생각했다  
`Set`의 접근(access) == 시간 복잡도 O(1)  

### solve 2 Code
```js
// Runtime: 102 ms, faster than 84.90%
// Memory Usage: 50.6 MB, smaller than 28.41%
const getIntersectionNode = function (headA, headB) {
  const nodeSet = new Set();
  let firstListNode = headA;
  let secondListNode = headB;

  while (firstListNode) {
    nodeSet.add(firstListNode);
    firstListNode = firstListNode.next;
  }
  while (secondListNode) {
    if (nodeSet.has(secondListNode)) return secondListNode;
    secondListNode = secondListNode.next;
  }
  return null;
};
```

### solve 3
이대로 마무리하기는 아쉬웠다  
공간 복잡도 O(1)을 만족시키지 못했기 떄문.  
나보다 더 좋은 풀이를 하는 사람이 있을까 싶어 찾아봤는데  
이럴수가 반복문 1번 사용 + 공간 복잡도 O(1) 만족  

#### Explantation
_**두 Linked List를 하나의 연결로 간주**_ 하여 풀이  

쉽게 설명하자면 
- `headA` 길이 + `headB` 길이 == 상수  
- `headA`, `headB`를 가리키는 포인터 `p1`, `p2` 선언  
- `while (p1 !== p2)` 조건 하, 다음 노드로 이동하며  
- `headA`를 모두 순회했다면 `p1`은 `headB`를 가리키고  
- `headB`를 모두 순회했다면 `p2`는 `headA`를 가리키게 하여 같은 노드를 가리키는지 확인  

### solve 3 Code
```js
// Runtime: 151 ms, faster than 32.33%
// Memory Usage: 49.5 MB, smaller than 89.02%
const getIntersectionNode = function (headA, headB) {
  let p1 = headA;
  let p2 = headB;

  while (p1 !== p2) {
    p1 = p1 === null ? headB : p1.next;
    p2 = p2 === null ? headA : p2.next;
  }
  return p1;
};
```

## 배운 점 or 주의할 점
Linked List 문제를 해결하면서 다양한 _**풀이 접근 방식**_ 을 배우는 것 같다  
다른 자료구조에 관한 문제를 풀 떄도 다양한 접근 방식을 배우게 될텐데  
단순히 자료구조에 대한 이해뿐만 아니라 _**사고력의 크기도 시간 복잡도 O(N!)**_ 로 증가하길
