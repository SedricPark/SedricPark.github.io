---
categories: python
---



## 파이썬 내장 함수

* all, any

```python
# all(x)는 반복가능한 자료형 x를 인수로 받아, x가 참이면 True, 거짓인 경우 False
all([1, 2, 3]) # true
all([1, 2, 0]) # 0 때문에 false

# any(x)는 all()의 반대
any([1, 2, 0]) # true
any([1, 2, 3]) # fale
```

* dir

```python
# 객체가 자체적으로 가지고 있는 변수나 함수를 보여준다
dir([1, 2, 3]) # 리스트의 멤버 함수
['append', 'count', 'extend', 'index', 'insert', 'pop',...]

dir({'1':'a'}) # 딕셔너리의 멤버 함수
['clear', 'copy', 'get', 'has_key', 'items', 'keys',...]
```

* divmod

```python
# divmod(a, b) 2개의 숫자를 입력 받아서, a를 b로 나눈 몫과 나머지를 튜플 형태로 리턴
divmod(7, 3) # 7을 3으로 나눈 몫과 나머지
(2, 1) # 튜플 형태로 리턴
```

* enumerate

```python
# 순서가 있는 자료형을 입력받아 인덱스 값을 포함하는 enumerate 객체를 리턴
for i, name in enumerate(['body', 'foo']):
    print(i, name)
0 body
1 foo
```

* eval

```python
# 실행가능한 문자열을 입력으로 받아 문자열을 실행한 결과값을 리턴하는 함수
eval('1+2')
3
eval("'hi' + 'a'")
'hia'
eval('divmod(4, 3)')
(1, 1)
```

* hex, id

```python
# hex(x)는 정수값을 입력 받아 16진수로 변환하여 리턴
hex(234)
'0xea'

# id는 객체를 입력받아 객체의 고유 주소값(래퍼런스)를 리턴하는 함수
id(10)
4519302496
```

* input

```python
# 사용자 입력을 받는 함수
number = input("Enter:")
Enter:
```

* int, oct

```python
# int(x, radix) 는 radix 진수로 표현된 문자열 x를 10진수로 변환하여 리턴
int('10') # 정수 형태로 반환
10
int('11', 2) # 2진수의 '11'을 10진수로 변환하여 리턴
3

# oct(x)는 정수 형태의 숫자를 8진수 문자열로 바꾸어 리턴하는 함수
oct(34) # 34 정수를 8진수로 변경하여 리턴
'0o42'
```

* isinstance

```python
# isinstance(인스턴스, 클래스) 인스턴스가, 클래스에 속하는지 반환
class Person:
    pass
person = Person()
isinstance(person, Person)
True
```

* pow

```python
# pow(x, y) x의 y 제곱한 결과값을 리턴하는 함수
pow(2, 4) # 2의 4제곱
14
```

* round

```python
# round(number [,ndigits]) 숫자를 입력받아 반올림 해주는 함수
round(4.6)
5

round(5.665, 2) # 소수점 2까지만 반올림
5.67
```

* type

```python
# 입력값을 자료형이 무엇인지 알려주는 함수
number = [1, 2, 3]
type(number) # 자료형이 무엇인지 반환
list
```

* zip

```python
# 동일한 개수로 이루어진 자료형을 묶어 주는 역할을 하는 함수
[(i, j) for i, j in zip(range(5), range(6, 10))]
[(0, 6), (1, 7), (2, 8), (3, 9)]
```