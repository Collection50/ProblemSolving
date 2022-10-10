# LeetCode/155/Min Stack

#Stack

## Want

`Stack`을 구현하라
`MinStack()`: Stack 초기화
`push(val)`: Stack에 데이터 추가
`pop()`: Stack에서 데이터 삭제
`top()`: Stack에서 맨 위에 있는 데이터 반환
`getMin()`: Stack의 최소값 반환

## First Understanding & Seperating

1. 배열을 활용하여 `Stack` 구현
2. `push` 수행 시 최소값을 어떻게 저장할 것인가? (`getMin()`을 위한 최소값 저장)
   `min` 배열을 생성하여 push된 값이 min보다 작다면 min 배열에 추가
3. `pop` 수행 시 최소값을 어떻게 비교할 것인가?
   `pop`된 값이 `min[length - 1]`값과 동일하다면 `min`도 `pop`

### solve 1

간단하게 풀 수 있는 문제라고 생각했다  
기본적인 `stack`의 구현이었고 추가로 **_최소값_** 만 판단하면 되었다  
하지만 생각해야 할 점은 `pop`할 때 **최소값의 변화 가능성**이 존재한다는 점이다  
따라서 각 `pop`, `push` 수행할 때 최소값의 변화 여부에 따라 수행하도록 하였다  
시간 복잡도 **O(N)**

### solve 1 Code

```js
// Runtime: 161 ms, faster than 57.20%
// Memory Usage: 50.2 MB, smaller than 21.68%
const MinStack = function () {
  this.stack = [];
  this.min = Infinity;
};

MinStack.prototype.push = function (val) {
  this.stack.push(val);
  this.min = this.min < val ? this.min : val;
};

MinStack.prototype.pop = function () {
  const poppedVal = this.stack.pop();
  if (poppedVal === this.min) this.min = Math.min(...this.stack);
};

MinStack.prototype.top = function () {
  const size = this.stack.length;
  return this.stack[size - 1];
};

MinStack.prototype.getMin = function () {
  return this.min;
};
```

## Refactoring

### solve 2

하지만 문제의 조건 중  
각 함수의 시간 복잡도는 O(1)이 되어야 한다고 써져있었다  
그에 따라 리팩토링을 시행했는데, `min` 배열을 생성하여  
최소값이 `min`배열의 맨 마지막 위치에 오도록 하였다  
따라서 `push`를 통해 입력되는 값이 최소값보다 작다면 `min` 배열에도 `push`하였고  
`pop`을 통해 삭제될 때도 최소값과 삭제되는 값이 같다면 `min` 배열에도 `pop`을 해주었다

### solve 2 Code

```js
// Runtime: 148 ms, faster than 67.03%
// Memory Usage: 48.7 MB, smaller than 98.93%
const MinStack = function () {
  this.stack = [];
  this.min = [];
};

/**
 * @param {number} val
 * @return {void}
 */
MinStack.prototype.push = function (val) {
  const stackSize = this.stack.length;
  const minSize = this.min.length;

  if (stackSize === 0) this.min.push(val);
  else if (this.min[minSize - 1] >= val) this.min.push(val);
  this.stack.push(val);
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function () {
  const minSize = this.min.length;
  const poppedVal = this.stack.pop();

  if (poppedVal === this.min[minSize - 1]) this.min.pop();
};

/**
 * @return {number}
 */
MinStack.prototype.top = function () {
  const stackSize = this.stack.length;
  return this.stack[stackSize - 1];
};

/**
 * @return {number}
 */
MinStack.prototype.getMin = function () {
  const minSize = this.min.length;
  return this.min[minSize - 1];
};
```

## 배운 점 or 주의할 점

문제의 **_전체적인 로직과 구성_**은 꽤 잘 짜는 듯한데  
사소한 부분에서의 **디테일**이 놓쳐지고 있는 느낌이다  
정말 수학 문제를 풀 때와 비슷하다고 여겨지는 단계인 듯한데  
어쩌면 **큰 구조**를 잡아나가면서 **디테일**을 잡는 방향으로 성장이 이뤄지지 않을까 싶다  
큰 구조를 잡고 분류하고 구분하며 문제를 이해하여 로직을 작성하고  
문제의 **요구조건, 제한사항** 등을 조금 더 유심히 보아야겠다고 느꼈다
