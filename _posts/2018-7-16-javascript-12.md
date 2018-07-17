---
categories: javascript
---

### Javascript 함수지향

#### 유효범위

* 전역변수와 지역변수

```javascript
// 전역변수
let vscope = 'global'; 			// 전역변수
function fscope () {
    alert(vscope);
}

// 지역변수
let vscope = 'global';
function fscope () {
    let vscope = 'local'		// 지역변수
    let myvar = 'myvar'			// 지역변수
    alert(vscope);
}

> alert(myvar); 				// 지역변수는 호출이 안됨
```

* 전역변수와 지역변수 그리고 `let` 

```javascript
let vscope = 'global';
function fscope () {
    vscope = 'local'	// let를 빼버리면 vscope 변수에 local이 할당되고
}
> alert(myvar);			// 호출 했을 때, vscope에 할당된 'local'이 호출 되는 것
```

* 잘못된 유효범위에 따른 문제점

```javascript
// 아래 코드는 무한 루프에 빠지는데, 이유는 반복문에서 계속해서 i = 0으로 초기화 시키기 때문
function a () {
    i = 0;
}
for (var i = 0; i < 5; i++) {
    a();						// 무한루프에 빠짐
    document.write(i);
}
```

#### 전역변수의 사용 

```javascript
// 전역변수를 사용해야 할 때는 익명함수처럼 사용하거나 객체를 생성해서 
// 객체 안에서 함수를 사용하는 방법을 이용해서 그 유효범위내에서 전역변수 사용 추천
function () {
    var MYAPP = {}
    MYAPP.calculator = {
        'left': null,
        'right': null
    }
    MYAPP.coordinate = {
        'left': null,
        'right': null
    }
    MYAPP.calculator.left = 10;
    MYAPP.calculator.right = 20;
    fuction sum () {
        return MYAPP.calculator.left + MYAPP.calculator.right;
    }
    document.write(sum());
}();
```

#### 유효범위의 대상(함수)

* 자바스크립트는 함수에 대한 유효범위만을 제공
  * 함수 안에서는 지역변수의 성질을 가지지만,
  * `for` 문이나 `if` 문에서는 그렇지 않다는것 유의 해야 한다 

#### 정적 유효범위

```javascript
var i = 5;
function a () {
    var i = 10;
    b();
}
function b () {
    document.write(i);
}

a(); 	// 5의 결과값은 5이다. 이유는 함수가 선언된 시점에서의 유효범으를 가지기 때문
```



---

#### 값으로서의 함수와 콜백

* 함수와 매소드의 차이

```javascript
// 함수
function a () {}

// 매소드, 속성값이 담겨져이 있는 함수를 매소드라고 함
a = {
    'a': function () {
        document.write('hello world')
    }
}
```

* 자바스크립트 함수는 값이기 때문에 다른 함수의 인자로 전달이 가능

```javascript
function cal(func, num) {
    return func(num)
}

function increase(num) {
    return num + 1
}

alert(cal(increase, 1));
> 2
```

* 함수가 함수의 리턴 값으로 사용 되는 경우

```javascript
function cal(mode) {
    let funcs = {
        'plus': function(left, right) {return left + right},
        'minus': function(left, right) {return left - right}
    }
    return funcs[mode];
}
alert(cal('plus')(2, 1));
```

* 함수가 배열의 값으로 사용 될 때

```javascript
let process = [
    function (input) { return input + 10 },
    function (input) { return input * input },
    function (input) { return input /2 }
];
let input = 1;
for (let i = 0; i < process.length; i++) {
    input = process[i](input);
}
alert(input);
```

#### 콜백과 비동기 처리

* 콜백

```javascript
// sort라는 함수에는 추가로 인자(함수형태)를 옵션으로 가지고 있는데
// 이것의 작동 방식을 변경해서 사용자가 사용 할 수 있다. 이를 콜백 함수라고 하는것 같음
function sortNumber(a,b) {
    return b - a;
}

let numbers = [20, 10, 22, 11, 6, 2, 8, 30];

alert(numbers.sort(sortNumber)); 
> [30,22,20,11,10,8,6,2]
```

* 비동기처리
  * jquery에서 ajax에서 콜백을 많이 이용하고 있음

```javascript
// json의 데이터 타입이 있다고 가정
{"title": "javascript", "author": "egoing"}

// ajax를 사용해서 비동기 처리, ajax가 콜백 함수를 이용하고 있음
$.get('./data.json.js', function(result) {	
    console.log(result);		
}, 'json');

// .get 인자로 3개가 필요
// .get(인자1, 인자2, 인자3), 인자1에서 받아온 결과값이 인자2의 매개변스로 넘겨주고
// 마지막으로 데이터 타입을 json으로 돌려주는 형태라고 이해가 된다 
```

