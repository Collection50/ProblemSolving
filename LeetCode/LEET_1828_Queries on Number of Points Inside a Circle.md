# LeetCode/1828/Queries on Number of Points Inside a Circle

#배열#정수

## Want

점의 좌표 배열 `points`와  
원의 좌표, 반지름이 저장된 `queries`가 매개변수로 주어진다  
각 원 안에 존재하는 점의 개수를 배열 형태로 반환하라

## INPUT && OUTPUT

```js
/**
 * @param {number[][]} points
 * @param {number[][]} queries
 * @return {number[]}
 */
```

## Solving Strategies

점과 점 사이의 거리 공식 활용  
각 원의 중심과 해당 점의 거리가 반지름보다 작거나 같다면 원 안의 점으로 판별

### solve 1

1. `queries` 순회하며 새로운 배열 생성(`map`)
2. `isInner` 함수를 통해 두 점의 거리가 반지름보다 작은지 확인
3. `filter` 함수로 `points`를 순회하며 `isInner`가 `true`인 값들의 개수 세기
4. 반환

### solve 1 Code

```js
const isInner = (points, query) => {
  const [x1, y1] = points;
  const [x2, y2, radius] = query;
  return Math.sqrt((x2 - x1) ** 2 + (y2 - y1) ** 2) <= radius;
};

const countPoints = (points, queries) =>
  queries.map(
    (query) => points.filter((point) => isInner(point, query)).length
  );
```

## 배운 점 or 주의할 점

함수형 프로그래밍을 활용하여 해결할 수 있는 듯한  
느낌이 들어 시도해보려고 했으나 쉽지 않다  
잘 적용할 수 있도록 연습이 필요하다
