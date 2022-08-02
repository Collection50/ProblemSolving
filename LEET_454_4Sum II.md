# LeetCode/454/4Sum II

#해시#배열

## Want

길이가 n으로 동일한 정수 배열 4개가 주어진다
각 정수 배열의 요소 1개씩 더해 0이 되는 조합의 개수를 반환하라

## INPUT && OUTPUT

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @param {number[]} nums3
 * @param {number[]} nums4
 * @return {number}
 */
```

## First Understanding & Seperating

1. 앞의 배열 2개의 요소를 각각 조합한 합, 개수를 키:값 형태로 객체에 저장 (`nums1, nums2`)
2. 뒤의 배열 2개의 요소를 각각 조합하여 합을 구한다 (`nums3, nums4`)
3. 2번에서 구한 값 \* -1의 값이 객체에 존재한다면 값만큼 zeroCount에 저장
4. `return`

### solve 1

문제는 각 배열에 존재하는 값을 1개씩 더하여 0이 되는 개수를 요구하고 있다  
**순서 상관없이** 합이 0이 되는 요소들의 **조합**을 찾으면 된다  
반복문을 통하여 `nums1`, `nums2`을 순회하며 1개씩 중복없이 더한 값으로 객체의 키를 생성한다  
이를 반복하여 같은 키가 나오는 것들의 개수를 세어준다 (값에 + 1)  
나머지 `nums3`, `nums4`을 순회하며 1개씩 중복없이 더한 값 \* -1 값이 객체의 키로 존재한다면  
이 값을 더했을 때 0이 되므로 개수(객체의 값)를 `zeroCount`에 더해준다  
순서는 상관이 없으므로 `nums3`, `nums4`의 값이 객체에 키로 존재할 때마다 값을 더한다  
시간 복잡도 O(N^2)

### solve 1 Code

```js
const fourSumCount = function (nums1, nums2, nums3, nums4) {
  const map = {};
  let zeroCount = 0;
  const size = nums1.length;

  for (let i = 0; i < size; i++) {
    for (let j = 0; j < size; j++) {
      const sum = nums1[i] + nums2[j];
      map[sum] = map[sum] + 1 || 1;
    }
  }

  for (let i = 0; i < size; i++) {
    for (let j = 0; j < size; j++) {
      zeroCount += map[(nums3[i] + nums4[j]) * -1] || 0;
    }
  }
  return zeroCount;
};
```

## 배운 점 or 주의할 점

해결하기까지 꽤 시간이 소요되었다  
자력으로 해결하지 못한 문제였기에 많은 고민 후 다른 사람의 풀이를 찾아보았는데 생각보다 **허탈하고 아쉬웠다**  
조합을 사용하기 위해 재귀, dfs 등을 생각하며 풀리지 않아 답답했던 문제였는데  
알고리즘과 같은 **개념적 접근 방법이 아닌 사고력을 요하는 문제**였던 것 같다  
모든 값을 탐색하면서(Brute Force) 합이 0이되는 경우를 찾아주면 되는 문제였고  
해시를 사용하여 반복 횟수를 줄여주는 문제였다고 생각한다
