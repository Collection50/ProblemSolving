# LeetCode/141/Linked List Cycle

#링크드리스트#투포인터

## Want
Linked List가 주어진다  
Linked List의 순환 여부를 확인하라

## Understanding & Seperating
[`Tortoise & Hare`](https://github.com/Collection50/Algorithm-DataStructrue/blob/master/Tortoise%20and%20Hare.md) 사용
1. Cycle 여부 반환 (`Boolean`)

### solve 1
Linked List의 Cycle(순환) 여부를 확인할 수 있는 **토끼와 거북이 알고리즘**을 새로 배우며 문제 해결  
(알고리즘에 대한 설명은 위 링크 참조)

### solve 1 Code
```js
// Runtime: 123 ms, faster than 26.23%
// Memory Usage: 45.4 MB, smaller than 28.95%
const hasCycle = (head) => {
  let tort = head;
  let hare = head;
  while (hare && hare.next) {
    tort = tort.next;
    hare = hare.next.next;
    if (hare === tort) return true;
  }
  return false;
};
```

## 배운 점 or 주의할 점
배열의 투포인터는 익숙하게 생각하고 있었는데  
링크드 리스트에 투포인터 개념이 존재한다고 하니 생각보다 새롭게 여겨졌다  
탐색할 때, 투포인터 개념이 여기저기 출몰하는데 조금 더 익숙해져야겠다고 생각했고  
문제를 보고 **어떤 알고리즘을 사용할 지 머릿속에서 분류**할 수 있는 능력이 조금 더 필요하다고 느꼈다
