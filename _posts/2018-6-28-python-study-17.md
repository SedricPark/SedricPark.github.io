---
categories: python
---

## 예외

* 예외 처리 문법

```python
print('line 1')
try:
    value = int('a') + 1
except ValueError as e:
    print(e)
print('line2')

# 예외를 잡아서 처리 했기 때문에, print('line 2')가 출력 됨
```

```python
# 호출한 함수 내에서 발생한 예외도 잡을 수 있음

def fn1(x, y):
    return x + y

def fn2(a, b):
    return 10 * fn1(a, b)

try:
    print(fn('a', 10))
except TypeError as e:
    print(e)
```

* 주요 예외 문구
  * Exception : 최상위 예외 클래스
  * StopIteration
    * Iterator 내에서 더 이상 생산할 Item이 없을 때
    * for in 구문에서 이 예외를 통해 반복문 중단을 처리
  * AtributeError : attribute 참조 실패 혹은 설정이 실패한 경우
  * ImportError: 지정 모듈/패키지를 import 하지 못한 경우
  * NotImplementedError: 구현하지 않은 부분임을 명시 할 때, 개발자가 예외 발생
  * IndexError: 범위 밖의 인덱스 참조시
  * KeyError: 존재 하지 않는 key 접근시
  * NameError: local/global name을 찾지 못한 경우
  * TypeError: 부적절한 연산/함수를 적용 했을 때
  * ValueError: 부적절한 값을 발견 했을 때
  * IndentationError: 소스코드 들여쓰기가 있을 때

### 예외 처리

* 튜플로 예외를 다수 지정 가능함
* `as` 를 통해 예외 인스턴스를 획득 가능
* `else:` 예외가 발생하지 않았을 때 호출되는 블럭

```python
# 일반적인 else 구문과 예외처리의 else 구문의 차이
for i in range(10):
    print(i)
else:				# for문이 정상적으로 반복된 후에 END 출력됨
    print('END')	# for 문이 break 등을 만나면 END 출력 안됨
    
# 예외처리의 else:

try:
    value = 1 + 1
except TypeError as e:
    print(e)
else:
    print('아무런 예외도 발생하지 않음') # 예외가 없으면 출력이 됨
```

* `finally:` 예외 발생 유무에 상관없이 호출되는 블럭 

### 사용자 예외 정의

```python
class TooBigNumberException(ValueError):
    def __init__(self, value):
        self.value = value
        
    def __str__(self):
        return 'too big numebr {}'.format(self.value)
    
class TooSmallNumberException(ValueError):
    def __init__(self, value):
        self.value = value
        
    def __str__(self):
        return 'too small number {}'.format(self.value)
    
def fn(i):
    if 1 > 100:
        raise TooBigNumberException(i)
    elif i < -100:
        raise TooSmallNumberException(i)
    return i * 10

try:
    fn(200)
except TooSmallNumberException as e:
    print(e)
except TooBigNumberException as e:
    print(e)
```
