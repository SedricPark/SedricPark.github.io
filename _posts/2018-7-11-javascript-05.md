---
categories: javascript
---

### Javascript 조건문

#### 조건문의 기본 문법

```javascript
// 실행
if (true) {
    alert('true');
}

// 실행되지 않음
if (false) {
    alert('false');
}
```

* else, else if 사용 하기

```javascript
// false이 화면에 출력
if (false) {
    alert('true');
} else {
    alert('false');
}

// else if 사용하기
if (false) {
    alert('true');
} else if (false) {
    alert('false');
} else {
    alert('final false')
}
```

* prompt 사용하기
  * 사용자의 입력 값을 받아서, 받은 내용을 돌려 줌

```javascript
alert(prompt('당신의 나이는?'));  // prompt가 먼저 실행 되고, alert이 실행 됨

// prompt 이용해서 조건문 사용하기
<script>
    id = prompt('아이디를 입력하셈');
    if (id == 'sedric') {
        alert('안녕하세요')
    } else {
        alert('누구세요?')
    }
</script>

// 간단한 아이디와 비밀번호 확인 코드, if문 중첩
<script>
    var id = prompt('아이디를 입력하셈');
    if (id == 'sedric') {
        var password = prompt('비밀번호 입력하셈');
        if (password == '1111') {
            alert(id + '님' + ' 안녕하세요');
        } else {
            alert('비밀번호 틀림');
        }
    } else {
        alert('누구세요?')
    }
```

#### 논리 연산자

* `&&` : 좌항과 우항이 모두 true인 경우에 true 값을 돌려줌

```javascript
// 간단한 아이디와 비밀번호 확인코드 간단하게 하기
<script>
    var id = prompt('아이디를 입력하세요');
    var password = prompt('비밀번호를 입력하세요');
    if (id == 'sedric' && password == '1111') {
        alert(id + '님' + ' 안녕하세요');
    } else {
        alert('누구신지요?')
    }
</script>
```

* `||` : 둘 중에 하나가 참이면 true 값을 돌려줌

```javascript
// 여러 아이디 중에서 하나라도 참이면, 비밀번호 값을 받아서 처리 하기
<script>
    let id = prompt('아이디를 입력하세요');
    let password = prompt('비밀번호를 입력하세요');
    if ((id === 'sedric' || id === 'lucky') && password === '1111') {
        alert(id + '님' + ' 안녕하세요');
    } else {
        alert('누구신지요?')
    }
</script>
```

#### boolean의 대체제

* 조건문에 사용되는 데이터 형은 true와 false 뿐만 아니라, 0은 false 0이 아니면 true로 간주
* 빈문자열, undefined, null 역시 false로 간주