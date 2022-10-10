# LeetCode/54/Spiral Matrix

#배열

## Want
`m * n`의 배열이 주어졌을 때, 나선형 순서로 정렬된 배열을 반환하라  

## Understanding & Seperating
1. 비어있는 새로운 배열 생성  
2. `rowStart`, `rowEnd`, `colStart`, `colEnd` 값을 사용하여 배열의 **row, col** 끝에 도달하기 전까지 배열에 `push` 
3. ****row, col**** 끝에 도달했다면 각 **start, end**값을 조절하여 나선 진행의 범위 수정
4. 중복을 제거하기 위해 `while`문 안에 각 ****row, col****의 **start, end**의 같지 않음(!==) 조건 추가  

## Refactoring

### solve 1
직관적으로 오른쪽, 아래쪽, 왼쪽, 위쪽으로 순서대로 움직이며  
배열의 맨 끝으로 이동시 각 **row, col**에 대한 범위를 축소시켜가며 나선형을 그려간 로직인데  
생각보다 신경써야 하는 부분이 많았다  
증가하고 감소하는 **row, col**의 **start, end**값을 생각해야 했고  
`while`문의 조건을 통과한 이후 같아질 **start, end**값에 대해서도 생각해야 했다  
아쉬움이 많이 남는 코드  
`시간복잡도 O(N**2)`

### solve 2
시간을 투자하며 많은 고민을 했지만, 내 머리속에서는 더 효율적인 코드가 생각나지 않았고  
다른 사람들은 어떤 방법으로 풀었는지 알고 싶었다  
몇 개의 코드 중 가장 **직관적이고 효율적**이라고 생각하는 코드를 보며 로직을 하나하나 곱씹었다  
내가 이해한 로직을 풀이하자면 이렇다  

각 진행 방향 구분, `switch`, `Circular Queue`를 적절히 활용한 방법이라고 생각했다  
그리고 `solve 1`에서의 계속적으로 변화하는 **row, col**값을 사용한 것이 아닌 size를 활용한다  
따라서 `for문`에서 반복횟수만 정해주어 오류 가능성을 줄여주는 효과와 동시에 가독성도 상승한다  
그리고 각 진행방향에 따라 r, c 값을 증감한다  
`시간복잡도 O(N**2)`

### solve 1
```js
// Runtime: 54 ms, faster than 97.19 %
// Memory Usage: 41.8 MB, smaller than 65.31 %
const spiralOrder = function (matrix) {
  const orderedArr = [];

  let rowStart = 0;
  let colStart = 0;
  let rowEnd = matrix.length;
  let colEnd = matrix[0].length;

  while (rowStart <= rowEnd - 1 && colStart <= colEnd - 1) {
    for (let i = colStart; i < colEnd; i++) {
      orderedArr.push(matrix[rowStart][i]);
    }
    rowStart++;

    for (let i = rowStart; i < rowEnd; i++) {
      orderedArr.push(matrix[i][colEnd - 1]);
    }
    colEnd--;

    if (rowStart !== rowEnd) {
      for (let i = colEnd - 1; i >= colStart; i--) {
        orderedArr.push(matrix[rowEnd - 1][i]);
      }
      rowEnd--;
    }

    if (colStart !== colEnd) {
      for (let i = rowEnd - 1; i >= rowStart; i--) {
        orderedArr.push(matrix[i][colStart]);
      }
      colStart++;
    }
  }
  return orderedArr;
};
```

### solve 2
```js
// Runtime: 62 ms, faster than 83.75 %
// Memory Usage: 42.2 MB, smaller than 24.95 %
const spiralOrder = function (matrix) {
  const orderedArr = [];
  const direcArr = ['RIGHT', 'DOWN', 'LEFT', 'UP'];
  let rowSize = matrix.length;
  let colSize = matrix[0].length;

  let r = 0;
  let c = -1;
  let directionIndex = 0;

  while (rowSize !== 0 && colSize !== 0) {
    const direction = direcArr[directionIndex];

    switch (direction) {
      case 'RIGHT':
        for (let i = 0; i < colSize; i++) {
          orderedArr.push(matrix[r][++c]);
        }
        rowSize--;
        break;
      case 'DOWN':
        for (let i = 0; i < rowSize; i++) {
          orderedArr.push(matrix[++r][c]);
        }
        colSize--;
        break;
      case 'LEFT':
        for (let i = 0; i < colSize; i++) {
          orderedArr.push(matrix[r][--c]);
        }
        rowSize--;
        break;
      default:
        for (let i = 0; i < rowSize; i++) {
          orderedArr.push(matrix[--r][c]);
        }
        colSize--;
        break;
    }
    directionIndex = (directionIndex + 1) % 4;
  }
  return orderedArr;
};
```

## 배운 점 or 주의할 점
`JavaScript-Like` 코드로 풀고싶었는데, 아쉬움이 많이 남는 문제  
그래도 배운 점이 상당히 많았던 문제다  
1. 다른 사람들의 풀이를 보니 DFS, BFS류 접근 방식으로 풀었던 사람도 보았는데  
나는 아직 그에 대한 이론적 **알고리즘 지식**을 더 배워야겠다고 생각했다  
이에 대한 보완과 공부도 필요하다고 여겨졌다  
2. 반복되는 방향에 대한 문제에서는 Circular Queue를 활용하여 푸는 것이 굉장히 매력적으로 느껴졌다  
역시 개념 **하나하나 배울 때 곱씹어서 배우면 사용하는 곳이 무궁무진**한 듯하다  
3. 나름 배열과 포인터에 대해 빠삭하게 알고 있다고 생각했지만, 실제 문제를 경험해보니 실전에서의 경험부족이 느껴졌다. 더 많은 문제를 깊게 경험해봐야겠다

난이도가 Easy인줄 알고 접근했지만, 풀고 보니 Medium이어서 당황쓰ㅋ
