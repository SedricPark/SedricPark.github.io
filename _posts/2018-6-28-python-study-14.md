## 파일에 저장하고 읽어오기

### 파일모드

* 모드 R : 기존 파일 읽기
* 모드 W 또는 A : 새 파일 생성해서 쓰기
* 모드 W : 기존 파일 내용 제거하고, 처음부터 쓰기
* 모드 A : 기존 파일에 추가하기

### 파일의 종류

* 텍스트 : 문자열 데이터
  * 인코딩과 디코딩이 필요 함(파이썬3에서 자동 지원)
* 바이너리 데이터 
  * 문자열보다 이미지, PDF , XLS 포맷
  * TEXT 데이터도 바이너리로 열 수 있음

### open 함수

```
file_obj = open(파일경로, mode='r', encoding=None)
read_data = file_obj.read()
file_obj.close()

# file object 주요 멤버 함수
1. .write 함수 : 파일에 쓰기 
2. .read 함수 : 파일 읽기
3. .close 함수 : 파일 닫기
```

* encoding 옵션

```
1. 자동 인코딩/디코딩 옵션
2. text mode 시에만 지정 가능하며 바이너리 모드에서는 지정 불가
3. 미지정시에 os설정에 따라 다른 인코딩이 지정
```

### 파일을 열 때, 5가지 모드

```
1. r(read), w(write), a(append)
2. 인코딩 모드
	t(text) : 자동 인코딩 / 디코딩 모드
	read() 반환타입은 str
	write() 인자로 str 타입 필요
	
	b(binary) : 바이너리 모드
	read() 변환타입은 bytes
	write() 인자로 bytes 타입 필요
3. 지정 예시
	rt(read + text), rb, wt, wb, at, ab
```

* R(read)
  * `filecode = open('파일경로와 파일명', 'rt', endcoding='utf8').read()`
  * 지정 경로에 파일이 없는 경우 IOError 발생
* W(write)
  * `open("파일경로와 파일명", 'wt', endcoding='utf8').write('가나다')`
  * 지정 경로에 파일이 없는 경우, 파일 생성
  * 지정 경로에 파일이 있는 경우, 해당 내용을 무시하고 새로운 파일 생성
  * 쓰기 권한이 없는 경우 PermissionError 예외 발생
  * 지정 경로에 디렉토리가 없는 경우, FileNotFoundError 발생
* (append)
  * `open('경로/파일', 'at', endoding='utf8').write('가나다')`
  * w(write)와 유사
  * 지정 경로에 파일이 있으면 해당 내용에 이어서 내용 추가
* t(text)
  * 지정 encoding으로 자동 인코딩/디코딩과 함께 파일 쓰기/일기

```
with open('파일경로와 파일명', 'wt', encoding='utf8') as f:
	f.write('가나다')
```

* b(binary)

```
with open('파일경로와 파일명.txt', 'wb') as f:
	f.write('가나다').encode('utf8')
	
# 바이너리 인경우 encoding이 필요 없음
with open('파일경로와 파일명.jpg', 'rb') as f:
	photo_data = f.read() # bytes 타입
```

### 파일에 접근하는 다양한 방법

```
f = open('sample.txt', 'rt', encoding='utf8')
print(f.read())
f.close()
```

* 열려진 파일은 반드시 닫아야 한다. `close()`
  * 에러가 발생하면 예외처리를 해주고 닫아 주는 방법도 있음

```python
f = open('sample.txt', 'wt', encoding='utf8')
try:
    f.write('hello')
    1/0
finally:
    f.close()
    print('file closed.')
```

### with 절

* 특정 block을 `with` 절을 통해, 실행전, 실행 후, 예외발생시의 처리가 가능

```python
with open('sample.txt', 'rt') as f:
    file_content = f.read()
```

### file object는 순회 가능한 객체

* 줄(line) 단위로 순회

```python
with open('sample.txt', 'rt', encoding='utf8') as f:
for line in f:
	print (line)

    
# 순회가능한 객체로 바로 넘기기
for line in open('sample.txt', 'rt', endcoding='utf8'):
    print(line)
```