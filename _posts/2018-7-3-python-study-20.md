---
categories: crawling
---

## 웹크롤링 시작하기1

###HTML 크롤링 하기

#### 헤더란?

* HTTP 요청/응답시에 헤더정보가 key/value 형식으로 세팅
* 대게의 브라우저에서는 다음 헤더를 설정
  * User-Agent : 브라우저의 종류
  * Referer : 이전 페이지의 URL 정보
  * Accept-Language : 어떤 언어의 응답을 원하는가
  * Authorization : 인증정보
* 크롤링 시에는 User-Agent헤더와 Referer를 커스텀하게 설정할 필요가 있음
  * 서비스에 따라 User-Agent, Referer 헤더를 통해 응답을 거부하기도 함

#### 바디란?

* HTTP 요청 시에는 바디가 없음. 응답에만 있음
* 예시 : HTML 코드, 이미지 데이터, JavaScript코드, CSS코드, 비디오 데이터 등

#### 웹 클라이언트 라이브러리

* requests
  * 첫 응답만 받음. 추가 요청이 없음
  * 단순한 요청에 최적화
  * HTML 응답을 받더라도, 이에 명시된 이미지/CSS/JavaScript 추가 다운 수행 하지 않음
  * 직접 다운로드 요청은 가능함
* selenium : 웹브라우저 자동화 툴
  * javascript/css 지원, 기존 GUI 브라우저 자동화 라이브러리
  * 사람이 웹서핑하는 것과 동일한 환경, 대신에 리소스를 많이 먹음
  * 웹브라우저에서 HTML에 명시된 이미지/CSS/javascript를 모두 자동 다운로드 / 적용

### requests (GET 요청)

* 단순 GET 요청

```python
# 단순 get 요청
import requests
response = requests.get('http://www.naver.com')

response.status_code # 응답코드
response.headers # 응답헤더
response.txt # 응답 html
```

* get 요청시 커스텀 헤더 

```python
# get 요청시 커스텀 헤더 지정
request_headers = {
    'User-Agent': ('Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36'), # Mozilla ~ 537.36까지 하나의 문자열로 이루어짐
    'Referer': 'http://www.naver.com'
}
response = requests.get('http://www.naver.com', headers=request_headers)
```

* get 요청시 get인자 지정

```python
# params 인자로 dict지정: 동일 Key의 인자를 다수 지정 불가
get_params = {'mode': 'LSD', 'mid': 'shm', 'sid1': '105'}
response = requests.get('http://news.naver.com/main.nhn', params=get_params)

# params 인자로 (key, value) 형식의 tuple 지정 : 동일 key의 인자를 다수 지정 가능
get_params = [('mode', 'LSD'), ('mid', 'shm'), ('sid', '105')]
response = requests.get('http://news.naver.com/main.nhn', params=get_params)

get_params = (('mode', 'LSD'), ('mode', 'shm'), ('sid', '105'))
response = requests.get('http://news.naver.com/main.nhn', params=get_params)
```

* 응답헤더

```python
# 응답헤더는 사전처럼 보이나 사전이 아니라 requests.structures.CaseInsensitiveDict 타입
# key 문자열의 대소문자를 가리지 않음
# 각 헤더의 값은 헤더이름을 key로 접근하여 획득
response.headers
response.headers['Content-Type'] # key문자 대소문자에 상관 없이 접근 가능

# encoding
response.encoding # context-type에 지정된 encoding을 알려줌
```

* 응답 바디

```python
bytes_data = response.content # 응답 Raw 데이터(bytes)
str_data = response.text # response.encoding으로 디코딩하여 유니코드 변환
```

>이미지 데이터일 경우에는 `.content`만 사용

```python
with open('flower.jpg', 'wb') as f:
    f.write(response.content)
```

> 문자열 데이터일 경우에는 `.text`를 사용

```python
html = response.text
html = response.content.decode('utf8')
```

> **`.text` 로 받아 온 값은 api는 딕셔너리처럼 보이지만 문자열이다**

```python
import requests
import json

get_params = (('mode', 'LSD'), ('mode', 'shm'), ('sid', '105'))
response = requests.get('http://httpbin.org/get', params=get_params)
print(response.text)

# 결과 값! 문자열이다
{"args":{"mode":["LSD","shm"],"sid":"105"},"headers":{"Accept":"*/*","Accept-Encoding":"gzip, deflate","Connection":"close","Host":"httpbin.org","User-Agent":"python-requests/2.19.1"},"origin":"58.76.130.85","url":"http://httpbin.org/get?mode=LSD&mode=shm&sid=105"}

# 그래서 받아온 문자열을 객체로 변환하는 과정이 필요하다
print(response.json()) #이런 식으로
```

* json 포멧의 응답일 경우

  * json.loads(응답문자열)을 통해 직접 Deserialize 수행

  * 혹은 .json()함수를 통해 Deserialize 수행

  * 응답문자열이 json 포멧이 아닐 경우 JSONDecodeError 발생

  * ```python
    import json
    
    # 동일한 Deserialize 
    obj = json.loads(response.text)
    obj = response.json()
    
    # 간결하게 작성하는 방법
    resonse.json()['args'] # 이런식으로
    ```

* 한글이 깨진 것처럼 보여질 경우

```python
# 다음과 같이 직접 인코딩을 지정한 후에 .text에 접근
response.encoding 
'iso-8859-1' # 또는 None일 경우
response.encoding = 'euc-kr' # 로 지정
html = response.text

# 혹은 .content를 직접 디코딩할 수도 있음
html = response.content.decode('euckr')
```

### requests (POST 요청)

* 단순 post 요청
  * `response = requests.post('http://httpbin.org/post')`
* 일반적인 Form 전송/요청(인자로 **data, files**)

```python
# data 인자로 dict지정: 동일 Key의 인자를 다수 지정 불가
data = {'mode': 'LSD', 'mid': 'shm', 'sid1': '105'}
response = requests.get('http://news.naver.com/main.nhn', data=data)

# data 인자로 (key, value) 형식의 tuple 지정 : 동일 key의 인자를 다수 지정 가능
data = [('mode', 'LSD'), ('mid', 'shm'), ('sid', '105')]
response = requests.get('http://news.naver.com/main.nhn', data=data)

data = (('mode', 'LSD'), ('mode', 'shm'), ('sid', '105'))
response = requests.get('http://news.naver.com/main.nhn', data=data)
```

#### JSON POST 요청

* json api 호출시

```python
# json 인코딩
import json
json_data = {'k': 'v2', 'k2': [1, 2, 3], 'name', 'ask장고'}

# json 포맷 문자열로 변환한 후, data 인자로 지정
json_string = json.dump(json_data)
response = request.post('http://httpbin.org/post', data=json_string)

# 객체를 josn인자로 지정하면, 내부적으로 josn.dump 처리
response = requests.post('http://httpbin.org/post', json=json_data)
```

#### 파일 업로드 요청

```python
# mulitpoar/form-data 인코딩
files = {
    'photo1': open('f1.jpg', 'rb'), # 데이터만 전송
    'photo2': open('f2.jpg', 'rb'),
    'photo3': ('f3.jpg', open('f3.jpg', 'rb'), 'image/jpeg', {'Expires': '0'}),
}
post_params = {'k1': 'v1'}
response = request.post('http://httpbin.org/post', files=files, data=post_params)
```

