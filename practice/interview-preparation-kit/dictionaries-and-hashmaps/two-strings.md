# Two Strings
두개의 단어가 한개 이상의 문자를 공유하는지 판단하는 문제.

## 제약조건
<var>s1</var>과 <var>s2</var>는 ascii문자이다(a-z).
<br>
<var>1 &le; p &le; 10</var>
<br>
<var>1 &le; |s1|, |s2| &le; 10<sup>5</sup></var>
<br>

## Try 1.
두 단어중 첫번째 단어의 문자들을 {문자: 개수} 형태의 Map으로 저장하고, 두번째 단어의 문자가 Map에 포함되어 있는지 확인하는 방법으로 진행.

### Code
```js
function twoStrings(s1, s2) {
  const charMap = {}

  let sharing = false

  s1.split('').forEach(c => {
    if(!charMap[c]) charMap[c] = 0
    charMap[c] ++
  })

  s2.split('').forEach(c => {
    if(charMap[c]) {
      charMap[c] --
      sharing = true
      return
    }
  })
  
   return sharing ? 'YES' : 'NO'
}
```

### Test Result
모든 테스트 
