# LeetCode/240/Search a 2D Matrix II

#배열#탐색

## Want

m \* n 형태의 2차원 정수 행렬과 `target`이 주어진다
각 열은 오름차순 형태이다
각 행은 오름차순 형태이다
행렬 내에 `target`의 존재 여부를 확인하라

## INPUT && OUTPUT

```js
/**
 * @param {number[][]} matrix
 * @param {number} target
 * @return {boolean}
 */
```

## Solving Strategies

1. 완전 탐색 (브루트 포스)
2. 이분 탐색

위와 같은 방법으로 해결할 수 있을 것 같은데?  
**완전 탐색**: 해시를 활용한 풀이
**이분 탐색**: 어떻게 활용할 수 있을까? pivot은?

### solve 1

완전 탐색을 활용하는 것은 굉장히 쉬웠다  
반복문을 활용하여 해결할 수도 있겠지만, 해시를 사용하는 편이 훨씬 가독성이 좋았다  
하지만, 제한 시간이 존재했는지 AC를 받지는 못했다  
**시간 복잡도 O(N)**

### solve 1 Code

```js
const searchMatrix = function (matrix, target) {
  return new Set(matrix.flat()).has(target);
};
```

### solve 2

시간을 조금 더 줄일 수 있는 방법을 찾기 시작했고 고민했다  
이분 탐색을 활용하여 문제를 해결하고 싶은데 어떻게 해결할까 고민하다가  
각 행, 열이 오름차순이므로 **각 행에 대한 이분 탐색을 열의 개수만큼 반복**하면 시간을 단축시킬 수 있다고 판단했다  
행: N  
열: M

기본적인 이분 탐색 방법을 사용하기 위해 `binarySearch` 함수를 생성해 활용했고  
배열 고차 함수 `some`을 활용하여 각 행에 `target`이 존재하는지 확인하여 반환하였다

**시간 복잡도 (logN \* M)**

### solve 2 Code

```js
// Runtime: 3326 ms, faster than 30.83%
// Memory Usage: 44.9 MB, smaller than 71.29%
const binarySearch = (array, target) => {
  let start = 0;
  let end = array.length - 1;

  while (start <= end) {
    const mid = Math.floor((start + end) / 2);

    if (array[mid] < target) start = mid + 1;
    else if (array[mid] > target) end = mid - 1;
    else return mid;
  }
  return -1;
};

const searchMatrix = function (matrix, target) {
  const FALSE = -1;
  return matrix.some((row) => binarySearch(row, target) !== FALSE);
};
```

### solve 3

꽤 괜찮은 방법이라고 생각했었는데, 상위 70%라는 숫자가 아직 아쉬웠다  
더하여 문제의 조건인 각 행, 열은 오름차순으로 정렬된다는 것을 제대로 활용하지 못한 느낌이었다  
따라서 다른 사람의 풀이를 참조하였는데, 문제의 조건을 아주 잘 활용하여 빠르게 해결하였다  
오름차순을 활용하여, 탐색의 시작을 오른쪽 끝에서 시작한다  
`target < num`이라면 `col--`  
`target > num`이라면 `row++`를 수행하여 탐색을 실시하는 방법이다  
직관적이라고 생각이 들지는 않았는데, 익혀두면 사용할 수 있는 곳이 있는 풀이 방법인 것 같다

### solve 3 Code

```js
const searchMatrix = function (matrix, target) {
  let row = 0;
  let col = matrix[0].length - 1;

  while (row < matrix.length && col >= 0) {
    const num = matrix[row][col];
    if (num === target) return true;
    if (num < target) row++;
    else col--;
  }
  return false;
};
```

## 배운 점 or 주의할 점

보통 탐색이 어려운 유형에 속한다고 알고 있다  
자료구조에 따라, 자료구조의 정렬 상태에 따라, 탐색 방법에 따라 다른 방법을 사용해야 하기 때문일까?  
그런데 어찌보면 우리 생활과 밀접한 것이 탐색이지 않을까 싶다  
문자열 검색, 결과 검색, 데이터 검색 등이 우리 생활과 밀접하게 맞닿아 있으니 말이다  
잘 알아두자
