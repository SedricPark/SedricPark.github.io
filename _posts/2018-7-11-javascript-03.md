---
categories: javascript
---

### Javascript 변수

#### 변수

* 변수 선언

```javascript
// var은 생략 가능함, 유효범위를 모르면 생략하지 마세요
var a = 1; 

// console에서 작성
var a = 1;
alert(a);
> 1

// 변수는 이렇게 사용도 가능함
var a = 'hello', b = 'world'
```

* 변수의 사용 이유
  * 코드의 재활용성을 높이기 위해

#### 주석

* `//` 한줄 주석처리
* 여러줄 주석 처리 : `/*      */`

```javascript
/*

*/
```

#### 줄바꿈과 여백

```javascript
<script>
    var a = 1;  // ;는 줄이 끝났다고 선언, 줄바꿈을 하게되면 생략가능
    alert(a);
</script>

// ;를 반드시 사용해야 하는 경우
<script>
    var a = 1; alert(a); // 한줄에 쓸 경우에는 구분을 해줘야 하기 때문
</script>
```





