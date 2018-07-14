---
categories: javascript
---

### Javascript 정규표현식

#### 정규표현식

* 정규표현식 리터럴
  * `let pattern = /a/;`
* 정규표현식 객체 생성자
  * `let pattern = new RegExp('a');`

#### 정규표현식 메소드 실행

```javascript
// 정규표현식 객체 생성
let pattern = /a/;

// 생성한 정규표현식 객체로 찾고자 하는 문자열을 인자로 줌
pattern.exec('abcde'); // 'abcde'에서 /a/를 찾음
> ["a"]

let pattern = /a./; // 정규표현식에서 . 은 어떤 문자도 올 수 있지만 앞에 a는 꼭있어야 함
pattern.exec('abcde');
> ['ab']

pattern.test('abcde') // test 함수는 찾고자 하는 값이 있는지 없는지 boolean 돌려줌
> true
```

#### 문자열에서 정규표현식

```javascript
let pattern = /a/;
let str = 'abcde'

str.match(pattern);			// match 일치하는지 확인하는 함수
> ['a']						// 값이 없는 경우 null 반환

str.replace(pattern, "A")	// str 문자열에서 pattern에 해당하는것을 "A"로 변경
```

#### 정규표현식 옵션

* i : 대소문자 구분하지 않는 옵션
* g : 검색된 모든 결과를 리턴한다

#### 정규표현식 활용 - 캡쳐

```javascript
let pattern = (\w+)/s(\w+); 				//객체 생성
let str = "hello world";
var result = str.replace(pattern, "$2, $1")	// $은 그룹을 뜻함, 숫자는 1부터
console.log(result);
> world, hello
```

정규표현식 활용 - 치환

```javascript
let urlpattern = /\b(?:https?):\/\/[a-z0-9-+&@#\/%?=~_|!:,.;]*/gim;
let content = 'hello : http://www.naver.com/news/223 world. naver : http://naver.com';
let result = content.replace(urlpattern, function(url){
    return '<a href="'+url+'">'+url+'</a>';
})
console.log(result)
```