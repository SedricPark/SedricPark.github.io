---
categories: python
---


## 빌트인 함수, 정렬

### built-in  함수

#### sorted

* 정렬된 리스트를 반환
  * `sorted_list = sorted(iterable, key=None, reverse=False)`
  * key : 정렬 기준값을 생성할 함수를 지정. iterable 객체의 각 원소마다 key 함수가 호출되고 그 리턴값으로 정렬을 수행

```python
def sort_fn(value):
    return value % 10 # 1의 자리수로서 정렬을 수행 하려면

sorted_list = sorted([19, 25, 32, 45], key=sort_fn, reverse=True)
sorted_list = sorted([19, 25, 32, 45], key=lambda value: value%10, reverse=True)
```

#### filter

* 각 원소마다 지정함수가 호출되어, 리턴값이 True 판정될 경우 통과
* `iterator = filter(필터링 여부를 결정할 함수, iterable)`

```python
def judge_fn(value):
    return value % 2 == 0 # 짝수만 통과

iterator = filter(judge_fn, [1, 2, 3, 4, 5])
iterator # <filter at 0x4020203> 이터레이터임
list(iterator) # 값을 호출시에 반환 함
list(iterator) # 두번 호출하면 값이 나오지 않음. 이유는 한번 사용 하였기 때문
```

#### map

* 지정함수의 리턴값을 반환할 iterator 반환
* `iterator = map(값을 변환할 함수, iterable)`

```python
def power_fn(value):
    return value ** 2

iterator = map(power_fn, [1, 2, 3, 4, 5])
```

#### map & filter

```python
iter_filter = filter(lambda i: i%2==0, [1, 2, 3, 4, 5, 6, 7, 8])
iter_map = map(lambda i: i**2, iter_filter)
```

#### max / min

* iterable에서 key 함수를 거친 결과 값 중에 가장 큰/작은 결과값을 반환
* `최대값 = max(iterable [, default=obj][, key=값을 변환 할 함수])`
* `최소값 = min(iterable [, default=obj][, key=값을 변환 할 함수])`
  * iterable이 비었을 경우, default 값을 반환
  * 디폴트값을 지정하지 않고 iterable 객체가 비었을 경우, ValueError 발생

```python
max([-10, 4, 5, 6, 7], key=lambda value: abs(value))
max([], defalut=0) # 비었을 경우 에러가 발생해서 디폴트 값을 지정해 줌
```

#### list의 sort 멤버 함수

* sorted : 다양한 iterable객체를 정렬한 새로운 리스트를 리턴
  * 원본 <iterable 객체>의 순서는 변경하지 않음
* list는 자체적으로 sort함수를 지원
  * list 자체의 순서를 변경
  * sorted와 다르게 **리턴값**이 없습니다. **None**을 반환함

```python
mylist = [10, 9, 2, 3, 5]
mylist.sort(key=None, reverse=False) # 리턴값이 없음
mylist #변환한 값을 확인
```

#### 임의 기준으로 정렬하기

```python
mylist = [5, -6, 3, 1, 3, 6, 9]

# 절대값을 기준으로 정렬
sorted(mylist, key=lambda i: abs(i), reverse=True)

# 3으로 나눈 나머지 값을 기준으로 내림차순 정렬
sorted(mylist, key=lambda i: i%3, reverse=True)

# list의 sort 함수를 호출하여 list 자체의 순서를 변경
mylist.sort(key=lambda i: i%3, reverse=True)
```

#### 대소비교

* 한 문자끼리 비교는 각 문자에 매핑된 문자코드를 따라 비교
* 문자열끼리의 비교는 첫번째 인덱스부터 대소비교가 판가름 날 때까지 비교
* list/tuple 끼리의 비교도 문자열 비교와 동일
