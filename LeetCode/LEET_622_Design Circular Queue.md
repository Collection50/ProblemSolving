# LeetCode/622/Design Circular Queue

#자료구조#큐#원형큐

## Want
원형 큐 구현 

## Understanding & Seperating
1. `k`개의 배열 선언, `undefined`로 초기화
2. index 의미하는 `front`, `rear`와 크기를 의미하는 `queueSize`, `maxSize` 선언 및 초기화
3. `enQueue(value):` 큐에 값을 집어넣고, 성공했다면 `true` 반환(아니면 `false`)  
4. `deQueue():` 큐의 값을 삭제하고, 성공했다면 `true` 반환(아니면 `false`)  
5. `Front():` 큐의 맨 앞에 있는 데이터 출력, 큐가 비었다면 `-1` 출력  
6. `Rear():` 큐의 맨 끝에 있는 데이터 출력, 큐가 비었다면 `-1` 출력  
7. `isEmpty():` 큐가 비었는지 확인(boolean)  
8. `isFull():` 큐가 꽉찼는지 확인(boolean)  

### solve 1
문제를 머릿속에서 그릴 때 `isEmpty()`, `isFull()`을 구현하는 것이 생각보다 복잡하게 구상하고 있어서 생각보다 오랜 시간이 걸렸다  
하지만 size만 비교하여 큐의 상태를 확인할 수 있다는 것을 알게 되고 굉장히 쉽게 풀렸던 문제  

```js
const MyCircularQueue = function (k) {
  this.queue = Array(k).fill(undefined);
  this.queueSize = 0;
  this.maxSize = k;
  this.front = 0;
  this.rear = k - 1;
};

MyCircularQueue.prototype.enQueue = function (value) {
  if (this.isFull()) return false;

  this.rear = (++this.rear) % this.maxSize;
  this.queue[this.rear] = value;
  this.queueSize++;
  return true;
};

MyCircularQueue.prototype.deQueue = function () {
  if (this.isEmpty()) return false;

  this.queue[this.front] = undefined; // optionable
  this.front = (++this.front) % this.maxSize;
  this.queueSize--;
  return true;
};

MyCircularQueue.prototype.Front = function () {
  return this.isEmpty() ? -1 : this.queue[this.front];
};

MyCircularQueue.prototype.Rear = function () {
  return this.isEmpty() ? -1 : this.queue[this.rear];
};

MyCircularQueue.prototype.isEmpty = function () {
  return this.queueSize === 0;
};

MyCircularQueue.prototype.isFull = function () {
  return this.queueSize === this.maxSize;
};
```

## 배운 점 or 주의할 점
기본적인 자료구조를 완전히 이해하고 그것을 코드로 풀어내는 과정에서  
컴퓨팅 사고력과 문제 해결 능력에 많은 도움이 되는 것 같다  
더 많은 자료구조와 알고리즘에 대한 이해도가 중요하게 여겨진다  