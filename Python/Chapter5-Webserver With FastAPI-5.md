## 5. 라우팅

FastAPI에서 라우팅은 URL 경로를 특정 함수에 연결하는 메커니즘입니다. 대규모 애플리케이션에서는 코드를 구조화하고 유지보수하기 쉽게 만들기 위해 라우터를 사용하여 경로를 그룹화할 수 있습니다.

### 기본 라우팅

FastAPI에서 기본 라우팅은 데코레이터를 사용하여 정의합니다:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "메인 페이지입니다"}

@app.get("/items/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id}
```


### APIRouter 사용하기

대규모 애플리케이션에서는 `APIRouter` 클래스를 사용하여 경로를 모듈화할 수 있습니다:

```python
from fastapi import APIRouter, FastAPI

app = FastAPI()

# 라우터 생성
router = APIRouter(
    prefix="/items",
    tags=["items"],
    responses={404: {"description": "아이템을 찾을 수 없습니다"}},
)

@router.get("/")
async def read_items():
    return [{"name": "Item 1"}, {"name": "Item 2"}]

@router.get("/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id, "name": f"Item {item_id}"}

# 메인 앱에 라우터 포함
app.include_router(router)
```


### 여러 라우터 구성하기

프로젝트를 모듈로 나누어 각 기능별로 라우터를 만들 수 있습니다:

```python
# users.py
from fastapi import APIRouter

router = APIRouter(
    prefix="/users",
    tags=["users"],
    responses={404: {"description": "사용자를 찾을 수 없습니다"}},
)

@router.get("/")
async def read_users():
    return [{"username": "user1"}, {"username": "user2"}]

@router.get("/{username}")
async def read_user(username: str):
    return {"username": username}

# items.py
from fastapi import APIRouter

router = APIRouter(
    prefix="/items",
    tags=["items"],
    responses={404: {"description": "아이템을 찾을 수 없습니다"}},
)

@router.get("/")
async def read_items():
    return [{"name": "Item 1"}, {"name": "Item 2"}]

# main.py
from fastapi import FastAPI
from users import router as users_router
from items import router as items_router

app = FastAPI()

app.include_router(users_router)
app.include_router(items_router)
```


### 중첩 라우터

라우터 안에 다른 라우터를 포함시킬 수도 있습니다:

```python
from fastapi import APIRouter, FastAPI

app = FastAPI()

# 상위 라우터
users_router = APIRouter(
    prefix="/users",
    tags=["users"],
)

# 하위 라우터
items_router = APIRouter(
    prefix="/{user_id}/items",
    tags=["items"],
    responses={404: {"description": "아이템을 찾을 수 없습니다"}},
)

@items_router.get("/")
async def read_user_items(user_id: int):
    return [{"user_id": user_id, "item_id": 1}, {"user_id": user_id, "item_id": 2}]

@items_router.get("/{item_id}")
async def read_user_item(user_id: int, item_id: int):
    return {"user_id": user_id, "item_id": item_id}

# 하위 라우터를 상위 라우터에 포함
users_router.include_router(items_router)

# 상위 라우터를 앱에 포함
app.include_router(users_router)
```

이 구조를 사용하면 `/users/{user_id}/items/` 및 `/users/{user_id}/items/{item_id}` 경로가 생성됩니다.

### 경로 작업 매개변수

라우터에 추가 매개변수를 지정할 수 있습니다:

```python
from fastapi import APIRouter, Depends, HTTPException, status
from typing import Callable

def get_token_header(token: str = Header(...)):
    if token != "secret-token":
        raise HTTPException(status_code=status.HTTP_403_FORBIDDEN)
    return token

router = APIRouter(
    prefix="/items",
    tags=["items"],
    dependencies=[Depends(get_token_header)],
    responses={404: {"description": "아이템을 찾을 수 없습니다"}},
)

@router.get("/")
async def read_items():
    return [{"name": "Item 1"}, {"name": "Item 2"}]
```

이렇게 구성된 라우터의 모든 경로는 `get_token_header` 의존성을 공유합니다.

라우팅을 효과적으로 구성하면 코드의 가독성과 유지보수성이 향상되며, 대규모 애플리케이션에서 특히 유용합니다.
