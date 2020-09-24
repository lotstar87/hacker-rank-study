# Ransom Note
Magazine 단어들을 이용하여 Note 단어를 재 구성할 수 있는가에 대한 문제.

## 제약조건
<var>1 &le; n, m &le; 3000</var>
<br>
<var>1 &le; |magazine[i]|, |note[i]| &le; 5</var>
<br>
각 단어는 알파벳으로 구성되어 있다(i.e., <var>a</var> to <var>z</var> and <var>A</var> to <var>Z</var>).
<br>

## Try 1.
Note의 단어를 하나씩 꺼내 Magazine의 단어중 일치하는 것을 찾고 제외시키는 방법으로 접근.
만약, Note의 단어를 Magazine에서 찾지 못하면 재구성 할 수 없으므로 No를 출력하면 될 것으로 보였음.

### Code
```javascript
function checkMagazine(magazine, note) {
  let usedOnlyMagazine = true

  while(note.length) {
    const n = note.shift()

    let foundIdx = magazine.findIndex(m => m === n)
    if(foundIdx === -1) {
      usedOnlyMagazine = false
      break
    }

    const m = magazine.splice(foundIdx, 1)
  }

  console.log(usedOnlyMagazine ? 'Yes': 'No')
}
```

### Test Result 
모든 테스트 통과

### 분석
Dictionary and Hash Table 단원인데 배열만을 가지고 풀었으므로, 해당 방법을 이용해 다시 풀어보기로 함.

## Try 2.
Magazine의 단어들을 {단어: 개수}가 되도록 Map에 저장하고, Note의 단어를 하나씩 꺼내 Map에 존재하는지 확인한다.
Map에 존재하면 개수를 하나씩 감소시키고 0개가 되면 Map에서 단어를 삭제.

위를 반복하다가, Note의 단어가 Map에 존재하지 않으면 재구성할 수 없다는 의미이므로 No를 출력하고, 아니라면 Yes를 출력한다.

### Code
```js
function checkMagazine(magazine, note) {
  let usedOnlyMagazine = true

  const frequencyOfMagWord = {}

  magazine.forEach(word => {
    if(!frequencyOfMagWord[word]) frequencyOfMagWord[word] = 0
    frequencyOfMagWord[word] ++
  })

  note.forEach(word => {
    if(frequencyOfMagWord[word]) {
      frequencyOfMagWord[word] --
      if(frequencyOfMagWord[word] === 0) delete frequencyOfMagWord[word]
    }
    else {
      usedOnlyMagazine = false
      return
    }
  })

  console.log(usedOnlyMagazine ? 'Yes': 'No')
}
```

### Test Result
모든 테스트 통과
