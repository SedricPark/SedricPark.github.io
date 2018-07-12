---
categories: javascript
---

### Javacript 함수

#### 함수 기본 형식

```javascript
function fn ( [para..[,para]]) {
    code
    return value
}
```

* 함수의 정의와 호출

```javascript
// 함수 정의
function myfn() {					
    document.write('hello world');
}
// 함수 호출
myfn();
```

* 함수의 입력과 출력
  * return은 함수의 값을 반환하고 종료

```javascript
function get_number1() {
    return 'sedric'
}
alert(get_number1)
```

* 함수의 인자

```javascript
function get_number(arg) {
    return document.write(arg);
}
get_number('a')

// 복수의 인자도 가능하다
function get_number(arg1, arg2) {
    return document.write(arg1 + arg2);
}
get_number(10, 20)
```

* 함수의 다양한 정의 방법

```javascript
// 함수명을 변수처럼 사용하는 경우
numbers = function (arg) {
    for (i = 0; i <=10; i++) {
        document.write(arg + '<br/>');
    }
}
numbers('hello')

//함수 정의와 호출을 같이하는 익명함수 방법
(function () {
    return document.write('hello');
})();
```

