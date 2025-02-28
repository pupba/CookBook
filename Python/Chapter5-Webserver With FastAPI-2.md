## 2. Pydantic 모델링

Pydantic은 데이터 검증과 설정 관리를 위한 라이브러리로, FastAPI와 함께 사용하면 요청 및 응답 데이터의 유효성을 자동으로 검사할 수 있습니다.

### 기본 모델 정의

Pydantic 모델은 클래스로 정의하며, 각 필드에 타입 어노테이션을 추가합니다:

```python
from pydantic import BaseModel
from typing import Optional, List

class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None
    tags: List[str] = []
```


### 요청 본문 검증

Pydantic 모델을 사용하여 요청 본문을 자동으로 검증할 수 있습니다:

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    price: float
    is_offer: bool = False

@app.post("/items/")
async def create_item(item: Item):
    return {"item_name": item.name, "item_price": item.price}
```

클라이언트가 잘못된 데이터를 보내면 FastAPI는 자동으로 오류 응답을 반환합니다.

### 응답 모델 정의

`response_model` 매개변수를 사용하여 응답 데이터의 형식을 정의할 수 있습니다:

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    price: float

class ItemResponse(BaseModel):
    name: str
    price_with_tax: float

@app.post("/items/", response_model=ItemResponse)
async def create_item(item: Item):
    return {"name": item.name, "price_with_tax": item.price * 1.1}
```


### 모델 상속

Pydantic 모델은 상속을 통해 확장할 수 있습니다:

```python
from pydantic import BaseModel
from typing import Optional

class ItemBase(BaseModel):
    name: str
    description: Optional[str] = None

class ItemCreate(ItemBase):
    price: float

class Item(ItemBase):
    id: int
    price: float

    class Config:
        orm_mode = True
```


### 유효성 검사

Pydantic은 필드 제약 조건을 정의할 수 있는 다양한 검증기를 제공합니다:

```python
from pydantic import BaseModel, Field, EmailStr

class User(BaseModel):
    username: str = Field(..., min_length=3, max_length=50)
    email: EmailStr
    age: int = Field(..., gt=0, lt=120)
```


### 중첩 모델

모델 안에 다른 모델을 중첩하여 복잡한 데이터 구조를 표현할 수 있습니다:

```python
from pydantic import BaseModel
from typing import List

class Tag(BaseModel):
    name: str
    value: str

class Item(BaseModel):
    name: str
    tags: List[Tag] = []
```

이렇게 Pydantic을 사용하면 데이터 검증 로직을 간소화하고, API의 입출력 데이터 형식을 명확하게 정의할 수 있습니다.

