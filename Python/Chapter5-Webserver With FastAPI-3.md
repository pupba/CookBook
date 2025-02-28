## 3. Uvicorn

Uvicorn은 FastAPI 애플리케이션을 실행하기 위한 **ASGI(Asynchronous Server Gateway Interface)** 서버입니다. FastAPI로 작성된 코드는 단독으로 실행될 수 없으며, Uvicorn과 같은 ASGI 서버가 필요합니다.

### Uvicorn의 역할

Uvicorn은 다음과 같은 역할을 수행합니다:

- HTTP 요청을 ASGI 프로토콜에 맞게 변환
- FastAPI 애플리케이션에 요청 전달
- 애플리케이션의 응답을 HTTP 응답으로 변환하여 클라이언트에 전송


### 기본 사용법

Uvicorn을 설치하고 FastAPI 애플리케이션을 실행하는 방법:

```bash
# 설치
pip install "uvicorn[standard]"

# 실행
uvicorn main:app --reload
```

여기서:

- `main`: Python 파일 이름 (main.py)
- `app`: FastAPI 인스턴스 변수 이름
- `--reload`: 코드 변경 시 자동으로 서버 재시작 (개발 환경에서만 사용)


### 프로덕션 환경 설정

프로덕션 환경에서는 다음과 같이 설정할 수 있습니다:

```bash
uvicorn main:app --host 0.0.0.0 --port 80 --workers 4
```

매개변수 설명:

- `--host 0.0.0.0`: 모든 네트워크 인터페이스에서 접속 허용
- `--port 80`: 80번 포트 사용
- `--workers 4`: 4개의 워커 프로세스 생성


### Python 코드에서 Uvicorn 실행

Python 코드 내에서 직접 Uvicorn을 실행할 수도 있습니다:

```python
import uvicorn
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello World"}

if __name__ == "__main__":
    uvicorn.run("main:app", host="0.0.0.0", port=8000, reload=True)
```

또는 비동기 방식으로:

```python
import asyncio
import uvicorn
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello World"}

async def main():
    config = uvicorn.Config("main:app", port=8000, log_level="info")
    server = uvicorn.Server(config)
    await server.serve()

if __name__ == "__main__":
    asyncio.run(main())
```


### Gunicorn과 함께 사용

대규모 프로덕션 환경에서는 Gunicorn과 Uvicorn을 함께 사용하는 것이 좋습니다. Gunicorn은 프로세스 관리자 역할을 하며, 비정상적인 상황에서 회복 능력이 뛰어납니다:

```bash
gunicorn main:app --workers 4 --worker-class uvicorn.workers.UvicornWorker --bind 0.0.0.0:80
```

이 방식에서 Gunicorn은 Uvicorn 워커 프로세스의 매니저 역할을 합니다.

### 성능 최적화

Uvicorn은 `uvloop`라는 고성능 이벤트 루프를 사용하여 성능을 향상시킵니다. `uvicorn[standard]`를 설치하면 이러한 최적화 라이브러리가 함께 설치됩니다.
