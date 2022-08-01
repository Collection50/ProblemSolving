# LeetCode/347/Top K Frequent Elements

#해시

## Want

정수 배열과 정수 k가 주어진다  
정수 배열에서 가장 빈도수가 높은 k개의 정수를 배열 형태로 반환하라

## INPUT && OUTPUT

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
```

## First Understanding & Seperating

1. 배열을 순회하면서 각 개수를 세어 객체에 저장
2. `Object.keys()`와 `sort` 사용하여 빈도수가 가장 높은 k개 배열 출력

### solve 1

문제를 이해한 후 로직을 머리속에서 그리니 어렵지 않게 풀 수 있었던 문제였다  
각 정수의 빈도수를 저장하고 내림차순으로 정렬한 후 `k`개를 잘라 반환하면 되었다  
이 과정에서 객체의 키 값을 배열로 반환하는 `Object.keys()`  
내림차순 정렬을 수행하는 `sort()`  
`k`개를 잘라낸 값을 반환하는 `splice()`를 사용하였다  
**_시간 복잡도 O(N^2)_**

### solve 1 Code

```js
const topKFrequent = function (nums, k) {
  const map = {};

  for (const num of nums) {
    map[num] = map[num] + 1 || 1;
  }

  return Object.keys(map)
    .sort((a, b) => map[b] - map[a])
    .splice(0, k);
};
```

## Refactoring

### solve 2

문제의 조건에서 **시간 복잡도 O(N \* logN)보다 더 적은** 시간이 소요되어야 한다고 적혀있었다  
이에 대해 고민했지만, 내 머리속에서는 `solve 1`보다 효율적인 코드가 떠오르지 않았었다  
따라서 훌륭한 답변을 완전히 이해했고 이를 설명하자면 아래와 같다  
(**bucket sort** 활용하여 문제를 해결)
**시간 복잡도 O(N)**

- 1번째 반복문: 각 정수의 빈도수 저장
- 2번째 반복문: 각 정수의 **빈도수 == 인덱스화**, 배열의 해당 인덱스에 `Set` 객체 추가하여 빈도수가 같은 정수끼리 저장
- 3번째 반복문: 빈도수 == 인덱스이기에 **역순**으로 배열을 순회하며 `result`에 추가(`push`)  
   이후 `result`에 `k`개만큼 저장되었다면 `break` (역순으로 순회해야 **빈도수가 높은 순으로 `result`에 저장**할 수 있음)

### solve 2 Code

```js
const topKFrequent = function (nums, k) {
  const map = new Map();
  const bucket = [];
  const result = [];

  for (const num of nums) {
    map.set(num, (map.get(num) || 0) + 1);
  }

  for (const [num, freq] of map) {
    bucket[freq] = (bucket[freq] || new Set()).add(num);
  }

  for (let i = bucket.length - 1; i >= 0; i--) {
    if (bucket[i]) result.push(...bucket[i]);
    if (result.length === k) break;
  }
  return result;
};
```

## 배운 점 or 주의할 점

어찌보면 `solve 1`이 가장 `JavaScript-Like` 코드이다  
하지만 알고리즘에서 가장 중요한 것은 시간이므로, 알고리즘 분야에 있어서 효율적인 코드는 아니기에 더 효율적인 코드를 고민해야겠다  
더하여 `solve 2`의 `for...of` 사용 시  
`Map` 객체에 **배열 디스트럭처링**을 사용하면 **키: 값** 형태로 값을 받을 수 있는 것을 발견했다
