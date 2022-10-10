# LeetCode/232/Implement Queue using Stacks

#스택#큐

## Want

Stack 2개를 활용하여 Queue를 구현하라  
구현 시, Stack의 메서드인 `push`, `pop`, `peek`, `empty`를 활용하여 구현하라  
`push`: Queue 맨 앞에 데이터 추가  
`pop`: Queue의 맨 앞 데이터 삭제  
`peek`: Queue의 맨 앞 데이터 출력  
`empty`: Queue가 비었는지 확인  
[스택이란?](https://github.com/Collection50/Algorithm-DataStructrue/blob/master/Stack.md)
[큐란?](https://github.com/Collection50/Algorithm-DataStructrue/blob/master/Queue.md)

## First Understanding & Seperating

1. Queue 특징인 선입선출을 유지하며 삽입, 삭제를 구현해야 한다
2. `pop`할 때 먼저 나온 것이 먼저 나갈 수 있도록 하려면 어떻게 해야 할까?
3. Stack을 한 개 더 사용해 순서를 반전시키자

### solve 1

문제를 해결하는 것은 크게 어렵지 않았다  
문제 해결의 중요한 열쇠는 `pop`부분이었는데  
`pop`을 호출할 때, 저장되어있는 `stack1`의 값들을 역순으로 `stack2`에 저장한다  
그렇다면 제일 먼저 들어왔던 값이 Stack의 마지막에 저장되게 되는데,  
이후 그 값을 `Array.pop()`하여 전달해주면 되었다  
이후 역순으로 저장된 값을 다시 올바른 순서로 `stack1`에 집어 넣어주면 되었다  
**시간 복잡도 O(N)**

### solve 1 Code

```js
// Runtime: 82 ms, faster than 53.03%
// Memory Usage: 41.6 MB, smaller than 88.47%
const MyQueue = function () {
  this.stack1 = [];
  this.stack2 = [];
};

MyQueue.prototype.push = function (x) {
  this.stack1.push(x);
};

MyQueue.prototype.pop = function () {
  while (this.stack1.length) {
    this.stack2.push(this.stack1.pop());
  }
  const poppedValue = this.stack2.pop();

  while (this.stack2.length) {
    this.stack1.push(this.stack2.pop());
  }
  return poppedValue;
};

MyQueue.prototype.peek = function () {
  return this.stack1[0];
};

MyQueue.prototype.empty = function () {
  return this.stack1.length === 0;
};
```

## Refactoring

<!-- [큐]() -->

더하여 문제의 조건은 각 함수가 **Amortized O(1) time complexity**을 만족시키라고 제한 사항을 주었었다  
**_Amortized_** 시간 복잡도란?  
우리가 흔히 말하는 시간 복잡도는 **최악**의 경우를 기준으로 판단한다  
하지만 **일반적으로는 O(1)이 소요되지만 아주 가끔 O(N)이 소요**된다면 이를 O(N)으로 판단하기에는 **애매**하다고 볼 수 있다  
이러한 배경에 의해 **Amortized 시간 복잡도가 출현**하게 되었다고 보면 된다

### solve 2

`pop`, `peek`, `empty`에서의 약간의 코드 리팩토링이 수행되었다  
`pop`에서의 간단한 로직 변경도 존재한다  
몇 번의 `pop`이 발생하던간에, 데이터들이 `stack1`, `stack2`을 모두 왔다 갔다했던 `solve 1`과는 달리  
`pop`이 수행된 시점에 `stack1`에 있던 데이터들을 `stack2`로 옮긴 후  
`pop` 호출 시 `stack2`의 데이터가 모두 사라지기 전까지 `stack2`의 데이터들을 `pop`하기에  
조금이나마 더 적은 시간으로 문제를 해결할 수 있었다  
바뀐 로직에 따라 `peek`, `empty` 부분도 변경해주었다  
**시간 복잡도 O(N)**

### solve 2 Code

```js
// Runtime: 82 ms, faster than 53.03%
// Memory Usage: 41.7 MB, smaller than 79.99%
const MyQueue = function () {
  this.stack = [];
  this.reverse = [];
};

MyQueue.prototype.push = function (x) {
  this.stack.push(x);
};

MyQueue.prototype.pop = function () {
  if (!this.reverse.length) {
    while (this.stack.length) {
      this.reverse.push(this.stack.pop());
    }
  }
  return this.reverse.pop();
};

MyQueue.prototype.peek = function () {
  return this.reverse[this.reverse.length - 1] || this.stack[0];
};

MyQueue.prototype.empty = function () {
  return !this.stack.length && !this.reverse.length;
};
```

## 배운 점 or 주의할 점

새로운 시간 복잡도를 학습했다  
스택, 큐는 삽입, 삭제에서 **_O(1)의 시간 복잡도를 유지_** 해야 하는 자료구조기에  
O(N)이 소요되는 `pop`을 수정하고 싶었는데 아무리 효율적으로 작성해도 모든 함수에서 **상수** 시간 복잡도를 유지할 수 없었다  
하지만 Amortized 시간 복잡도를 학습하여 이해하니 새롭고 재밌었으며 답답함이 해결된 느낌이었다  
하지만 **의문은 여전히** 남아있었다  
O(1)을 유지하는 일반적인 스택, 큐가 아닌 Amortized 시간 복잡도를 유지하는 **자료구조를 사용하는 이유가 뭘까?**
