# Array Manipulation

0으로 채워진 사이즈가 n(<var>3 &le; n &le; 10<sup>7</sup></var>)인 배열에서 Queries에 대한 연산 최대값을 구하는 문제.

## 제약조건
<var>3 &le; n &le; 10<sup>7</sup></var> (n은 배열의 길이)
<br>
<var>1 &le; m &le; 2 * 10<sup>5</sup></var> (m은 queries의 길이)
<br>
<var>1 &le; a &le; b &le; n</var> (a는 시작 인덱스, b는 끝 인덱스)
<br>
<var>0 &le; k &le; 10<sup>9</sup></var> (k는 연산할 값)
<br>

## Try 1.
### Code 
```javascript
function arrayManipulation(n, queries) {
  const arrayOfZeros = new Array(n).fill(0)

  queries.forEach(query => {
    const [idxA, idxB, k] = query
    for(let i = idxA -1; i <= idxB -1; i++) {
      arrayOfZeros[i] += k
    }
  })

  return Math.max(...arrayOfZeros)

}
```

### Test Result
Time limit에 걸려 실패

### 원인 분석
시간 복잡도가 <var>O(m(b-a))</var>가 되는데 이를 줄이는 방향으로 재시도.

## Try 2.
### Code
```javascript
function arrayManipulation(n, queries) {
  const resultArray = new Array(n).fill(0)
  
  let max = 0

  for(let i = 0; i < queries.length; i++) {
    const query = queries[i]
    const [idxA, idxB, k] = query

    resultArray[idxA - 1] += k
    resultArray[idxB] -= k
  }

  let accVal = 0
  for(let i = 0; i < n; i ++) {
    const result = resultArray[i]
    accVal += result
    max = Math.max(max, accVal)
  }
  
    
  return max
}
```

### Test Result
모든 테스트 케이스 통과

### 설명
사실, 온갖 방법을 해보다가 결국 Google을 이용하여 컨닝을 했다.
Queries 이용해 미리 연산결과를 도출할 수 있지 않을까 싶었는데, 잘 안됐기 때문...

컨닝한 내용을 보자면, [누적합 방식(prefix sum)](https://en.wikipedia.org/wiki/Prefix_sum)을 이용하는 것인데, 연산이 이루어 져야할 경계에 값을 기록하는 방식이다.
예를 들어, [2, 4, 10], [3, 5, 10] 이라고 할 때

|0|1|2|3|4|5|
|-|-|-|-|-|-|
|0|10|0|0|-10|0|

|0|1|2|3|4|5|
|-|-|-|-|-|-|
|0|10|10|0|-10|-10|

이러한 순서로 기록을 하여, 값을 누적하여 더하면서 최대값을 저장하는 방식이다.

이번 계기를 통해 한가지 배우게 되었다.

