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



