---
categories: javascript
---

### Javacript 반복문

#### 반복문 while

* while 반복문

```javascript
while (조건) {
    // 반복 실행될 코드
}

// 기본 반복문
<script>
    let a = 0;
    while (a <= 10) {
        document.write(a + ' hello world <br />') // 웹에 작성하는 함수
        a += 1
    }
</script>
```

* for 반복문
  * `++ i` 와 `i++` 은 다르다.
    * ++i는 1을 더한 상태에서 i가 실행되고, i++는 실행되고 나서 1이 더해진다

```javascript
for (let a = 0; a <= 10; a++) {
    document.write(a + ' hello world <br/>') // for문 사용
}
```

* 반복문 제어

```javascript
// break, 5이후는 출력되지 않음
for (let a = 0; a <=10; a++) {
    if (a === 5) {
        break;
    }
    document.write(a + ' hello world <br/>')
}

// continue, 5를 빼고 계속 실행 됨
for (let a = 0; a <=10; a++) {
    if (a === 5) {
        continue;
    }
    document.write(a + ' hello world <br/>')
}
```

* 반복문 중첩, 구구단 출력 하기

```javascript
for (let i = 2; i <=9; i++){
    for (let j = 1; j <=9; j++) {
        document.write(i + ' X ' + j + ' = ' + i*j + '<br/>')
    }
}
```

* 크롬 콘솔창 이용하여 디버그

![image-20180712152854523](https://ws1.sinaimg.cn/large/006tKfTcgy1ft72b22n70j313q0xu46i.jpg)