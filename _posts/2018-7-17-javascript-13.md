---
categories: javascript
---

### 객체지향

#### 객체지향 프로그래밍

* 자바스크립트는 다른 객체지향 프로그램과 다른 독특함을 가지고 있음

#### 생성자와 new 1

```javascript
// 가장 기본적인 객체 생성과 호출이지만 중간에 다른 코드가 끼어들 수 있는 문제점이 있음
let person = {}
person.name = 'sedric';
person.introduce = function () {
    return 'my name is' + this.name;
}
document.write(person.introduce());

// 수정된 객체 생성
let person = {
    'name': 'sedric',
    'introduce': function () {
        return 'my name is' + this.name;
    }
}
document.write(person.introduce());
```

#### 생성자와 new 2

```javascript
// 자바스크립트에서는 클래스가 존재 하지 않음
// 함수 객체를 생성하고, 변수에 new를 사용해서 객체를 생성하게 되는 형태
function Person () {} // 함수 객체 생성

let p = new Person(); // 생성자로 새로운 객체 생성
p.name = 'sedric'
p.introduce = function () {
    return 'my name is' + this.name;
}
```

#### 생성자와 new 3

```javascript
function Person(name) {						// 객체 생성
    this.name = name;
    this.introduce = function () {
        return 'my name is' + this.name;
    }
}
let p1 = new Person('sedric');				// 생성자를 통한 새로운 객체 생성
let p2 = new Person('tomas');
```

#### 전역객체

```javascript
function func () {
    alert('hello');
}

func(); 		// func 함수 호출
window.func() 	// 위와 동일함. window가 바로 전역객체이며 평소 생략
```

* 전역객체의 API
  * 모든 객체는 이 전역객체의 프로퍼티이며
  * ECMAScript에서 전역객체 API를 정의 하고 있음

---



#### this

* this는 맥락에서 이해를 해야 한다

```javascript
// 함수를 호출 할 경우
// 함수가 특별히 지정되어 있지 않기 때문에 전역 객체인 window이기 때문이고
function func() {
        if(window === this) {
            document.write("window == this")
        }
    }
func();

> window === this

// 매소드로 호출 할 경우
// 매소드로 특별히 지정 할 경우에는 객체 그 자신이 this가 된다
let myobject = {
    func : function () {
        if(myobject === this) {
            document.write("myobject === this")
        }
    }
}
myobject.func();

> myobject === this
```

* 생성자의 호출

```javascript
let funcThis = null;
function Func () {
    funcThis = this;		// funcThis에 'let'가 없는거 유의
}

let myobject1 = Func();

if(funcThis === window) {	// window의 매소드이기 때문에 window을 가르킴
    document.write('window') 
}
> window

let myobject2 = new Func()
if (funcThis === myobject2) {
    document.write('myobject2')
}
> myobject2

// 생성자로 객체를 호출하게 되면 this가 해당 객체가 되고
// 일반 함수를 호출하게 되면, this는 전역객체인 window를 가르킨다
```

* apply, call을 이용한 this의 값을 제어

```javascript
let o = {}
let p = {}
function func () {
    switch(this) {
        case o:
            document.write('o');
            break;
        case p:
            document.write('p');
            break;
        case window:
            document.write('window');
            break;
    }
}

func();
> window
func.apply(o);
> o
func.apply(p);
> p
```
---

#### 상속

```javascript
// 부모 객체 생성
function Person (name) {
    this.name = name;
}
Person.prototype.name = null;
Person.prototype.introduce = function () {
    return "hello" + this.name;
}

// 자식 객체 생성
function Whoami (name) {
    this.name = name
}

// 부모 객체 생속
Whoami.prototype = new Person(); //prototype

let p1 = new Whoami('sedric');
document.write(p1.introduce())
> hello sedric
```

---

#### Prototype 프로토타입

```javascript
// prototype chain
function A() {}
A.prototype.atype = true;

function B() {}
b.prototype = new A();

function C() {}
c.prototype = new B();

let d = new C();
console.log(d.atype);

// 프로토타입에 담긴 값의 변경
function C() {}
c.prototype.atype = 1;
c.prototype = new B();

let d = new ();
console.log(d.atype);
> 1 // 결과값이 1로 변경 됨, atype가 어느 부모에 지정되어 있는지 확인 후 값 반환
```

---

#### 표준 내장 객체

