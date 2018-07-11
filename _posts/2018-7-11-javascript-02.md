---
categories: javascript
---

### Javacript 숫자와 문자

#### 숫자

* alert 창에서 실수 포함해서 +, -, * / 모두 가능 

```javascript
<script>
  alert(1+1);
</script>
```

* 브라이저 콘솔 창에서 사친 연산 가능

#### 수와 연산

* 다양한 연산 하기(콘솔에서 작업)

```javascript
Math.pow(3, 2); // 3의 2승 결과를 돌려줌
> 9 

Math.round(10.6); // 반 올림 명령어
> 11

Math.ceil(10.2); // 올림 명령어
> 11

Math.floor(10.2); // 버림 명령어
> 10

Math.sqrt(9); // 9의 제곱근
> 3

Math.random(); // 1보다 적은 난수 생성 
100 * Math.random(); // 100보다 작은 난수 생성 

Math.round(100 * Math.random());
```

#### 문자

* 문자 표현 방법

```html
<script>
    alert("hello world");
</script>
```

* 문자 이스케이핑 : `\`

#### 문자의 연산

* 줄바꿈 : `\n`

```html
<script>
    alert("hello\n world");
</script>
```

* 문자열 더하기

```html
<script>
    alert("hello"+"world");
</script>
```

* 문자와 관련된 함수

```html
<script>
    alert("hello world".length);
</script>
```

* 문자와 관련된 다큐먼트 이용하기
* indexOf 함수 사용하기

```javascript
// 기본문법
stringValue.indexOf(searchValue[,fromIndex])

// 문자열값에.indexOf(찾고자하는 값 [,옵션이며 생략가능])
"hello".IndexOf('h')
> 0
```