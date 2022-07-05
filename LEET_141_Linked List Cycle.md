# LeetCode/141/Linked List Cycle

#링크드리스트#투포인터

## Want
Linked List가 주어진다  
Cycle의 시작 노드를 반환하라  
만약 Cycle이 없다면 `null` 반환  
단, Linked List를 수정하면 안 된다  

## Understanding & Seperating
[`Tortoise & Hare`](https://github.com/Collection50/Algorithm-DataStructrue/blob/master/Tortoise%20and%20Hare.md) 사용
1. Cycle 여부 확인
2. Cycle 존재한다면 Cycle 시작 노드 찾기
3. 반환

## Refactoring

### solve 1
Linked List의 Cycle(순환) 여부를 확인할 수 있는 **토끼와 거북이 알고리즘**을 새로 배우며 문제 해결  
(알고리즘에 대한 설명은 위 링크 참조)

### solve 2
Cycle 여부를 확인하는 `hasCycle()` 내부 함수를 만들어 해결 

### solve 1
```js
// Runtime: 90 ms, faster than 80.13%
// Memory Usage: 44.5 MB, 91.70%
const detectCycle = function (head) {
  let tort = head;
  let hare = head;
  let hasCycle = false;

  while (hare && hare.next && !hasCycle) {
    tort = tort.next;
    hare = hare.next.next;
    if (hare === tort) hasCycle = true;
  }
  if (!hasCycle) return null;

  tort = head;
  while (hare !== tort) {
    tort = tort.next;
    hare = hare.next;
  }
  return hare;
};
```

### solve 2
```js
// Runtime: 85 ms, faster than 87.06%
// Memory Usage: 44.8 MB, 78.56%
const detectCycle = function (head) {
  const hasCycle = (linkedList) => {
    let tort = linkedList;
    let hare = linkedList;
    while (hare && hare.next) {
      tort = tort.next;
      hare = hare.next.next;
      if (hare === tort) return [tort, hare, true];
    }
    return [tort, hare, false];
  };

  let [tort, hare, isCycle] = hasCycle(head);
  if (!isCycle) return null;

  tort = head;
  while (hare !== tort) {
    tort = tort.next;
    hare = hare.next;
  }
  return hare;
};
```

## 배운 점 or 주의할 점
배열의 투포인터는 익숙하게 생각하고 있었는데  
링크드 리스트에 투포인터 개념이 존재한다고 하니 생각보다 새롭게 여겨졌다  
탐색할 때, 투포인터 개념이 여기저기 출몰하는데 조금 더 익숙해져야겠다고 생각했고  
문제를 보고 **어떤 알고리즘을 사용할 지 머릿속에서 분류**할 수 있는 능력이 조금 더 필요하다고 느꼈다
