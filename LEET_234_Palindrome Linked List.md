# LeetCode/234/Palindrome Linked List

#링크드리스트#투포인터

## Want

Linked List가 주어진다
Linked List가 Palindrome(회문)인지 불리언 값을 반환하라

## First Understanding & Seperating

1. 배열에 각 노드 집어 넣고
2. 처음과 끝 인덱스의 값을 비교 (`i`, `length - 1 - i`)
3. 결과 `return`

### solve 1

생각보다 **쉽게** 풀린 문제였다  
배열에 각 노드를 집어넣은 후에 반복문을 사용,  
앞에서 n번째와 뒤에서 n번째 노드가 같은지 비교하면 되는 문제였다  
`시간복잡도 O(N)`

### solve 1 Code

```js
// Runtime: 223 ms, faster than 47.78%
// Memory Usage: 78.4 MB, smaller than 41.14%
const isPalindrome = function (head) {
  const nodeArray = [];
  let current = head;

  while (current) {
    nodeArray.push(current.val);
    current = current.next;
  }

  const size = nodeArray.length;
  for (let i = 0; i < Math.floor(size / 2); i++) {
    if (nodeArray[i] !== nodeArray[size - i - 1]) return false;
  }
  return true;
};
```

## Refactoring

### solve 2

하지만 문제의 조건에서 시간 복잡도 O(N)에 더불어 **공간 복잡도 O(1)**을 만족시키라는 권장사항이 존재했다  
공간 복잡도 O(₩)을 만족하기 위해선,  
추가적인 메모리 사용 없이 **포인터만 이용**하여 문제를 풀어야만 했다  
여기서도 사용된 풀이 방법은 [토끼와 거북이 알고리즘](https://github.com/Collection50/Algorithm-DataStructrue/blob/master/Tortoise%20and%20Hare.md)

### Explantation

길이 `N`에서 **토끼는 2걸음**, **거북이는 1걸음**씩 나아간다면 토끼가 맨 끝(`N`)에 닿을 때 거북이는 정확히 `N/2` 위치에 존재한다  
이후 맨 끝에 도착한 토끼를 다시 맨 처음으로 보내고 (`fast = head`)  
그다음 중간에 위치하는 거북이를 맨 끝(`N`)으로 이동시키면서 절반의 Linked List를 reverse한다 (**역방향 가리키기**)  
현재 토끼는 맨 왼쪽에, 거북이는 맨 오른쪽에 존재하게 되고  
반복문을 이용해 거북이와 토끼가 한칸씩 움직이며 현재 노드의 값을 비교하며 회문 여부를 확인한다

### solve 2 Code

```js
// Runtime: 241 ms, faster than 37.25 %
// Memory Usage: 69.2 MB, smaller than 85.94 %
const isPalindrome = function (head) {
  let slow = head;
  let fast = head;
  let prev = null;
  let nextNode = null;

  while (fast && fast.next) {
    slow = slow.next;
    fast = fast.next.next;
  }
  prev = slow;
  slow = slow.next;
  prev.next = null;

  while (slow) {
    nextNode = slow.next;
    slow.next = prev;
    prev = slow;
    slow = nextNode;
  }
  fast = head;
  slow = prev;

  while (slow) {
    if (fast.val !== slow.val) return false;
    fast = fast.next;
    slow = slow.next;
  }
  return true;
};
```

## 배운 점 or 주의할 점

투포인터는 시간 복잡도, 공간 복잡도를 **_줄이면서_** 문제를 굉장히 **_효율적_** 으로 해결할 수 있는 방법인 듯하다  
꽤 **매력적**으로 느껴지는 알고리즘 풀이 방법인데, 내가 **사용하고 싶을 때 사용할 수 있는** 수준으로 만들어 놓는다면 꽤 유용하게 사용할 것 같다  
큰 시간 복잡도, 공간 복잡도를 차지해야만 하는 문제에서도 획기적으로 복잡도를 단축하며 풀이할 수 있기 때문이다
