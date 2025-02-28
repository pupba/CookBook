## 4. 커스텀 예외 처리

FastAPI에서는 기본적으로 `HTTPException`을 사용하여 오류를 처리하지만, 더 세밀한 제어가 필요한 경우 커스텀 예외 클래스와 핸들러를 정의할 수 있습니다.

### 기본 커스텀 예외 클래스 만들기

먼저 기본 커스텀 예외 클래스를 정의합니다:

```python
from fastapi import FastAPI, Request, status
from fastapi.responses import JSONResponse

class BaseCustomException(Exception):
    """모든 커스텀 예외의 기본 클래스"""
    
    def __init__(self, status_code: int, detail: str, code: str = None):
        self.status_code = status_code
        self.detail = detail
        self.code = code
        
    def __str__(self):
        return self.detail
```


### 구체적인 예외 클래스 만들기

기본 예외 클래스를 상속받아 구체적인 예외 클래스를 정의합니다:

```python
class ItemNotFoundException(BaseCustomException):
    def __init__(self, item_id: str):
        super().__init__(
            status_code=status.HTTP_404_NOT_FOUND,
            detail=f"Item with ID {item_id} not found",
            code="ITEM_NOT_FOUND"
        )

class UnauthorizedAccessException(BaseCustomException):
    def __init__(self, resource: str):
        super().__init__(
            status_code=status.HTTP_403_FORBIDDEN,
            detail=f"Unauthorized access to {resource}",
            code="UNAUTHORIZED_ACCESS"
        )
```


### 예외 핸들러 등록하기

FastAPI 애플리케이션에 예외 핸들러를 등록합니다:

```python
app = FastAPI()

@app.exception_handler(BaseCustomException)
async def custom_exception_handler(request: Request, exc: BaseCustomException):
    """모든 커스텀 예외를 처리하는 핸들러"""
    response_content = {"detail": exc.detail}
    
    # 코드가 있으면 응답에 추가
    if exc.code:
        response_content["code"] = exc.code
        
    return JSONResponse(
        status_code=exc.status_code,
        content=response_content
    )
```


### 기본 예외 핸들러 재정의하기

FastAPI의 기본 예외 핸들러도 재정의할 수 있습니다:

```python
from fastapi.exceptions import RequestValidationError
from starlette.exceptions import HTTPException as StarletteHTTPException

@app.exception_handler(StarletteHTTPException)
async def http_exception_handler(request: Request, exc: StarletteHTTPException):
    """기본 HTTP 예외 핸들러 재정의"""
    return JSONResponse(
        status_code=exc.status_code,
        content={
            "detail": exc.detail,
            "code": "HTTP_ERROR"
        }
    )

@app.exception_handler(RequestValidationError)
async def validation_exception_handler(request: Request, exc: RequestValidationError):
    """요청 검증 예외 핸들러 재정의"""
    return JSONResponse(
        status_code=status.HTTP_422_UNPROCESSABLE_ENTITY,
        content={
            "detail": str(exc),
            "code": "VALIDATION_ERROR",
            "errors": exc.errors()
        }
    )
```


### 예외 사용 예시

라우트 핸들러에서 커스텀 예외를 발생시키는 예:

```python
@app.get("/items/{item_id}")
async def read_item(item_id: str):
    items = {"1": "Item 1", "2": "Item 2"}
    
    if item_id not in items:
        raise ItemNotFoundException(item_id)
        
    return {"item": items[item_id]}

@app.get("/secure-resource")
async def access_secure_resource(token: str = None):
    if not token or token != "valid_token":
        raise UnauthorizedAccessException("secure-resource")
        
    return {"message": "Access granted to secure resource"}
```

이러한 접근 방식을 통해 일관된 오류 응답 형식을 유지하면서 다양한 오류 상황을 세밀하게 처리할 수 있습니다.

