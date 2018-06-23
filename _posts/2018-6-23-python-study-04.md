# python 흐름제어

## 조건문, if

```python
if 조건:
	위 조건에 부합될 때 실행 할 코드 블록
elif 두번째 조건:
	두번째 조건에 부합 될 때 실행할 코드 블록
else:
	위 조건에 모두 맞지 않을 때 실행 할 코드 블록
```

* if 문에서 `if/else` 는 1회만 사용 할 수 있으나 `elif`는 원하는 만큼 사용 가능

## 반복문, for

```python
for 변수 in 리스트:
	매 항목마다 수행할 코드 블록
```

* for 내에서 break 문을 만나면 해당 반복문 종료

```python
for i in range(20):
	print(i)
    if i > 10:
    	break
```

### 반복문 중첩

```python
for i in range(2, 10):
	for j in range(1, 10):
    	print(i, j)
        break
````
* break문을 만나면 즉시 반복문을 빠져 나온다(if문에서는 break를 지원하지 않음)

* 중첩 반복문을 한 번에 종료시키기

```python
def gugu():
	for i in range(2, 10):
    	for j in range(1, 10):
        	print(i, j)
            return None
```

## 내장함수 range

* range(stop) : 0부터 stop 미만의 범위에서 1씩 증가시킨 값으로 순회가능한 객체 구성
` range(start, stop[, step]) : start 값 이상, stop값 미만 범위에서 step씩 증가시킨 값`

## 반복문 while

```python
while 조건:
	매 항목마다 수행할 코드 블록
```

```python
i = 0
while i < 20:
	print(i)
    if i > 10:
    	break
    i += 1
```
