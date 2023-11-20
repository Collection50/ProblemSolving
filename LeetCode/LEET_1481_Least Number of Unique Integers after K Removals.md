# Leetcode/1481/Least Number of Unique Integers after K Removals

#구현

## Want

정수 배열 `arr`와 정수 `k`가 주어진다  
`k`개의 요소를 제거한 후 고유한 정수의 최소 개수를 반환하라  
[문제 링크](https://leetcode.com/problems/least-number-of-unique-integers-after-k-removals/description/)

## INPUT && OUTPUT

```js
/**
 * @param {number[]} arr
 * @param {number} k
 * @return {number}
 */
```

## Solving Strategies

`k`개를 삭제한 뒤의 정수의 개수가 최소값이 되도록 하라  
그러니까 중복을 더 많이 만들어야 하는거네  
빈도수가 적은 것들을 없애야??  
시간 복잡도 O(NlogN)

### solve 1

1. 각 숫자와 개수를 객체에 저장
2. 개수가 적은 순서대로 정렬
3. 정렬된 배열 순회
4. `k`개만큼 숫자 개수 삭제
5. 만약 개수가 `0`이라면 삭제
6. `return keys.length`

### solve 1 Code

```js
const map = (arr) =>
  arr.reduce((map, num) => map.set(num, (map.get(num) || 0) + 1), new Map());

const findLeastNumOfUniqueInts = function (arr, k) {
  const numMap = map(arr);
  const sortedNums = [...numMap.entries()].sort((a, b) => a[1] - b[1]);

  for (const [num, count] of sortedNums) {
    if (k < count || k <= 0) {
      break;
    }
    numMap.delete(num);
    k -= count;
  }
  return [...numMap.keys()].length;
};
```

## 배운 점 or 주의할 점

Easy Peasy

1. `Map.prototype.set`의 반환값은 `Map`이다
2. `Map.prototype.entries`, `Map.prototype.keys`, `Map.prototype.values`의 반환값은 **이터러블**이다.
   - 배열 아님
