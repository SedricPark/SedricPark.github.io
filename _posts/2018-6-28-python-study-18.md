---
categories: python
---

## 장식자

* 어떤 함수를 감싸는 목적의 함수
* 1급 함수 : 함수를 동적으로 생성 가능, 반환값으로 전달 가능

```python
# 1급 함수
def base_10(fn):
    def wrap(x, y):
        return fn(x, y) + 10
    return wrap

def mysum(x, y):
    return x + y

mysum = base_10(mysum)
mysum(1, 2)

def mymulti(x, y):
    return x * y

mywrap = base_10(mymulti)
mywrap(10, 20)
```

* 장식자 사용

```python
@base_10
def mysum(x, y):
    return x + y

mysum(1, 2)
```

* 장식자 활용

```python
import time

def memoize(fn):
    cached = {}
    def warp(x, y):
        key = (x, y)
        if key not in cached:
            cached[key] = fn(x, y)
        return cached[key]
    return wrap

@memoize
def mylongtimesum(x, y):
    time.sleep(1)
    return x + y + 10

@memoize
def mylongtimemulti(x, y):
    time.sleep(1)
    return x * y + 10
```

* 장식자에 인자 지원

```python
def base(base_i):
    def outer(fn):
        def wrap(x, y):
            return x + y + base_i
        return wrap
    return outer

@base(20)
def mysum2(x, y):
    return x + y

@base(30)
def mysum3(x, y, z):
    return x + y + z
```