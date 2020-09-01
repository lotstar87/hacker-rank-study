# Minimum Swaps 2

사이즈가 10<sup>5</sup> 이하인 숫자의 배열을 입력 받고, 최소 자리바꿈(swap)을 통해 오름차순으로 정렬시키고 자리바꿈 횟수를 반환하는 문제이다.

## 제약조건
<var>
1 &le; n &le; 10<sup>5</sup>
</var>
<br>
<var>
1 &le; arr[i] &le; n
</var>

## Try 1.
### Code
```javascript
function swap(arr, a, b) {
    const temp = arr[a]
    arr[a] = arr[b]
    arr[b] = temp
}

function getDistanceAndSwap(arr, swapCount) {
    let swapped = false
    for(let i = 0; i<=arr.length-1; i++) {
        const distance = i + 1 - arr[i]

        if(swapped) break
        if(distance !== 0) {
            const originIdx = i - distance
            swap(arr, i, originIdx)
            swapped = true
        }
    }

    if(swapped) {
        swapCount = getDistanceAndSwap(arr, swapCount+1)
    }

    return swapCount
}

// Complete the minimumSwaps function below.
function minimumSwaps(arr) {
    return getDistanceAndSwap(arr, 0)
}
```
### Test Result
8번 테스트 케이스부터 Runtime Error가 발생

### 원인 분석
무분별한 재귀함수를 이용하여 callstack 사이즈를 초과한게 원인.
재귀함수를 제거하는 방식으로 재시도.

## Try 2.
### Code
```javascript
function swap(arr, a, b) {
  const temp = arr[a]
  arr[a] = arr[b]
  arr[b] = temp
}

function getDistanceAndSwap(arr, swapCount) {
  let i = 0;
  while(i < arr.length-1) {
    const distance = i + 1 - arr[i]
    if(distance !== 0) {
        const originIdx = i - distance
        swap(arr, i, originIdx)
        
        i = 0
        swapCount ++
    } else {
      i++
    }
  }

  return swapCount
}

// Complete the minimumSwaps function below.
function minimumSwaps(arr) {
  return getDistanceAndSwap(arr, 0)
}
```

### Test Result
모든 테스트 케이스 통과
