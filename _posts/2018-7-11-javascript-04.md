---
categories: javascript
---

### Javascript 비교

#### 비교 연산자

* 비교연산자 종류 : >, <, <=, >=, == , ===

* 비교 연사자의 결과값은 `true` 또는 `false`

```javascript
// 동등 연산자
alert(1==2)				//false
alert(1==1)				//true
alert('hello'=='world')	//false
alert('hello'=='hello')	//true

// 일치 연산자, 정확하게 같을 때 true, 아니면 false
alert(1=='1')			//true
alert(1==='1')			//false
```

* `==`과 `===` 비교
  * 정신 건강상 `===`을 사용을 권장함

```javascript
// 데이터 타입에 따른 true와 false 값은 변화
// string, booln, number, null, undefined

alert(null == undefined);       //true
alert(null === undefined);      //false
alert(true == 1);               //true, 동등 연산자에서 1은 true, 0은 false
alert(true === 1);              //false
alert(true == '1');             //true, 동등 연산자에서 빈 문자열은 false
alert(true === '1');            //false
 
alert(0 === -0);                //true
alert(NaN === NaN);             //false, 0/0을 나누 경우 등의 계산 불가
```

* 부정과 부등호

```javascript
!= 		// ==의 반대 일 경우
!== 	// ===의 반대 일 경우
>
<
>=		// 같거나 큰 경우 
<= 		// 같거나 작은 경우
```