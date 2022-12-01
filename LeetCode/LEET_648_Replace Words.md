# LeetCode/648/Replace Words

#문자열

## Want

긴 단어를 형성하기 위해 어근 뒤에는 단어들이 덧붙여질 수 있다  
뒤따르는 단어를 `successor`라고 칭한다  
예를 들어 어근 `an` 뒤에 후속 단어 `other`가 오면  
새로운 단어 `another`를 형성할 수 있다

많은 어근으로 구성된 사전과  
문장이 주어지면 문장의 모든 `successor`를 그것을 형성하는 어근으로 대체하라  
두 개 이상의 어미로 대체할 수 있다면 길이가 짧은 루트로 대체한다

## INPUT && OUTPUT

```js
/**
 * @param {string[]} dictionary
 * @param {string} sentence
 * @return {string}
 */
```

## Solving Strategies

1. 공백으로 구분된 문자열을 `split`
2. 문자열의 각 단어에 대해 순회
3. `dictionary` 요소들로 시작하게 되는 단어가 있는지 확인
4. `true`라면 `successor`만큼만 자르기
5. `false`라면 자르지 않고 그대로 반환
6. 각 어미로 바꿔진 문자열 배열 => 문자열로 변환(`join(' ')`)

### solve 1

`Solving Strategies`의 코드화  
**시간 복잡도 O(N^2)**

### solve 1 Code

```js
// Runtime: 2591 ms, faster than 5.29%
// Memory Usage: 75.7 MB, less than 10.58%
const replaceWords = function (dictionary, sentence) {
  return sentence
    .split(' ')
    .map((word) =>
      dictionary.reduce(
        (replacedWord, successor) =>
          new RegExp(`^${successor}`).test(word)
            ? replacedWord.slice(0, successor.length)
            : replacedWord,
        word
      )
    )
    .join(' ');
};
```

## 배운 점 or 주의할 점

**O(N^2)**이라는 굉장히 오랜 시간이 소요된다
다른 사람들의 풀이를 참고하니, `Trie`를 사용하여서 해결하는 모습을 볼 수 있다  
아직 나에겐 꽤 어려운 접근방법이었다  
하지만 언젠가는 마주쳐야 하겠지  
쇠뿔도 단김에 빼자
