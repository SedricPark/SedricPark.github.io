---
categories: python
---


## 코루틴 및 제너레이터

### Co-Routine(코루틴)

* Sub Routine : 일반적인 함수
  * 진입점이 하나이며, 부모/자식의 종속적인 관계가 성립
  * 매 호출시마다, Rountine 내 context가 초기화
* Co Rountine : 코루틴
  * 진입점이 여럿이며, 병렬(Concurrensy, not Parallelism) 수행
  * 호출부와 대등한 관계
  * 여러번 호출이 되어도, Routine 내 Context가 유지

```python
# 일반적인 함수
def mysum(x, y):
    x += 1
    y += 1
    return x + y       # 함수 진입점이 하나, 호출 후에는 종료되며 초기화 된다

# 코루틴
def to_3(base):
    i = 0
    yield base
    i += 1
    yield base + 1
    i += 1
    yield base + 2
    i += 1
    yield base + 3
    yield i				# 함수 실행이 종료되지 않고 핑퐁식으로 호출시 마다 계속 유지
```

### generators(제너레이터)

* 연속된(Sequence) 값들을 생산해내는 함수
* 함수에 yield 키워드가 쓰여지면, Generator
* yield 한 값들이 순차적으로 생산됩니다
* Generator에서 return문을 만나더라도 종료만 될 뿐, 리턴값이 사용되지는 않습니다
* 추가 yield가 없으면, Stoplteration 예외 자동 발생
  * `for` 루프는 Stoplteration 예외를 자동으로 처리
  * 직접 Stoplteration 예외를 발생시켜도 됩니다

```python
def muxrange(start, end):
    while stat < end:
        yield start			# 함수 generator 문법
        start += 1
```

### 중첩된 generator

```python
gen1 = (i**2 for i in range(10))	# gen1에서 0이 생성되자 마자
gen2 = (j+10 for j in gen1)			# gen2로 전달 되고
gen3 = (k*10 for k in gen2)			# gen3로 전달 된다

for i in gen3:		# 첫 번째 값을 받아올 때, 그제서야 gen1에서 값을 생산 시작
    print(i, end=' ')
    
# 제너레이터를 튜플이나 리스트로 변화하지 말자
t1 = tuple(i**2 for i in range(10))	# 모든 값이 튜플로 생성 된다
t2 = tuple(j+10 for j in t1)		# 위와 동일하게 생성
t3 = tuple(k*10 for k in t2)		# 위와 동일하게 생성

for i in t3:		# 이미 생성된 튜플 값에 대해서 순회를 하게 됨
    print(i, end=' ')
```

### tuple / list / iterator를 dict으로 변환

```python
# dict는 key/value 쌍으로 구성
mylist = [['a', 1], ['b', 2]]
dict(mylist) # {'a':1, 'b':2}

# key가 중복 되면, 마지막 key/value가 남음
mylist = [['a', 1], ['b',2], ['a', 3]]
dict(mylist) # {'a':3, 'b':2}

# key/value 쌍이 맞지 않을 경우, ValueError 발생
mylist = [['a', 1], ['b',2], ['a']]
dict(mylist) # ValueError 발생
```

### Generator로 피보나치 수열 생산하기

```python
# 제한된 크기만큼만 생산
def fib(max_count):
    x, y, count = 1, 1, 0
    while True:
        if count >= max_count:
            break
        yield x
        x, y = y, x + y
        count += 1
for i in fib(10):
    print(i, end=' ')
    
# 소비하는 만큼만 생산
def fib():
    x, y = 1, 1
    while True:
        yield x
        x, y = y, x + y
count = 0
for i in fib():
    print(x, end='')
    count += 1
    if ount >= 10:
        break

# islice를 이용하자
from itertools import islice
islice(fib(), 10)
tuple(islice(fib(), 10))
```

### list/set/dict Comprehension

* 순회가능한 객체를 조작하여, 필터링/새로운 리스트/사전/집합을 만들 수 있는 방법
* tuple comprehension은 없음. 필요하다면 만들어 줘야 함

```python
# list comprehension
[표현식 for 변수 in 순회가능한객체 if 필터링 조건]
[i ** 2 for i in range(10) if i % 2 == 0]

# dict comprehension
{key:표현식 for 변수 in 순회가능한객체 if 필터링 조건}
{i:i**2 for i in range(10) if i % 2 == 0}

# set comprehension
{표현식 for 변수 in 순회가능한객체 if 필터링 조건}
{i%5 for i in range(10) if i % 2 == 0}
```

### Generator Expression

* list comprehension과 비교 : 한 번에 list를 생성

```python
# list comprehension
[i**2 for i in range(100)]

# Generator expression
(i**2 for i in range(100))  # () 사용, 호출시마다 값을 생성

# 제너레이터 표현식으로 list / tuple 만들기
list(i**2 for i in range(100))
tuple(i**2 for i in range(100))
```
