# Programmers/H-Index

#배열

## Want

과학자의 논문의 인용 횟수 배열 citations가 매개변수로 주어진다  
이 과학자의 **H-Index**를 반환하라  
[H-Index란?](https://en.wikipedia.org/wiki/H-index)

## INPUT && OUTPUT

```js
/**
 * @param {number[]} citations
 * @return {number}
 */
```

## Solving Strategies

1. `n`개 중 `h`번 이상 인용된 논문이 `h`개 이상, `h`개 이하인 것을 찾아라
2. 내림차순 정렬
3. 해당 값이 `index`보다 작거나 같을 때의 인덱스가 H-Index

### solve 1

1. 내림차순 정렬
2. 배열 순회하며 해당 값(`value`)가 `index`보다 작을 때까지 반복
3. `index` 반환

### solve 1 Code

```js
function solution(citations) {
  const sortedNumber = citations.sort((a, b) => b - a);
  let index = 0;

  while (index + 1 <= sortedNumber[index]) {
    index++;
  }
  return index;
}
```

## 배운 점 or 주의할 점

문제를 이해하는데 시간이 조금 소요됐다  
중요점은 내림차순으로 배열해야 한다는 점과  
중간 값을 찾는 것이 아니라는 점이다

요즘 코딩테스트 경향은 이렇게 특정 개념을 알고 있어야 한다기 보다  
창의력을 요구하는 문제가 많아지는 것 같다  
하지만 방심하지 말자  
압도적 우위가 중요하다
