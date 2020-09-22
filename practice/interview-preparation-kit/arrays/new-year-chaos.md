# New Year Chaos
놀이기구를 타기위해 줄을 서 있는데, 먼저 탑승하기 위해 뇌물을 주고 순서를 바꿀 수 있다.
한 사람당 최대 두번의 뇌물을 줄 수 있으며, 전체 뇌물을 준 횟수를 출력한다.
단, 한 사람이 최대 뇌물 허용횟수를 초과하면 "Too chaotic"을 출력한다.

## Try 1.
원래 있어야 할 위치와 현재 위치의 차이를 이용해 뇌물 횟수를 계산하려 했으나, 너무 복잡하여 실패.

## Try 2.
역순으로 재 실행하는 방법으로 재시도.

### Code
```javascript
// Complete the minimumBribes function below.
function minimumBribes(q) {
  let bribeCounts = {}
  let swapped = false
  while(true) {
    swapped = false
    for(let i = 0; i < q.length - 1; i ++) {
      const currentVal = q[i]
      const nextVal = q[i + 1]

      if(currentVal > nextVal) {
        q[i] = nextVal
        q[i + 1] = currentVal
        
        bribeCounts[currentVal] = (bribeCounts[currentVal] || 0) + 1
        if(bribeCounts[currentVal] > 2) {
          return console.log('Too chaotic')
        }

        swapped = true
      }

    }
    if(!swapped) {
      break
    }
  }
  let cnt = 0
  for(let k in bribeCounts) {
    cnt += bribeCounts[k]
  }

  console.log(cnt)
}
```

### Test Result
모든 테스트 통과

### 설명
너무 어렵게 접근하려고 했던 것이 실패 원인인 것 같다.
요구사항에 맞는 적절한 방법을 찾았어야 했는데, 무조건 for-loop를 적게 순회하려고 생각한 점이 가장 큰 실수인듯 하다.
