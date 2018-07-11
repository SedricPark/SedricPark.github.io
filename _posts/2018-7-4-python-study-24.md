---
categories: crawling
---

## 웹크롤링 시작하기 5

* 네이버 실시간 검색어 크롤링하기

```python
import requests
from bs4 import BeautifulSoup

url = 'https://www.naver.com'
html = requests.get(url).text
soup = BeautifulSoup(html, 'html.parser')
result = soup.select('.PM_CL_realtimeKeyword_list_base .ah_item .ah_k')

for i, j in enumerate(result, 1):
    print(i, j.text)
```

* 네이버 블로그 검색어로 크롤링하기

```python
def mydict(fn):
    def wrap(x, y):
        r_dict = fn(x, y)
        for i, j in r_dict.items():
            print(i, j)
    return wrap
        

@mydict
def search_keyword(keyword, value): # 검색어와 검색할 페이지수 입력
    result = {}
    keyword = keyword
    value = value
    for i in range(value):
        params = {
                'query': keyword,
                'where': 'post',
                'start': i,
            }        
        html = requests.get(url, params=params, headers=headers).text
        soup = BeautifulSoup(html, 'html.parser')
            
        for tag in soup.select('.sh_blog_title'):
            if tag.text not in result:		# 중복시 저장 되지 않음
                result[tag.text] = tag['href']
    return result

search_keyword('웹툰고수', 20)
```



