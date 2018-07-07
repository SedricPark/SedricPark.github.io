---
categories: crawling
---

## 웹크롤링 시작하기2

### DOM 문서

* The Document Object Model
* 브라우저는 HTML 문자열을 DOM Tree로 변환하여 문서를 표현
  * 크롬개발자 도구에서 보는것과 소스보기를 해서 보는것과 정보가 다름

### BeautifulSoup4 기본 작동

* Html/Xml Parser: Html과 Xml 문자열에서 원하는 태그정보를 뽑아낸다

  ```python
  from bs4 import BeautifulSoup
  
  html = '''
  	<ul>
  		<li>하나 - 블라블라1</li>
  		<li>둘 - 블라블라2</li>
  	</ul>
  '''
  
  soup = BeautifulSoup(html, 'html.parser') # html.parser을 명시
  for tag in soup.select('li'):
      print(tag.text)
  ```

#### Parser : HTML parser / lxml parser

* html parser : bs4에 내장 되어 있음
* lxml parser
  * 별도의 설치가 필요 함
  * html.parser보다 유연하고 빠른 처리

#### bs4에서 태그를 찾는 2가지 방법

* find를 통해 태그 하나씩 찾기
* tag 관계를 지정해서 하나씩 찾는 방법(CSS selector 사용)

#### find를 통해서 찾는 방법 예시

* **find로 찾을시 class는 예약어이기 때문에 class_ 로 사용해야 함**

```python
import requests
from bs4 import BeautifulSoup

headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36',
    'Referer': 'http://www.melon.com'
}

html = requests.get('http://www.melon.com/chart/index.htm', headers=headers).text
soup = BeautifulSoup(html, 'html.parser')

tag_list = []
for tr_tag in soup.find(id='tb_list').find_all('tr'):
    tag = tr_tag.find(class_='wrap_song_info')
    if tag:
        tag_sub_list = tag.find_all(href=lambda value: (value and 'playSong' in value))
        tag_list.extend(tag_sub_list)

for idx, tag in enumerate(tag_list, 1):
    print(idx, tag.text)
```

#### CSS Selector를 사용하는 방법

* CSS Selector를 통한 Tag 찾기 지원
  * tag_name : "tag_name"
  * tag id : "#tag_id"
  * tag class names : ".tag_class"

```
* : 모든 태그
tag : 해당 모든 태크
Tag1 > Tag2 : Tag1의 직계인 모든 Tag2
Tag1 Tag2 : Tag1의 자손인 모든 Tag2 (직계임이 요구되지 않음)
Tag1, Tag2 : Tag1이거나 Tag2인 모든 Tag
tag[attr] : attr속성이 정의된 모든 Tag
```

* CSS Selector 작성법

```
1. tag#tag_id : id가 tag_id인 모든 Tag
2. tag.tag_class :클래스명 중에 tag_class가 포함된 모든 Tag
3. tag#tag_id.tag_cls1.tag_cls2 # id가 tag_id이고, 클래스명 중에 tag_cls1, tag_cls2가 모두 포함된 모든 Tag
4. tag.tag_cls1.tag_cls2 # 클래스명 중에 tag_cls1와 tag_cls2가 포함된 모든 tag
5. tag.tag_cls1 .tag_cls2 # 클래스명 중에 tag_cls1이 포함된 tag의 자식 중에(직계가 아니어도 상관 없음) 클래스명에 tag_cls2가 포함된 모든 tag
```

* CSS Selector을 사용한 크롤링 예시

```python
import requests
from bs4 import BeautifulSoup

headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36',
    'Referer': 'http://www.melon.com'
}

html = requests.get('http://www.melon.com/chart/index.htm', headers=headers).text
soup = BeautifulSoup(html, 'html.parser')

tag_list = soup.select('#tb_list tr .wrap_song_info a[href*=playSong]')
for idx, tag in enumerate(tag_list, 1):
    print(idx, tag.text)
```