# Leetcode/No/Title

#구현

### solve 1

### solve 1 Code

```js
const direction = [
  [0, 1],
  [-1, 1],
  [-1, 0],
  [-1, -1],
  [0, -1],
  [1, -1],
  [1, 0],
  [1, 1]
];

function countLiveCellCount(board, r, c) {
  let liveCellCount = 0;

  for (const [row, col] of direction) {
    const nr = row + r;
    const nc = col + c;

    if (board?.[nr]?.[nc] === 1 || board?.[nr]?.[nc] === 3) {
      liveCellCount += 1;
    }
  }
  return liveCellCount;
}

const gameOfLife = (board) => {
  for (let r = 0; r < board.length; r++) {
    for (let c = 0; c < board[0].length; c++) {
      const liveCellCount = countLiveCellCount(board, r, c);

      if (board[r][c] === 1) {
        if (liveCellCount < 2 || liveCellCount > 3) {
          board[r][c] = 3;
        }
      } else {
        if (liveCellCount === 3) {
          board[r][c] = 2;
        }
      }
    }
  }

  for (let r = 0; r < board.length; r++) {
    for (let c = 0; c < board[0].length; c++) {
      if (board[r][c] === 2) {
        board[r][c] = 1;
      } else if (board[r][c] === 3) {
        board[r][c] = 0;
      }
    }
  }
};
```
