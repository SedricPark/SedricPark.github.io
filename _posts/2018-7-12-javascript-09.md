---
categories: javascript
---

### Javascript 객체

#### 객체(object)

* 파이썬의 딕셔너리와 유사함

```javascript
// 객체 생성
let mydict = {'a': 1, 'b': 2, 'c': 3}

// 또 다른 객체 생성 방법
let mydict = new object();
mydict['a'] = 1
mydict['b'] = 2
mydict['c'] = 3

// key: value 형식으로 key로 value를 호출 가능함
mydict['a']
mydict.a  // 역시 호출 가능 함 
```

#### 반복문과 객체

```javascript
// key 값 가져오기
let mydict = {'초코우유': 2, '바나나우유': 3, '사과주스': 4}
for (key in mydict) {
    document.write(key + '<br/>');
}

// value 값 가져오기
let mydict = {'초코우유': 2, '바나나우유': 3, '사과주스': 4}
for (key in mydict) {
    document.write(mydict[key] + "<br/>");
}
```

#### 배열과 반복문

```javascript
let name = ['a', 'b', 'c']
for (get_name in name) {
    document.write(get_name)	// 인덱스 값 출력
}

let name = ['a', 'b', 'c']
for (get_name in name) {
    document.write(name[get_name])	// 값 출력
}
```

####  객체지향 프로그래밍

```javascript
// 객체에 함수가 저장 될 수 있고, 변수로 지정 될 수 있음
let grade = {
    'list': {'초코우유': 10, '바나나우유': 8, '사과주스': 2},
    'show': function () {	
        document.write(this); // this는 함수가 속해있는 객체를 가르킴
    }
}
grade['show'](); //this로 인해서 객체 자체가 호출 됨

// this 확장 응용
let grade = {
        'list': {'초코우유': 10, '바나나우유': 8, '사과주스': 2},
        'show': function () {
            for (let name in this.list) {
                document.write(name, this.list[name]);
            }
        }
    }
grade.show();
```