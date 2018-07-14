---
categories: javacript
---

### Javascript 모듈

#### 모듈

* 모듈 호출과 사용

```javascript
// 헤더 부분에 호출 js 파일 선언
// greeting.js 파일 생성
<head>
    <script src="greeting.js"></script>
</head>

// greeting.js 파일 작성
function welcome() {
    return 'hello world'
}

// 모듈 사용
script>
    document.write(welcome());
</script>
```

#### 라이브러리

* 대표적인 라이브러리 : jQuery

```javascript
// jQuery 설치
npm install jquery // node.js가 설치되어 있어야 npm 사용 가능

// jquery 호출
<head>
    <script src="./node_modules/jquery/dist/jquery.js"></script>
</head>

// body에서 jqery 사용
<body>
    <ul id="list">
        <li>empty</li>
        <li>empty</li>
        <li>empty</li>
    </ul>
    <input type="button" id="execute_btn" value="execute">
    <script type="text/javascript">
        $('#execute_btn').click(function () {
            $('#list li').text('hello world');
        });
    </script>
</body>
```

