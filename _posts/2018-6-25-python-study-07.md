---
categories: python
---

##  파이썬 클래스

* 함수는 데이터의 처리방법을 구조화, 데이터 자체는 구조화하지 못함
* 큰 문제를 작게 쪼개는 것이 아니라, 먼저 작은 문제들을 해결 할 수 있는 객체들을 만든 뒤, 이 객체들을 조합해서 큰 문제를 해결하는 상향식 해결 방법 도입

### OOP 주요 특징

* 캡슐화 : 관련 특성/기능을 하나의 클래스에 결합
* 상속 : 코드의 재활용성 증대
* 다형성
  * 다른 동작을 동일한 함수로 사용할 수 있도록 지원

### 클래스

* 파이썬은 사용자 정의 데이터 타입
  * 관련된 다수의 변수와 함수의 묶음으로 구성
  * ex) Circle 클래스, Person 클래스 등등
  * 함수명은 snake_case, 클래스는 CamelCase

### 파이썬3 기본 클래수는 object를 상속 받아도, 안 받아도 동일 함

```python
# Python 3

class Python3New:
    pass

class python3New(object):
    pass
```

### Circle 로직구현(class 사용 전)

```python
from math import sqrt

def get_area(circle):
    return circle['radius']**2

def get_distance(circle1, circle2):
    return sqrt((circle1['x'] - circle2['x']) **2 + (circle1['y'] - circle2['y']) ** 2) - (circle1['radius'] + circle2['radius'])

circle1 = {'x': 10, 'y': 20, 'radius': 3}
circle2 = {'x': 100, 'y': -40, 'radius': 10}
```

### Circle 클래스로 로직 구현

```python
from math import sqrt

class Circle(object):
    def __init__(self, x, y, radius):
        self.x = x
        self.y = y
        self.radius = radius

    def area(self):
        return self.radius ** 2
    
    def distance(self, other):
        return sqrt((self.x - other.x) **2 + (self.x - other.y) ** 2) - (self.radius + other.radius)
```

### Person 클래스

```python 
class Person(object):
    def __init__(self, name, age, region):
        self.name = name
        self.age = age
        self.region = region
        
    def say_hello(self):
        print("안녕하세요. {}님. {}에서 오셨군요". format(self.name, self.region))
        
    def move_to(self, region):
        print("{}에서 {}(으)로 이사합니다.".format(self.region, region))
        self.region = region
```

* 지정 클래스 타입의 변수 = 인스턴스
* 인스턴스 생성 문법
  * 함수가 호출하듯이, 클래스를 호출하여 인스턴스 생성
* 클래스가 호출이 될 때, 클래스내 `__init__` 함수가 자동 호출
  * 생성자라하며, 해당 인스턴스를 초기화하는 역할
  * 클래스 호출 시에 넘겨진 인자는 모두 생성자의 인자로 넘겨짐
* 인스턴스를 위한 함수/변수를 인스턴스 함수, 인스턴스 변수

### 클래스 변수와 인스턴스 변수

* 클래스 변수 : 클래스 공간에 저장
* 인스턴스 변수 : 각 인스턴스마다 개별 공간에 저장

```python
# 파이썬에서 잘못된 인스턴스 변수 선언
class Dog():
    tricks = []
    
    def add_tricks(self, trick):
        self.tricks.append(trick)
        
# 파이썬에서 올바른 인스턴스 변수 선언
class Dog():
    def __init__(self):
        self.tricks = []
    
    def add_tricks(self, trick):
        self.tricks.append(trick)
```

### Data Hiding, Name Mangling

* 파이썬에서는 접근 제한자(public, private, protected) 미지원
* 언더스코어 2개 `__`로 시작하는 이름에 한하여 이름을 변경 기법을 제공
  * 인스턴스 함수 내에서는 이름 그대로 접근
  * 클래스 밖에서는 `_클래스명.__변수명` 으로 접근

```python
class Person:
    def __init__(self, name):
        self.__name = name
        
    def say_hello(self):
        print('안녕 {}.'.format(self.__name)) # 인스턴스 함수 내에서 그대로 접근

tom = Person("tom")
tom.__name # 에러 접근할 수 없음
tom._Person__name
```
