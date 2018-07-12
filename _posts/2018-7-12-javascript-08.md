---
categories: javascript
---

### Javacript 배열

#### 배열

* 연관된 데이터를 모아서 관리하기 위해서 사용하는 데이터 타입
* 배열 문법
  * 배열 인덱스는 0부터 시작

```javascript
let member = ['korea', 'japan', 'china']

//배열 호출 방법
member[0]
> korea
```

#### 배열과 반복문 사용

```javascript
function get_member () {
    return ['korea', 'japan', 'china']
}
member = get_member()

for (i = 0; i < get_member.length; i++) {	//.length 함수는 배열의 수 리턴
    document.write(get_numebr[i].touppercase + '<br/>') // 대문자로 변경 리턴
}
```

#### 배열의 조작

* 배열에 값 추가하기

```javascript
// 하나의 원소 추가하기
member = ['korea', 'japan', 'china']
member.push('USA')  // push 함수를 이용해서 배열에 추가

// 여러 원소 추가하기
member = member.concat('USA', 'UK')

// 배열에 앞쪽에 추가하기
member.unshift('swiss')

// 원하는 지점에 추가하기
member.splice(1, 0, 'taiwan') // 인덱스와 삭제할 요수의 수, 그리고 추가할 요소 순서
member.splice(1, 1, 'taiwan') // 1인덱스의 1개 요소를 삭제하고 삭제 값 반환
```

#### 배열 제거

```javascript
// 배열 첫번째 원소를 제거하는 방법
member = ['korea', 'japan', 'china']
member.shift();

// 배열 제일 뒷쪽 원소를 제거하는 방법
member = ['korea', 'japan', 'china']
member.pop();		// 원소를 제거하고 제거한 값을 돌려 줌
```

#### 배열 정렬

```javascript
member = ['korea', 'japan', 'china']
member.sort(); // 순서대로 정렬
member.reverse(); // 반대로 정렬
```