* javascript가 built-in 으로 지원해주는 객체
* 배열의 확장 1

```javascript
let myarr = new Array('a', 'b', 'c', 'd', 'e');
function getrandom(getin) {
    let index = Math.floor(getin.length * Math.random());
    return getin[index];
}
console.log(getrandom(myarr));
```

* 배열의 확장 2

```javascript
// 내장 함수 만들어 사용하기
Array.prototype.random = function () {
    let index = Math.floor(this.length * Math.random());
    return this[index];
}
let arr = new Array('a', 'b', 'c', 'd', 'e')
console.log(arr.ramdom());
```

---

#### Object

* 객체의 가장 상위 객체이며 모든 객체가 사용할 수 있는 프로토타입도 만들 수 있음

```javascript
// object api 사용에는 두가지 형태가 있는데 이를 구별 해야 함

// 1. api가 Object.keys() 형태로 되어 있는것
let arr = ['a', 'b', 'c'];
console.log('Object.keys(arr)', Object.keys(arr));

// 2. api가 Object.prototype.toString() 처럼 프로토타입이 중간에 들어간 경우
let o = new Object();
console.log('o.toString()', o.toString());

let a = new Array(1, 2, 3);
console.log('a.toString()', a.toString());

// Object.keys() 형태로 되어 있는것은 바로 사용 가능하지만
// Object.prototype.toString() 처럼되어 있는것은 객체를 새로 생성해서 사용
```

* object 확장( 어느 객체에서나 사용할 수 있는 사용자 매소드 구현)

```javascript
// object에 contain은 원래 있는 메소드이며, 객체 값이 있는지 체크하고,
// 있을 경우 true, 없을 경우 false을 돌려준다
// 아래는 contain을 재정의 하는 과정이다.

Object.prototype.contain = function(neddle) {
    for(let name in this) {
        if(this[name] === neddle) {
            return true;
        }
    }
    return false;
}

let o = {'name':'sedric', 'city': 'seoul'}
console.log(o.contain('sedric'));

// Object 확장의 위험성
for(let name in o) {
    console.log(o[name]);
}
> name
> contain // 객체를 돌려줌

for(let name in o) {
    if(o.hasOwnProperty(name)) //hasOwnProperty 속으로 해결 가능하나 비추
        console.log(name)	   //인자로 전달된 속성의 이름이 객체의 속성인지 판단
}
```

---

#### 데이터 타입

* 원시 데이터 타입, (객체가 아닌 데이터 타입)
  * 숫자
  * 문자열
  * 불리언
  * null
  * Undefined
* 래퍼 객체

```javascript
let str = 'hello'; // hello는 문자열
console.log(str.length) // str은 객체로 임시로 만들고, 끝나면 제거한다

//랩퍼 객체란 아래와 같이 임시적으로 원시데이터를 객체로 만드는것을 뜻한다
let str = 'hello';
str.prop = 'world';	   // 객체로 임시로 만들기 때문에 에러가 안나지만
console.log(str.prop); // undefined 여기서 삭제되기 때문에 속성을 사용하려면 에러
```

* 래퍼 객체 변환
  * 숫자 : Number
  * 문자열 : String
  * 불리언 : Boolean
  * null : 없음
  * Undefined : 없음

---

#### 참조

* 복제와 참조

```javascript
// 복제, 원시데이터는 복제된다
let a = 1;
let b = a; // 복제된다
b = 2
console.log(a)
> 1

//참조, 객체인 경우에는 참조 한다
let a = {'id': 1};
let b = a;
b.id = 2
console.log(a.id)
> 2

//객체이지만 복제처럼 되는 경우
let a = {'id': 1};
let b = a;
b.id = {'id': 2} // a.id.id : 2	인 새로운 객체 생성 
console.log(a.id.id)
> 2
```

* 참조와 복제 그리고 함수

```javascript
// 함수에서 복제
let a = 1;
function func (b) {
    b = 2				// 값을 복제한 상태
}
func(a);
console.log(a);
> 1

// 함수에서 새로운 객체 생성
let a = {'id': 1};
function func (b) {
    b = {'id': 2};		// 새로운 객체 생성
}
func(a);
console.log(a.id);
> 1

// 함수에서 객체 값 변경
let a = {'id': 1};
function func (b) {
    b.id = 2;			// 객체의 값을 변경
}
func(a);
console.log(a.id);
> 2
```
