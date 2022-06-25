# LeetCode/344/Reverse String

#문자열

## Solving

want:
문자열이 주어진다  
문자열 내 단어 순서를 역순으로 바꾼 문자열을 반환하라  
단, 문자열이 공백으로 시작하거나 끝나면 안 되며, 2개 이상의 연속된 공백이 포함되면 안 된다  
  
INPUT: STRING  
OUTPUT: STRING  

## Understanding & Seperating
1. `split` 활용 => 각 단어를 개별 요소로 나눈다
2. `reverse` 활용 => 역순 만들기
3. `join` 활용 => 문자열화
4. `replace` 활용 => 공백처리

## Refactoring

### solve 1
`Understanding & Seperating`을 그대로 코드화 

### solve 2
더 간소화 할 수 있다고 판단  
문자열 양 끝의 공백을 제거하는 `trim`을 활용하였고  
`split` 인수에 정규 표현식을 전달하여 2개 이상의 공백으로도 나눠질 수 있도록 하였다  
따라서 `replace`를 여러 개 사용할 필요가 없어졌고 코드도 훨씬 간소화되었다  
더하여 `solve 1`에서 사용한 `word`라는 변수 없이 바로 리턴이 가능하게 하였다  

### solve 3
`for문`을 활용한 풀이  
문자열을 공백을 제거하고 단어 단위로 나눈 후에  
역순으로 배열에 넣은 후(`push`)  
`join` 을 활용해 반환하였다  

### solve 1

```js
  // Runtime: 65 ms,faster than 93.41%
  // Memory Usage: 44.2 MB, smaller than 43.29%
const reverseWords = function (s) {
  const word = s
    .split(' ')
    .reverse()
    .join(' ')
    .replace(/^\s+/g, '')
    .replace(/\s+$/g, '')
    .replace(/\s+/g, ' ');
  return word;
};
```

### solve 2

```js
  // Runtime: 68 ms, faster than 89.99%
  // Memory Usage: 44.1 MB, smaller than 51.90%
const reverseWords = function (s) {
  return s
    .trim()
    .split(/\s+/)
    .reverse()
    .join(' ');
};
```

### solve 3

```js
  // Runtime: 111 ms, faster than 23.20%
  // Memory Usage: 44 MB, smaller than 67.42%
const reverseWords = function (s) {
  const str = s.trim().split(/\s+/);
  const reversedString = [];
  for (let i = str.length - 1; i >= 0; i--) {
    if (str[i] === ' ') continue;
    reversedString.push(str[i]);
  }
  return reversedString.join(' ');
};
```


## 배운 점 or 주의할 점
문제를 보자마자 함수 몇 개가 머리속에 바로 떠올랐다  
핵심 로직이 바로 머리속에 그려졌다는 것임에도 불구하고  
`trim`의 존재를 망각하고 `replace`만 덕지덕지 붙여썼었다  
  
연장선상으로 `split`에도 정규표현식을 인수로 전달할 수 있었는데  
좋은 코드를 위해선 언어 특성을 더 잘 파악해야겠다고 여겨졌다  
그리고 바로 반환할 수 있는 것들은 바로바로 반환하자  
