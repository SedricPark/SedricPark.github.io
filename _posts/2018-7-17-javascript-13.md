---
categories: javascript
---

### 객체지향

#### 객체지향 프로그래밍

* 자바스크립트는 다른 객체지향 프로그램과 다른 독특함을 가지고 있음

#### 생성자와 new 1

```javascript
// 가장 기본적인 객체 생성과 호출이자만 중간에 다른 코드가 끼어들 수 있는 문제점이 있음
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

// 매소드를 할 경우
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