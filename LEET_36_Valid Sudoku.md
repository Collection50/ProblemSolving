# LeetCode/36/Valid Sudoku

#해시#배열

## Want

9 \* 9 크기의 스도쿠 보드가 주어진다  
유효한 스도쿠 보드인지 확인하라

**유효 조건**

1. 각 행에 존재하는 숫자는 중복이 존재할 수 없습니다.
2. 각 열에 존재하는 숫자는 중복이 존재할 수 없습니다.
3. 3 \* 3의 미니 보드에 존재하는 숫자도 중복이 존재할 수 없습니다.

## INPUT && OUTPUT

- INPUT: ARRAY
- OUPUT: BOOLEAN

## First Understanding & Seperating

1. 1 ~ 9까지 숫자의 중복 여부를 확인할 수 있는 객체 생성
2. 반복마다 확인 객체 초기화하여 각 행, 열 검사
3. 만약 중복되는 것이 하나라도 존재한다면 `return false`
4. 2, 3번이 모두 참이라면 미니 보드 검사 (sub board)
   각 보드의 시작인 (0,0), (0,3), (0,6), (3,0), (3,3), (3,6), (6,0), (6,3), (6,6) 검사
   객체를 활용하여 9개 검사
5. `return`

### solve 1

**시간 복잡도 O(N^2)**로 풀 수밖에 없는 문제  
효율적인 방법을 위해 많은 생각을 해도, 모든 행, 열의 값을 확인해야만 했다  
로직은 간단했다 `Set`을 활용하여 **중복 여부를 검사**하여 반환 값을 정해주면 되었다  
행, 열을 검사하는 `for`문을 1개로 줄여 작성하면 조금이나마 시간을 단축할 수 있었지만  
**가독성, 유지 보수성 향상**을 위해 2개의 반복문으로 분리하였다  
더하여 미니 보드의 중복 여부를 확인할 때는 **들여쓰기의 depth를 적게 유지**하기 위해  
`isValidSub`라는 함수를 따로 분리하여 작성하였다

### solve 1 Code

```js
const isValidSub = function (board, row, col) {
  const THREE = 3;
  const set = new Set();

  for (let r = row; r < row + THREE; r++) {
    for (let c = col; c < col + THREE; c++) {
      const value = board[r][c];

      if (value === '.') continue;
      if (set.has(value)) return false;

      set.add(value);
    }
  }
  return true;
};

const isValidSudoku = function (board) {
  const size = board.length;
  let subValid = true;
  const THREE = 3;

  // checkRow
  for (let row = 0; row < size; row++) {
    const set = new Set();
    for (let col = 0; col < size; col++) {
      const value = board[row][col];

      if (value === '.') continue;
      if (set.has(value)) return false;

      set.add(value);
    }
  }

  // checkCol
  for (let col = 0; col < size; col++) {
    const set = new Set();
    for (let row = 0; row < size; row++) {
      const value = board[row][col];

      if (value === '.') continue;
      if (set.has(value)) return false;

      set.add(value);
    }
  }

  // checkSub
  for (let row = 0; row < size; row += THREE) {
    for (let col = 0; col < size; col += THREE) {
      subValid = isValidSub(board, row, col);
      if (!subValid) return false;
    }
  }

  return true;
};
```

## 배운 점 or 주의할 점

생각을 해야 한다  
문제를 풀기 전, 풀면서, 풀고 난 후 그저 AC를 바라보며 좋아할 것이 아니라  
어떤 코드가 더 좋은 코드인지, 효율성을 더 증가시킬 방법은 없는지 생각하고 고민하여야 한다  
좋은 개발자의 판가름은 어떤 생각하여 코드로 연결시키는 지가 중요할 듯하다
