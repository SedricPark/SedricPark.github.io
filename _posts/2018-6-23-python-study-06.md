---
categories: python
---


# 파이썬 함수


## 함수

* 함수는 <작업 지시서>와 같은 개념
 * 코드 중복을 제거하기 위한 목적
* 함수의 구성
 * 1개의 함수명(필수) : 작업의 이름
 * 0개 이상의 인자값(옵션) : 작업에 필요한 정보
 * 1개의 반환값(옵션) : 작업의 결과를 하나 돌려 받음
* 빌트인 함수 (Builtin Functions) : print, range 등
* 반환값이 없는 함수호출은 None을 리턴함

```python
def myfn(a, b):
	return a + b
```


### 함수의 유형

* 인자 값, 반환 값 없는 함수
* 인자 값은 있지만, 반환 값은 없는 함수
* 인자 값은 없고, 반환 값이 있는 함수
* 인자 및 반환 값이 모두 있는 함수

## Scope 변수의 유효범위

* 지역 변수 : 함수 안에서 선언되어, 함수 내에서만 활용이 가능한 변수
* 전역 변수 : 함수 밖에서 선언되어, 함수 안에서도 활용이 가능한 변수
 * 상수명을 대문자로 작성

## 인자 Arguments

* 함수가 실행되는데에 필요한 0개 이상의 변수 목록

### Positional Arguments

* 인자의 위치에 기반한 인자

```
def fn_positional(name, age):
   print("당신의 이름 {}이며, 나이는 {}입니다".format(name, age))
```

* 인자의 이름에 기반한 인자
 * 디폴트 인자 문법이 함께 적용 : 함수 호출 시에 해당 인자를 지정하지 않으면, 디폴트 적용
 * Positional Arguments로도 적용 가능

```
def fn_keyword(name="", age=0):
	print("당신의 이름 {}이며, 나이는 {}입니다".format(name, age))
```

## Packing

* 인자의 갯수를 제한하지 않고, 다수의 인자를 받음
* 다수의 Positional Arguments를 하나의 tuple로서 받을 수 있음(Packing)

```python
def fn2(*colors):
	for color in colors:
    	print(color)
```

## Unpacking

```python
colors = ['white', 'yellow', 'black']
fn2(*colors)
fn2('brown', 'pink', *colors)
```

## 가변인자 keyword Arguments

* 인자의 갯수를 제한하지 않고, 다수의 인자를 받을 수 있음
* 다수의 키워드 인자를 dict로서 받을 수 있음(packing)

```pytnon
def fn2(**scores):
	for key, score in scores.items():
    	print(key, score)

fn2(apple=10, orange=5, banana=8)

def fn3(apple=0, **scores):
	print('apple': apple)
    for key, score in scores.items():
    	print(key, score)

fn3(apple=10, orange=5, mango=9)
```

## 익명함수 (Anonymous Function)

```python
lambda x, y: x + y
(lambda x, y: x + y)(2, 4)
mysum = lambda *args: sum(args)
```

* 리턴을 사용하지 않아도, 마지막 값을 리턴값으로 처리
* 대게 인자로 1줄 함수를 지정할 때 많이 사용
* 일반 함수와 인자처리도 동일하게 처리(Positional Arguments, Keyword Arguments)

## 1급 객체

* 다른 객체들에 적용 가능한 연산을 모두 지원하는 객체
 * 인자로 넘기기, 변수에 대입하기 등
 * 종류 : 일급 함수 / 클래스 / 컨트롤 /타입 / 데이터타입

## 파이썬의 1급 함수와 클래스

* 함수/클래스를 런타임에 생성 가능
* 함수/클래스를 변수에 할당이 가능
* 함수/클래스를 인자나 리턴값으로 전달 가능

```python
mysum1 = lambda x, y: x + y
mysum2 = mysum1
mysum2(10, 20)

def myfn(fn, x, y):
	return fn(x, y)

myfn(mysum1, 10, 20)
myfn(lambda x, y: x + y, 10, 20)
```
## 고차함수 (High order Function)

* 다른 함수를 생산/소비하는 함수
* 다른 함수를 인자로 받거나 그 결과로 함수를 반환하는 함수

```python
def base_cal(base):
	wrap = lambda x, y: base + x + y
    return wrap

cal_10 = base_cal(10)
cal_10(1, 2)
cal_10(10, 20)
```
