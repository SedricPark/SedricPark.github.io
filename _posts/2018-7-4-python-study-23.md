---
categories: crawling
---

## 웹크롤링 시작하기 4

#### 단순 HTML 크롤링

```python
# 사이트 주소 : https://askdjango.github.io/lv1/

import requests
from bs4 import BeautifulSoup

url = 'https://askdjango.github.io/lv1/'
response = requests.get(url).text
result = BeautifulSoup(response, 'html.parser')

for tag in result.select('ul#course_list li a'):
    print(tag.text, tag['href']) # .text 지원, key=value를 이용해서 'href'지정
```

#### Ajax 랜더링 크롤링

* 찾고자 하는 데이터가 html 형태로 나오지 않을 때
  * 구글 개발자 도구를 이용해서, 네트워크탭으로 이동해서 페이지 새로 고침
  * 새로고침시 서버와 응답 받는 여러가지 형태를 확인하는 것이 필요
  * 크롬 Response에 json 형태 등으로 보인다면 주소를 복사해서 사용 하면 됨 

```python
# 사이트 주소 : https://askdjango.github.io/lv2/

import requests
import json

url = 'https://askdjango.github.io/lv2/data.json'
response = requests.get(url).text
result = response.json()

for i in result:
    print(i['name'], i['url'])
```

#### 자바스크립트 랜더링 크롤링

* 데이터가 없는경우 자바스크립트에 임베딩 되어 있는지 확인

```python
# 사이트 주소 : https://askdjango.github.io/lv3/

import json
import re
import requests

url = 'https://askdjango.github.io/lv3/'
response = requests.get(url).text
matched = re.search(r'var courses = (.+?);', response, re.S)
result = matched.group(1)

result_list = json.loads(result)
for i in result_list:
    print('{name} {url}'.format(**i))
```