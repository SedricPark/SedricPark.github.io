---
categories: python
---



## 순회 가능한 객체



![2017-01-25-iter-vs-gen-relationships](https://ws4.sinaimg.cn/large/006tKfTcgy1fsnju34f8zj30zc0fytay.jpg)

* iterable : 순회 가능한 객체
* Generator : 값을 생산해내는 객체
  * generator expression : 제너레이터 표현식
  * generator function : 제너레이터 함수

### 제너레이터 맛보기

```python
# 제너레이터 표현식(튜플)
(i ** 2 for i in range(10))

# 제너레이터 표현식(리스트)
list(i ** 2 for i in range(10))

# 제너레이터 함수
def power():
    for i in range(10):
        yield i ** 2
power()
```

### 순회 가능한(iterable) 객체

* set, list, dict, tuple, string, generator 모두 순회 가능한 객체
* Custom 클래스에 대해서도 순회 가능토록 만들 수 있음
  * `__iter__` 멤버함수 구현 : self가 iterator로서 동작을 하기 위해 self 반환
  * `__next__` 멤버함수 구현 : iterator 로서 동작
* `for in` 구문에서 활용 가능

### 클래스를 통한 Iterable 객체 만들기

```python
class MyRange:
    def __init__(self, start, end):
        self.start = start
        slef.end = end
        
    def __iter__(self):
        return self		# iterator를 요구 받고, __next__에서 처리
    
    def __next__(self):
        if self.start >= self.end:
            raise StopIteration
        value = self.start
        self.start += 1
        return value


iterable = MyRange(0, 3)

for i in interable:
    print (i)
    
# for in 구문이 이와 같은 클래스를 통해 사용되어짐
            
```
