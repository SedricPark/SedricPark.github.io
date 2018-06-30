---
categories: python
---

## 오버로딩과 오버라이딩

* 오버로딩 : 파이썬에서는 지원하지 않음
* 오버라이딩 
  * 클래스 상속과 관련이 있음
  * 부모 클래스의 매소드를 자식 클래스가 재정의 하는 것을 뜻함

### 클래스 오버라이딩 주요 멤버함수

* `__init__(self[, ..])`: 생성자 함수
* `__repr__(self)`: 시스템이 해당객체를 인식할 수 있는 Official 문자열
  * 대게 디버깅을 위해 사용
  * 출력 문자열을 통해, 바로 인스턴스를 생성 할 수 있도록 인스턴스 생성
* `__str__(self)`: informal 문자열.str(obj)시에 호출
* `__getitem__(self, key)`: self[key]를 구현
* `__setitem__(self, key, value)`: self[key] = value

```python
class Person:
    def __init__(self, name, age, foods):
        self.name = name
        self.age = age
        self.foods = foods
        
    def __str__(self):
        return self.name
    
    def __getitem__(self, key):
        return self.foods[key]
    
    def __setitem__(self, key, value):
        self.foods[key] = value
        
foods = ['사과', '피자', '치킨', '우유', '바나나']
tom = Person('tom', 10, foods)
print(tom[3])

tom[3] = '초코우유'
print(tom[3])

print(tom.foods)
```

### 클래스 주요 오버라이딩 멤버함수 - 연산자 재정의

* `__add__`, `__iadd__` 구현하기

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
        
    def __add__(self, value):
        return Person(self.name, self.age + value) # 새로운 인스턴스 만듬
    
    def __iadd__(self, value):
        self.age += value
        return self
    
    def __repr__(self):
        return "Person('{}', '{}')".format(self.name, self.age)
    
tom = Person('tom', 10)
tom + 10
Tom += 20
```

### 클래스 주요 오버라이딩 멤버함수 - with절 지원

* `__enter__(self)`
* `__exit__(self, exctype, excvalue, traceback)`
  * exc_type: 예외 클래스 타입
  * exc_value: 예외 인스턴스
  * traceback : Traceback 인스턴스
  * 예외가 발생하지 않았다면, 인자 3개 값은 모두 None으로서 호출

```python
class File:
    def __init__(self, path, mode):
        self.path = path
        self.mode = mode
        
    def __enter__(self):
        self.f = open(self.path, self.mode, encoding='utf-8')
        return self.f
    
    def __exit__(self, exc_type, exc_value, traceback):
        self.f.close()
        
with File('filepath.txt', 'wt') as f:
    f.write('hello world')
```