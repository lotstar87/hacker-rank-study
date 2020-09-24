# Sherlock and Anagrams
주어진 단어에 몇개의 Anagram이 존재하는지 숫자를 세는 문제.

## 제약조건
<var>1 &le; q &le; 10</var>
<br>
<var>2 &le; |s| &le; 100</var>
<br>
<var>s</var>는 소문자 <var>&isin;</var> ascii[a-z]</var>


## Try 1.
가능한 경우의 수를 {길이: 조각[]} 형태로 Map을 만들고, Map을 순회하면서 서로를 비교하여 Anagram인지 파악.
Anagram을 파악하기 위해 두 단어를 오름차순으로 정렬하고 비교.

### Code
```js
function sherlockAndAnagrams(s) {
  const lengthChunksMap = {}

  for(let i = 0; i < s.length - 1; i ++) {
    const chunks = []
    for(let y = 0; y < s.length - i; y ++) {
      const chunk = s.split('').splice(y, i+1).join('')
      chunks.push(chunk)
    }
    lengthChunksMap[i+1] = chunks
  }

  let anagrams = 0
  for(let length in lengthChunksMap) {
    const chunks = lengthChunksMap[length]
    for(let i = 0; i < chunks.length - 1; i ++) {
      for(let y = i + 1; y < chunks.length; y ++) {
        if(chunks[i].split('').sort().join('') === chunks[y].split('').sort().join('')) anagrams ++
      }
    }
  }

  return anagrams
}
```

### Test Result
4번 테스트 항목에서 시간초과 발생.

### 분석
Anagram 여부를 파악하기 위해 각 경우의 수에 대해 오름차순 정렬하는 과정에서 for-loop을 많이 순회하는 것 때문으로 파악됨.


## Try 2.
가능한 경우의 수를 {길이: 조각[]} 형태로 Map을 만들고, Map을 순회하면서 각 조각에 대해 {문자: 개수} 형태의 Map을 추가적으로 생성한 후 모든 조각이 Map에 포함되는지 확인하는 방법으로 진행.

### Code
```js
function sherlockAndAnagrams(s) {
  const lengthChunksMap = {}

  for(let i = 0; i < s.length - 1; i ++) {
    const chunks = []
    for(let y = 0; y < s.length - i; y ++) {
      const chunk = s.split('').splice(y, i+1).join('')
      chunks.push(chunk)
    }
    lengthChunksMap[i+1] = chunks
  }

  let anagrams = 0
  for(let length in lengthChunksMap) {
    const chunks = lengthChunksMap[length]
    for(let i = 0; i < chunks.length - 1; i ++) {
      const originMap = new Map()

      for(let ii = 0; ii < length; ii ++) {
        if(!originMap.has(chunks[i][ii])) originMap.set(chunks[i][ii], 0)
        originMap.set(chunks[i][ii], originMap.get(chunks[i][ii]) + 1)
      }

      for(let y = i + 1; y < chunks.length; y ++) {
        const map = new Map(originMap)
        for(let yy = 0; yy < length; yy ++) {
          if(map.has(chunks[y][yy])) map.set(chunks[y][yy], map.get(chunks[y][yy]) - 1)
          if(map.get(chunks[y][yy]) === 0) map.delete(chunks[y][yy])
        }

        if(map.size === 0) anagrams ++
      }
    }
  }

  return anagrams
}
```

### Test Result
모든 테스트 통과
