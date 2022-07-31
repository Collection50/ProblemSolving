# LeetCode/599/Minimum Index Sum of Two Lists

#해시

## Want

음식점 이름이 문자열로 적혀있는 배열이 2개 주어진다  
두 배열에 모두 포함되어 있는 음식점을 배열 형태로 반환하라  
단, 2개 이상이 동일하게 포함되어 있을 경우, 인덱스의 합이 가장 낮은 음식점을 반환하라

## INPUT && OUTPUT

- INPUT: ARRAY
- OUTPUT: ARRAY

## First Understanding & Seperating

1. 한 배열에서 음식점 이름을 인덱스와 함께 map에 추가
2. 나머지 배열을 순회하며 각 음식점 이름이 map에 존재한다면 `restaurants`에 이름과 인덱스의 값 추가
3. 중복된 음식점 중에서 인덱스의 합이 가장 낮은 음식점 출력

고민했던 부분: `restaurants`는 객체인데 가장 작은 인덱스 합을 갖고 있는 값을 어떻게 반환할까?

### solve 1

**중복**하는 음식점 이름, 음식점의 **인덱스 합**을 모두 기억하여 활용해야 하는 문제였다  
따라서 중복을 확인하기 위한 비교 객체 하나가 필요했고 (`nameMap`)  
중복된 음식점을 저장하기 위한 객체 하나가 필요했다 (`restaurants`)  
2번째 배열 `list2`에 존재하는 음식점 이름이 `nameMap`에 존재한다면  
`restaurant` 객체에 추가될 이름과 인덱스를 `key: value` 형태로 저장하였다  
또한 추가된 인덱스 중 최소값을 찾아야 하기에 `minIndexSum`이라는 변수를 통해 가장 낮은 인덱스를 기억했다  
마지막으로 중복되는 음식점 중에서 인덱스 값이 `minIndexSum`과 동일한 값을 찾아 반환하였다  
`list1`을 순회할 때 `forEach`를 사용하였기에 일치감을 위해 `list2`에도 `forEach`를 사용하였다

### solve 1 Code

```js
// Runtime: 142 ms, faster than 56.21%
// Memory Usage: 48.3 MB, smaller than 70.75%
const findRestaurant = function (list1, list2) {
  const nameMap = new Map();
  const restaurants = new Map();
  let minIndexSum = Infinity;

  list1.forEach((name, index) => {
    nameMap.set(name, index);
  });

  list2.forEach((name, index) => {
    if (nameMap.has(name)) {
      restaurants.set(name, nameMap.get(name) + index);
      minIndexSum = Math.min(nameMap.get(name) + index, minIndexSum);
    }
  });

  return [...restaurants.keys()].filter(
    (name) => restaurants.get(name) === minIndexSum
  );
};
```

## Refactoring

### solve 2

`solve 1`과 모든 부분이 동일하지만 `list2`를 순회할 때 `for`문을 사용하였다  
이유는 `if`문에서 불필요한 들여쓰기가 사용되었다고 생각했기 때문이다  
하지만 `forEach`는 전달된 인수에 이름을 붙여줄 수 있기에 훨씬 가독성이 좋은 코드인 것 같다

### solve 2 Code

```js
// Runtime: 157 ms, faster than 42.91%
// Memory Usage: 48.9 MB, smaller than 57.09%
const findRestaurant = function (list1, list2) {
  const nameMap = new Map();
  const restaurants = new Map();
  let minIndexSum = Infinity;

  list1.forEach((name, index) => {
    nameMap.set(name, index);
  });

  for (let i = 0; i < list2.length; i++) {
    const name = list2[i];
    if (!nameMap.has(name)) continue;

    restaurants.set(name, nameMap.get(name) + i);
    minIndexSum = Math.min(nameMap.get(name) + i, minIndexSum);
  }

  return [...restaurants.keys()].filter(
    (name) => restaurants.get(name) === minIndexSum
  );
};
```

## 배운 점 or 주의할 점

**변수명**을 정하는 것이 생각보다 많은 자원이 들어간다는 것을 알았다  
단순히 `a`, `b`, `c` 등으로 대충 변수명을 작성하여도 코드는 동작하겠지만  
나는 작동만 하는 코드가 아니라, **좋은 코드로 작동시키는 엔지니어**가 되고 싶다  
**기본**에 더 충실하자
