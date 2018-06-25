---
layout: post
title: python study 08
categories: python
---


## 호출가능한 객체

### 호출문법 : obj()

* 함수를 호출해서, 리턴값을 취한다
* 클래스를 호출해서 인스턴스를 생성한다
* 하지만 클래스의 인스턴스를 호출 할 수는 없다

```python
class Calculator(object):
    def __init__(self, base):
        self.base = base
        
calculator = Calculator(10)
print(calculator()) # 호출 할 수 없다는 에러 발생
```

```python
# 일반적인 인스턴스 호출 방법
class Calculator(object):
    def __init__(self, base):
        self.base = base
        
    def mysum(self, x, y):
        return self.base + x + y

calc2 = Calculator(10)
calc2.mysum(1, 2)
```

```python
# 인스턴스를 호출하고 함수처럼 사용되게끔 하고자 할 때

class Calculator(object):
    def __init__(self, base):
        self.base = base
        
    def __call__(self, x, y):	 # __call__ 멤버함수 사용
        return self.base + x + y

calc2 = Calculator(10)
calc2.__call__(1, 2)
calc2(1, 2)						# 함수처럼 사용 가능
```

