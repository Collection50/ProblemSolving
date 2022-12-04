# Programmers/Title

#문자열

## Want

덧셈, 뺄셈 수식들이  
`X [연산자] Y = Z` 형태로 들어있는 문자열 배열 quiz가 매개변수로 주어진다  
수식이 옳다면 `O`를 틀리다면 `X`를 순서대로 담은 배열을 반환하라

## INPUT && OUTPUT

```js
/**
 * @param {string[]} quiz
 * @return {string[]}
 */
```

## Solving Strategies

입력되는 문자열은 곧 코드다  
문자열로 이뤄진 코드를 해석하는 `eval` 함수 사용

### solve 1

`Solving Strategies` 코드화

### solve 1 Code

```js
function solution(quiz) {
  return quiz.map((expression) => {
    const [left, right] = expression.split('=');
    return eval(left) === +right ? 'O' : 'X';
  });
}
```

## 배운 점 or 주의할 점

포기하지 말자
