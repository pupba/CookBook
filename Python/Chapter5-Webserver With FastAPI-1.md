## A basic tutorial for the Webserver (using Fast API) 
### Contents

- 1. default use
- 2. Pydantic 모델링
- 3. Uvicorn
- 4. 커스텀 예외처리
- 5. 라우팅
- 6. 정적파일 제공
- 7. 백그라운드 테스크

## 1. 기본 사용법

FastAPI는 Python으로 작성된 현대적이고 빠른 웹 프레임워크로, 자동 문서화 기능과 타입 힌트를 활용한 데이터 검증을 제공합니다.

### 설치하기

먼저 FastAPI와 ASGI 서버인 Uvicorn을 설치합니다:

```bash
pip install fastapi uvicorn
```


### 첫 번째 API 만들기

다음은 가장 기본적인 FastAPI 애플리케이션입니다:

```python
from fastapi import FastAPI

# FastAPI 인스턴스 생성
app = FastAPI()

# 루트 경로("/")에 대한 GET 요청 처리
@app.get("/")
async def root():
    return {"message": "안녕하세요! FastAPI에 오신 것을 환영합니다!"}
```


### 서버 실행하기

위 코드를 `main.py`로 저장한 후, 다음 명령어로 서버를 실행합니다:

```bash
uvicorn main:app --reload
```

이제 브라우저에서 `http://127.0.0.1:8000`에 접속하면 JSON 응답을 확인할 수 있습니다.

### 경로 매개변수

URL에서 값을 받아오는 경로 매개변수를 정의할 수 있습니다:

```python
@app.get("/items/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id}
```


### 쿼리 매개변수

URL의 `?` 뒤에 오는 쿼리 매개변수도 쉽게 처리할 수 있습니다:

```python
@app.get("/items/")
async def read_items(skip: int = 0, limit: int = 10):
    return {"skip": skip, "limit": limit}
```


### 자동 문서화

FastAPI는 자동으로 API 문서를 생성합니다:

- **Swagger UI** : `http://127.0.0.1:8000/docs`
- **ReDoc** : `http://127.0.0.1:8000/redoc`


