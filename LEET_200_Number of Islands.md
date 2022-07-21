# LeetCode/200/Number of Islands

#DFS

## Want

`m * n` 사이즈의 2차원 지도(`grid`)가 주어진다  
이 지도는 `'0'` or `'1'`로만 이뤄져 있다 (0 == 바다, 1 == 육지)  
지도에 있는 섬의 개수를 반환하라  
섬의 판단 기준 == 동서남북 4방향  
육지를 기준으로 4면이 바다면 섬이다

## First Understanding & Seperating

1. 재귀 호출을 위한 `helper` 함수 생성
2. 인덱스 `i`, `j`를 활용하여 `'ij'`을 키로 `set`에 저장
3. 배열의 4방향을 돌면서 현재 인덱스 `ij`의 주변이 모두 `set`에 저장되어 있거나 `0`이라면 `return`
4. 존재하지 않는다면 2, 3번 반복
5. 현재 인덱스의 값이 `1`이라면 `helper` 호출, `count++`

### solve 1

현재 인덱스를 기준으로 **우, 하, 좌, 상** 순서대로 돌면서 섬이 **_연결_**되어 있는지 확인하였다  
**재귀 함수를 호출**하여 각 인덱스의 상하좌우를 확인할 수 있도록 하였다  
`set`을 활용하여 처음 확인 하는 인덱스라면 `set`에 집어넣고, 이미 확인했다면 다음 인덱스로 진행하도록 하였다  
하지만 결과는 **_실패._**  
로직은 잘 구성한 것 같은데 섬의 개수가 세어지는 것이 아니라, 노드의 개수가 세어졌다  
이를 해결하기 위해서 정말 다양한 방법을 구상했었는데,  
예를 들어 `result` 변수를 생성해 `count /= count` 수행 후 `result++`를 해보기도 하였고  
`helper`함수의 맨 마지막 line에 `count++`를 넣기도 하였다  
하지만 모두 **동일하게 각 노드의 개수를 세는** 과정을 수행하는 것 뿐이었다.

### solve 1 Code

```js
// Fail...
const numIslands = function (grid) {
  let count = 0;
  const ZERO = '0';
  const ONE = '1';
  const set = new Set();
  const ROW_SIZE = grid.length;
  const COL_SIZE = grid[0].length;

  const helper = function (idx1, idx2) {
    if (idx1 >= ROW_SIZE || idx2 >= COL_SIZE || idx1 < 0 || idx2 < 0) return;
    if (grid[idx1][idx2] === ZERO) return;
    if (set.has(`${idx1}${idx2}`)) return;

    set.add(`${idx1}${idx2}`);
    helper(idx1, idx2 + 1);
    helper(idx1 + 1, idx2);
    helper(idx1, idx2 - 1);
    helper(idx1 - 1, idx2);
  };

  for (let r = 0; r < ROW_SIZE; r++) {
    for (let c = 0; c < COL_SIZE; c++) {
      if (grid[r][c] === ONE) {
        helper(r, c);
        count++;
      }
    }
  }
  return count;
};
```

## Refactoring

### solve 2

생각보다 **간단한 곳**에 해결 방법이 존재했다  
바로 **확인한 인덱스를 바다 처리**(`0`)하면 반복되는 노드의 개수를 구하는 것이 아니라,  
처음 섬이 시작하는 부분에서만 `count++`를 수행하여 **최종 섬의 개수**를 구할 수 있게 되는 것이다  
더하여 내부의 재귀 함수도 `findIsland`로 이름을 변경하여 더욱 더 **가독성이 좋은** 코드가 되도록 하였다

### solve 2 Code

```js
// Runtime: 68 ms, faster than 99.72 %
// Memory Usage: 44.9 MB, smaller than 70.92 %
const numIslands = function (grid) {
  let count = 0;
  const ZERO = '0';
  const ONE = '1';
  const ROW_SIZE = grid.length;
  const COL_SIZE = grid[0].length;

  const findIsland = function (idx1, idx2) {
    if (idx1 >= ROW_SIZE || idx2 >= COL_SIZE || idx1 < 0 || idx2 < 0) return;
    if (grid[idx1][idx2] === ZERO) return;

    grid[idx1][idx2] = ZERO;
    findIsland(idx1, idx2 + 1);
    findIsland(idx1 + 1, idx2);
    findIsland(idx1, idx2 - 1);
    findIsland(idx1 - 1, idx2);
  };

  for (let r = 0; r < ROW_SIZE; r++) {
    for (let c = 0; c < COL_SIZE; c++) {
      if (grid[r][c] === ONE) {
        findIsland(r, c);
        count++;
      }
    }
  }
  return count;
};
```

## 배운 점 or 주의할 점

나의 **재귀 Level**은 몇 정도일까  
재귀를 활용하는 것이, 재**귀적으로 생각하는 것**이 익숙해지기 시작했는데, 꽤 재밌고 흥미롭게 느껴진다  
재귀적으로 문제를 해결하면 굉장히 간단하고 효율적으로 풀리는 문제들을 재귀적으로 풀었을 때(지금처럼)  
굉장히 기분이 좋다  
다양한 문제 영역에서 감을 찾아가며 익숙해지도록 노력해야겠다  
**_필요한 부분에서 필요한 카드가 뽑히도록_**
