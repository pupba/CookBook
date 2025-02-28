## 7. 백그라운드 태스크

FastAPI에서는 백그라운드 태스크를 사용하여 HTTP 응답을 반환한 후에도 작업을 계속 실행할 수 있습니다. 이는 이메일 전송, 데이터 처리, 외부 API 호출과 같이 시간이 오래 걸리는 작업에 유용합니다.

### 기본 백그라운드 태스크

FastAPI에서 백그라운드 태스크를 생성하는 가장 간단한 방법은 `BackgroundTasks` 객체를 사용하는 것입니다:

```python
from fastapi import FastAPI, BackgroundTasks

app = FastAPI()

def write_notification(email: str, message: str):
    with open("log.txt", "a") as log:
        log.write(f"{email}: {message}\n")

@app.post("/send-notification/{email}")
async def send_notification(email: str, background_tasks: BackgroundTasks):
    background_tasks.add_task(write_notification, email, "환영합니다!")
    return {"message": "알림이 전송될 예정입니다"}
```

이 예제에서는 `/send-notification/{email}` 엔드포인트가 즉시 응답을 반환하고, 그 후에 `write_notification` 함수가 백그라운드에서 실행됩니다.

### 여러 백그라운드 태스크 추가하기

하나의 요청 핸들러에 여러 백그라운드 태스크를 추가할 수 있습니다:

```python
from fastapi import FastAPI, BackgroundTasks

app = FastAPI()

def write_log(message: str):
    with open("log.txt", "a") as log:
        log.write(f"{message}\n")

def send_email(email: str, message: str):
    # 이메일 전송 로직
    pass

@app.post("/users/")
async def create_user(email: str, background_tasks: BackgroundTasks):
    background_tasks.add_task(write_log, f"사용자 생성: {email}")
    background_tasks.add_task(send_email, email, "가입을 환영합니다!")
    return {"message": "사용자가 생성되었습니다"}
```


### 의존성 주입과 함께 사용하기

백그라운드 태스크는 의존성 주입 시스템과 함께 사용할 수 있습니다:

```python
from fastapi import FastAPI, BackgroundTasks, Depends

app = FastAPI()

def get_db():
    # 데이터베이스 연결 로직
    db = Database()
    try:
        yield db
    finally:
        db.close()

def process_item(item_id: int, db: Database):
    # 데이터베이스를 사용한 아이템 처리 로직
    db.process_item(item_id)

@app.post("/items/{item_id}")
async def create_item(
    item_id: int,
    background_tasks: BackgroundTasks,
    db: Database = Depends(get_db)
):
    background_tasks.add_task(process_item, item_id, db)
    return {"message": "아이템 처리가 시작되었습니다"}
```


### 장기 실행 태스크 처리하기

백그라운드 태스크는 요청 수명 주기와 연결되어 있으므로, 매우 오래 실행되는 작업에는 적합하지 않을 수 있습니다. 장기 실행 태스크의 경우 Celery나 RQ와 같은 작업 큐를 사용하는 것이 좋습니다:

```python
from fastapi import FastAPI
from celery_worker import process_data_task

app = FastAPI()

@app.post("/process-data/")
async def process_data(data: dict):
    # Celery 태스크 시작
    task = process_data_task.delay(data)
    return {"task_id": task.id, "message": "데이터 처리가 시작되었습니다"}

@app.get("/task-status/{task_id}")
async def get_task_status(task_id: str):
    # Celery 태스크 상태 확인
    task = process_data_task.AsyncResult(task_id)
    return {"task_id": task_id, "status": task.status, "result": task.result}
```


### 비동기 백그라운드 태스크

백그라운드 태스크는 비동기 함수일 수도 있습니다:

```python
from fastapi import FastAPI, BackgroundTasks
import asyncio

app = FastAPI()

async def send_email_async(email: str, message: str):
    # 비동기 이메일 전송 로직
    await asyncio.sleep(10)  # 비동기 작업 시뮬레이션
    print(f"이메일 전송 완료: {email}, 메시지: {message}")

@app.post("/send-email/{email}")
async def send_email(email: str, background_tasks: BackgroundTasks):
    background_tasks.add_task(send_email_async, email, "중요한 알림입니다")
    return {"message": "이메일이 백그라운드에서 전송됩니다"}
```

백그라운드 태스크를 사용하면 사용자 경험을 향상시키고 API의 응답 시간을 줄일 수 있습니다. 그러나 매우 복잡하거나 장기 실행 작업의 경우 전용 작업 큐(celery, rabbitmq, redis, kafka) 시스템을 고려하는 것이 좋습니다.

