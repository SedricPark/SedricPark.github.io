---
categories: crawling
---

### 웹크롤링3

#### 이미지 크롤링(Pillow 사용)

* 이미지 다운로드 받기
  * requests의 response.content는 응답 원본 bytes 타입

```python
import os
from PIL import Image
import requests

image_url = ('https://www.google.co.kr/images/branding/google'
			'logo/2x/googlelogo_color_272x92dp.png')
image = requests.get(image_url).content # 서버응답 받아 파일 내용 획득
filename = os.path.basename(image_url) # url에서 파일명 획득
with open(filename, 'wb') as f:
    f.write(image) # 파일에 저장
    
# 주피터 노트북에서 확인 하는 방법
from IPython.display import Image
Image(filename='googlelogo_color_272x92dp.png')
```

* 이미지 품질 변경 / 포멧 변경하기

```python
from PIL import Image

WHITE = (255, 255, 255) # RGB color
with Image.open('python.png').convert('RGBA') as im:
    with Image.new('RGBA', im.size, WHITE) as canvas: # 파일 새로 생성
        merged_im = Image.alpha_composite(canvas, im) # 파일 합치기
        # jpg 파일 형태로 바꾸기 위해서는 'RGB'형태로 변경 jpg는 RGBA 미지원
        convert_im = merged_im.convert('RGB')
        convert_im.save('python_white.jpg', quality=100)
        # quality는 jpg만 지원하며, 0 ~ 100까지 설정
```

* 이미지를 용량으로 줄이기
  * resize(size, resample=0)
    * 리사이징된 이미지 복사본을 생성
    * 원본 가로/세로 비율 무시, 지정 크기로 강제 리사이징
  * thumbnail(sie, resample=0)
    * 원본 **이미지 객체**를 변경
    * 원본의 가로/세로 비율 유지하면서 지정 크기로 리사이징

```python
from PIL import Image

with Image.open('python.png') as im:
    print('current size : {}'.format(im.size))
    im.thumbnail((300, 300)) # 튜플 형태로 인자가 하나만 들어감, 원본 변경 
    im.save('python_thumb.png')
```

* 이미지 이어 붙이기

```python
from PIL import Image

WHITE = (255, 255, 255)
with Image.open('python1.jpg') as im1:
    with Image.open('python2.jpg') as im2:
        width = max(im1.width, im2.width) # 가로 넓이는 큰 이미지 기준
        height = sum((im1.height, im2.height)) # 높이는 두개 합친 기준, 
        # sum() 함수는 순회가능한 객체를 받기 때문에 sum((1, 2))튜플을 사용
        size = (width, height)
        
        with Image.new('RGB', size, WHITE) as canvas: # 새로운 캔버스 생성
            canvas.paste(im1, box=(0, 0)) # 0, 0은 왼쪽, 위쪽을 기준
            canvas.paste(im2, box=(0, im1.height)) # im1이 끝나는 시점
            canvas.save('canvas.jpg')
```