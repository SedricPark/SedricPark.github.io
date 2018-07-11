---
categories: javascript
---

### Javascript 실습방법과 실습 환경

#### hello world 

* sample.html 파일 만들어서 'hello world' alert 창 띄우기

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
<script>
  alert('hello world');
</script>
</body>
</html>
```

#### 크롬브라우저 개발자 도구 사용

* 크롬 개발자 도구 열기 : `cmd + opt + i`
  * 콘솔창으로 이동
  * 위에서 작성한 sample.html 파일 열기
* 크롬 콘솔창에 'hello world' 띄우기
  * 개발 할 때 마다 alert를 사용하는것은..

```html
<html lang="en">
<head>
  <title>Document</title>
</head>
<body>
<script>
  console.log('hello world')
</script>
</body>
</html>
```

#### text editor 

* ATOM이나 WebStorm 사용