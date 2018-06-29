## 파이썬 클래스 상속

* 부모 클래스를 자식이 상속 받아서 메소드 등을 그대로 사용하는 것 / 오버라이딩과 다름

```python 
class Person:
    def __init__(self, name):
        self.name = name
        
    def eat(self):
        print("{} 처묵 처묵".format(self.name))
        
    def sleep(self):
        print("{} 쳐 잠".format(self.name))
        
class Doctor(Person):
    def work(self):
        print("{}은 노예입니다".format(self.name))
```

### 클래스 상속의 MRO

* 파이썬 클래스 탐색순서는 MRO에 따름
  * `Class.mro` 확인 가능
  * MRO가 꼬이면 클래스 상속을 받을 수 없음

### 부모 함수의 호출

* 내장함수 `super()` 를 통해 부모의 함수 호출
* 호출시에는 MRO에 기반하여 호출

```python
class A:
    def fn(self):
        print("A-fn")
        
class B(A):
    def fn(self):
        super().fn()
        print("B-fn")
        
class C(A):
    def fn(self):
        super().fn()
        print("C-fn")
        
class D(A):
    super().fn()
    print("D-fn")
```

