## 6. 정적 파일 제공

FastAPI에서 정적 파일(CSS, JavaScript, 이미지 등)을 제공하는 방법을 알아보겠습니다. 웹 애플리케이션을 개발할 때 정적 파일을 효율적으로 제공하는 것은 중요합니다.

### StaticFiles 마운트하기

FastAPI는 Starlette의 `StaticFiles` 클래스를 사용하여 정적 파일을 제공합니다:

```python
from fastapi import FastAPI
from fastapi.staticfiles import StaticFiles

app = FastAPI()

# /static URL 경로에 정적 파일 디렉토리 마운트
app.mount("/static", StaticFiles(directory="static"), name="static")
```

이 코드는 프로젝트 루트의 `static` 디렉토리에 있는 파일을 `/static` URL 경로를 통해 제공합니다.

### 디렉토리 구조

일반적인 프로젝트 구조는 다음과 같습니다:

```
my_project/
├── main.py
├── static/
│   ├── css/
│   │   └── styles.css
│   ├── js/
│   │   └── script.js
│   └── images/
│       └── logo.png
└── templates/
    └── index.html
```


### 정적 파일 접근하기

정적 파일에 접근하는 URL은 다음과 같습니다:

- CSS 파일: `http://localhost:8000/static/css/styles.css`
- JavaScript 파일: `http://localhost:8000/static/js/script.js`
- 이미지 파일: `http://localhost:8000/static/images/logo.png`


### 템플릿에서 정적 파일 사용하기

Jinja2 템플릿 엔진과 함께 사용할 경우:

```html
<!DOCTYPE html>
<html>
<head>
    <title>FastAPI 예제</title>
    <link rel="stylesheet" href="/static/css/styles.css">
    <script src="/static/js/script.js" defer></script>
</head>
<body>
    <img src="/static/images/logo.png" alt="로고">
    <h1>{{ title }}</h1>
</body>
</html>
```


### 여러 정적 파일 디렉토리 마운트하기

여러 정적 파일 디렉토리를 다른 경로에 마운트할 수 있습니다:

```python
from fastapi import FastAPI
from fastapi.staticfiles import StaticFiles

app = FastAPI()

# 여러 정적 파일 디렉토리 마운트
app.mount("/static", StaticFiles(directory="static"), name="static")
app.mount("/media", StaticFiles(directory="media"), name="media")
app.mount("/assets", StaticFiles(directory="assets"), name="assets")
```


### 루트 경로에 파비콘 제공하기

웹사이트의 파비콘을 제공하려면 별도의 `StaticFiles` 인스턴스를 사용합니다:

```python
from fastapi import FastAPI
from fastapi.staticfiles import StaticFiles
from fastapi.responses import FileResponse
import os

app = FastAPI()

# 정적 파일 디렉토리 마운트
app.mount("/static", StaticFiles(directory="static"), name="static")

# 파비콘 제공
@app.get("/favicon.ico")
async def favicon():
    return FileResponse(os.path.join("static", "favicon.ico"))
```


### 정적 파일 캐싱

성능 최적화를 위해 캐싱 헤더를 설정할 수 있습니다:

```python
from fastapi import FastAPI
from fastapi.staticfiles import StaticFiles
from starlette.staticfiles import StaticFiles as StarletteStaticFiles

class CachingStaticFiles(StarletteStaticFiles):
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.cache_control = "public, max-age=604800, immutable"

app = FastAPI()

# 캐싱이 적용된 정적 파일 마운트
app.mount("/static", CachingStaticFiles(directory="static"), name="static")
```

이 설정은 브라우저에게 정적 파일을 1주일(604800초) 동안 캐싱하도록 지시합니다.