---



#### 클로저

* 내부함수가 외부함의 context에 접근 할 수 있는 것을 가르킨다
* 내부함수와 외부함수

```javascript
function outter() {						//외부함수
    function inner () {					//내부함수
        let title = 'hello everybody';
        alert(title);
    }
    inner();
}
outter();
```

* 내부함수에서 외부함수 접근

```javascript
function outter() {						//외부함수
    let title = 'hello everybody';
    function inner () {					//내부함수        
        alert(title);					//외부함수 변수에 접근 가능
    }
    inner();
}
outter();
```

* 클로저 정의
  * **외부함수가 호출되어 종료된 이후에도 내부함수는 외부함수에 접근 할 수 있는데, 이를 클로저라고 한다.** 

```javascript
function outter () {
    let title = 'hello';
    return function () {
        alert(title);
    }
}
inner = outter();  	// outter()가 호출되는 순간 outter는 끝난 함수인데
inner();			// inner()를 호출하면 outter의 title에 접근 가능
```

* 클로저 예시 1

```javascript
function myfn (title) {
    return {
        get_title : function () {			// title 접근가능
            return title;
        },
        set_title : function (_title) {		// title 접근가능
            title = _title
        }
    }
}
number_one = myfn('hello');
number_two = myfn('world');

// 같은 외부함수의 지역변수 title에 접근 하였지만 각기 다른 값 반환. 인스턴스 처럼
alert(number_one.get_title()); // hello 반환
alert(number_two.get_title()); // world 반환 

alert(number_one.set_title('and you')); // number_one의 title 변경
```

* 잘못된 클로저 사용

```javascript
// var를 let으로 변경하면 이상 없이 작동 한다
// let은 블럭 단위의 스콥이기 때문
var arr = [];
for (var i = 0; i < 5; i++) {
    arr[i] = function() {		// arr[i] 배열에 실행되지 않은 함수가 할당
        return i;
    }
}
for (var index in arr) {
    console.log(arr[index]());
}
> [5, 5, 5, 5, 5]
```

* 수정된 클로저 함수 사용법

```javascript
var arr = [];
for (var i = 0; i < 5; i++) {
    arr[i] = function(id) {
        return function () {
            return id;
        }
    }(i);
}
for (var index in arr) {
    console.log(arr[index]());
}
```

---



### Arguments

* 매개변수 : 함수를 생성 할 때 받을 변수
* 인자 : 함수를 호출 할 때, 매개변수로 넘겨줄 요소
* 자바스크립트는 관대한 언어

```javascript
// arguments는 받은 인자를 알려주는 내부함수
// 함수를 만들 때 매개변수를 지정하지 않아도, 함수를 호출 할 때 인자를 넣어도 되는 관대함
// 인자의 갯수도 상관 없음
function sum () {
    let _sum = 0;
    for (let i = 0; i < arguments.legth; i++) {
        document.write(i + ' : ' + arguments[i] + '<br/>');
        _sum += arguments[i]
    }
    return _sum;
}

document.write('result : ' + sum(1, 2, 3, 4));
```

* 매개변수 수와 인자의 수

```javascript
// 자바는 매개변수와 주어지는 인자의 차이를 변별하기 위해 다음을 이용 한다
// 매개변수의 갯수와 인자의 갯수를 비교, 같은 경우에는 진행, 다른 경우 에러 발생 시키는방법
function zero(arg) {
    console.log (
    'zero.length', zero.length,		// zero 함수의 매개 변수 갯수
    'arguments', arguments.length	// zero 함수 호출시의 인자 갯수
    )
}
zero('a', 'b')
```

---



#### 함수의 호출

```javascript
// this는 myobject1을 의미하고
// apply 함수는 function.apply([thisObj[,argArray]]) 형태다
// 즉, apply(객체)로 객체를 인자로 넘겨 주면, 객체에 해당하는 this를 사용 가능 하다 

myobject1 = {'a': 1, 'b': 2, 'c': 3};
myobject2 = {'d': 4, 'e': 5, 'f': 6};

function sum() {
    let _sum = 0;
    for (name in this) {
        _sum += this(name)
    }
    return _sum;
}

alert(sum.apply(myobject1)) // 6
alert(sum.apply(myobject2)) // 15
```