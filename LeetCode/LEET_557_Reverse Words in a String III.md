# LeetCode/557/Reverse Words in a String III

#문자열

## Solving

want:
문자열이 주어진다  
문자열의 단어 안의 알파벳 순서를 역순으로 한 문자열을 반환하라  
단, 단어와 단어 사이의 공백은 오직 1개이다
  
INPUT: STRING  
OUTPUT: STRING  

## Understanding & Seperating
1. `split` 이용하여 공백으로 단어 구분
2. `map` + `split` + `reverse` + `join` 활용
3. 결과 반환
  
INPUT: STRING  
OUTPUT: STRING

## Refactoring

### solve 1
`JavaScript-Like`  
`Understanding & Seperating`을 그대로 코드화  
`split`을 통해 문자열을 공백을 기준으로 나눈 배열이 반환된다  
`map`을 통해 각 단어들을 알파벳 단위로 나누기(`split`) + 역순으로 배열(`reverse`) + 문자열로 합쳐진 단어(`join`)로 이뤄진 배열 생성  
`join`을 통해 문자열화  

### solve 1

```js
// Runtime: 128 ms, faster than 23.18%
// Memory Usage: 48.3 MB, smaller than 73.89%
const reverseWords = function (s) {
  return s
    .split(/\s/)
    .map((word) => word.split('').reverse().join(''))
    .join(' ');
};
```

## 배운 점 or 주의할 점
재밌다
