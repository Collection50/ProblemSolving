# Programmers/JadenCase 문자열 만들기

#문자열

## Want

문자열이 주어진다  
**JadenCase**로 변경하여 반환하라  
**JadenCase**: 모든 단어의 첫 문자가 대문자이고, 그 외의 알파벳은 소문자인 문자열

`3people unFollowed me` => `3people Unfollowed Me`  
`for the last week` => `For The Last Week`

## INPUT && OUTPUT

```js
/**
 * @param {string} s
 * @return {string}
 */
```

## Solving Strategies

1. `split`으로 나누기
2. 각 요소의 첫문자는 대문자로 만들고, 나머지는 소문자로 만들어 반환
3. `map` 활용 => 각 문자에 대해 첫 문자는 대문자로, 나머진느 소문자로 만들어 반환
4. `join` 하여 `return`

### solve 1

Fail.  
첫 문자를 대문자로 바꿔준다는 로직은 맞았으나  
연속된 공백도 `map`에 요소로 평가되어 빈 문자열로 바뀐다  
이때 `''[0]`은 빈 문자열로 출력되는 것이 아니라 `undefined`가 출력된다

### solve 1 Code

```js
function solution(s) {
  return s
    .split(' ')
    .map((word) => word[0].toUpperCase() + word.substring(1).toLowerCase())
    .join(' ');
}
```

## Refactoring

`word[0]` => `word.charAt(0)` 사용

### solve 2

따라서 원하는 대로 동작하기 위해선 인덱스 접근이 아니라  
`charAt(0)`을 사용해야 했다  
빈 문자열을 빈 문자열 그대로 인식하여 반환한다

### solve 2 Code

```js
function solution(s) {
  return s
    .split(' ')
    .map(
      (word) => word.charAt(0).toUpperCase() + word.substring(1).toLowerCase()
    )
    .join(' ');
}
```

## 배운 점 or 주의할 점

대괄호를 통한 **인덱스 접근**과  
`charAt()`을 통한 접근의 차이를 알자
