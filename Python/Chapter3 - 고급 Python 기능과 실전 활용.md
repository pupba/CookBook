## Chapter3 - 고급 Python 기능과 실전 활용

### 제너레이터와 이터레이터

제너레이터는 이터레이터를 생성하는 함수로, `yield` 키워드를 사용하여 값을 하나씩 반환합니다. 메모리 효율성이 높아 대용량 데이터를 처리할 때 유용합니다.

```python
def count_up_to(max: int) -> Iterator[int]:
    """
    0부터 max-1까지의 숫자를 생성하는 제너레이터
    
    Args:
        max: 최대값(미포함)
        
    Yields:
        0부터 max-1까지의 정수
    """
    count = 0
    while count < max:
        yield count
        count += 1

# 제너레이터 사용
for number in count_up_to(5):
    print(number)  # 0, 1, 2, 3, 4

# 제너레이터 표현식
squares = (x**2 for x in range(5))
print(list(squares))  # [0, 1, 4, 9, 16]
```


### 데코레이터

데코레이터는 함수나 메서드의 기능을 수정하거나 확장하는 데 사용됩니다:

```python
import time
from functools import wraps
from typing import Callable, TypeVar, Any, ParamSpec

P = ParamSpec('P')  # 파라미터 사양을 위한 타입 변수
R = TypeVar('R')    # 반환 타입을 위한 타입 변수

def timing_decorator(func: Callable[P, R]) -> Callable[P, R]:
    """
    함수 실행 시간을 측정하는 데코레이터
    
    Args:
        func: 측정할 함수
        
    Returns:
        시간 측정 기능이 추가된 함수
    """
    @wraps(func)
    def wrapper(*args: P.args, **kwargs: P.kwargs) -> R:
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"{func.__name__} 함수 실행 시간: {end_time - start_time:.4f}초")
        return result
    return wrapper

@timing_decorator
def slow_function(n: int) -> list[int]:
    """
    시간이 오래 걸리는 함수 예시
    
    Args:
        n: 계산할 숫자 범위
        
    Returns:
        계산 결과 리스트
    """
    result = []
    for i in range(n):
        result.append(i ** 2)
        time.sleep(0.01)  # 의도적인 지연
    return result

# 데코레이터가 적용된 함수 호출
result = slow_function(100)
# slow_function 함수 실행 시간: 1.0123초
```


### 클래스 데코레이터

클래스에도 데코레이터를 적용할 수 있습니다:

```python
def singleton(cls):
    """
    클래스를 싱글톤으로 만드는 데코레이터
    
    Args:
        cls: 싱글톤으로 만들 클래스
        
    Returns:
        싱글톤 패턴이 적용된 클래스
    """
    instances = {}
    
    def get_instance(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]
    
    return get_instance

@singleton
class DatabaseConnection:
    def __init__(self, host: str, port: int):
        self.host = host
        self.port = port
        print(f"데이터베이스 연결: {host}:{port}")
    
    def query(self, sql: str) -> list[dict]:
        print(f"SQL 실행: {sql}")
        return [{"id": 1, "name": "홍길동"}]

# 싱글톤 인스턴스 생성 및 사용
db1 = DatabaseConnection("localhost", 5432)  # 데이터베이스 연결: localhost:5432
db2 = DatabaseConnection("localhost", 5432)  # 새로운 연결 메시지가 출력되지 않음

print(db1 is db2)  # True (동일한 인스턴스)
```


### 프로퍼티와 디스크립터

`@property` 데코레이터를 사용하여 메서드를 속성처럼 접근할 수 있습니다:

```python
class Circle:
    def __init__(self, radius: float):
        self._radius = radius
    
    @property
    def radius(self) -> float:
        """원의 반지름"""
        return self._radius
    
    @radius.setter
    def radius(self, value: float) -> None:
        if value <= 0:
            raise ValueError("반지름은 양수여야 합니다.")
        self._radius = value
    
    @property
    def area(self) -> float:
        """원의 넓이"""
        return 3.14159 * self._radius ** 2
    
    @property
    def circumference(self) -> float:
        """원의 둘레"""
        return 2 * 3.14159 * self._radius

# 프로퍼티 사용
circle = Circle(5)
print(circle.radius)  # 5
print(circle.area)    # 78.53975

# 프로퍼티 설정
circle.radius = 10
print(circle.area)    # 314.159

try:
    circle.radius = -5  # ValueError: 반지름은 양수여야 합니다.
except ValueError as e:
    print(e)
```


### 메타클래스

메타클래스는 클래스의 생성을 제어하는 "클래스의 클래스"입니다:

```python
class LoggingMeta(type):
    def __new__(mcs, name, bases, namespace):
        # 모든 메서드에 로깅 기능 추가
        for key, value in namespace.items():
            if callable(value) and not key.startswith('__'):
                namespace[key] = LoggingMeta.add_logging(value)
        
        return super().__new__(mcs, name, bases, namespace)
    
    @staticmethod
    def add_logging(method):
        @wraps(method)
        def wrapper(*args, **kwargs):
            print(f"{method.__name__} 메서드 호출")
            return method(*args, **kwargs)
        return wrapper

class Service(metaclass=LoggingMeta):
    def process(self, data: str) -> str:
        return f"처리된 데이터: {data}"
    
    def analyze(self, data: str) -> dict:
        return {"result": f"분석 결과: {data}"}

# 메타클래스가 적용된 클래스 사용
service = Service()
result = service.process("테스트")  # process 메서드 호출
print(result)  # 처리된 데이터: 테스트

analysis = service.analyze("샘플")  # analyze 메서드 호출
print(analysis)  # {'result': '분석 결과: 샘플'}
```


### 비동기 프로그래밍 (asyncio)

Python 3.5부터 `asyncio` 모듈을 통해 비동기 프로그래밍을 지원합니다:

```python
import asyncio
from typing import List

async def fetch_data(url: str) -> str:
    """
    URL에서 데이터를 비동기적으로 가져오는 함수
    
    Args:
        url: 데이터를 가져올 URL
        
    Returns:
        가져온 데이터
    """
    print(f"{url} 데이터 요청 중...")
    # 실제로는 aiohttp 같은 라이브러리를 사용하여 HTTP 요청
    await asyncio.sleep(2)  # 네트워크 지연 시뮬레이션
    return f"{url}의 데이터"

async def process_urls(urls: List[str]) -> List[str]:
    """
    여러 URL에서 동시에 데이터를 가져오는 함수
    
    Args:
        urls: 데이터를 가져올 URL 목록
        
    Returns:
        가져온 데이터 목록
    """
    tasks = [fetch_data(url) for url in urls]
    results = await asyncio.gather(*tasks)
    return results

# 비동기 함수 실행
async def main():
    urls = [
        "https://example.com/api/data1",
        "https://example.com/api/data2",
        "https://example.com/api/data3"
    ]
    results = await process_urls(urls)
    for result in results:
        print(result)

# Python 3.7 이상에서는 asyncio.run() 사용
import asyncio
asyncio.run(main())
```


### 타입 어노테이션 고급 기능

#### 제네릭 타입

```python
from typing import TypeVar, Generic, List

T = TypeVar('T')  # 타입 변수 정의

class Stack(Generic[T]):
    def __init__(self):
        self.items: List[T] = []
    
    def push(self, item: T) -> None:
        self.items.append(item)
    
    def pop(self) -> T:
        if not self.items:
            raise IndexError("스택이 비어있습니다")
        return self.items.pop()
    
    def peek(self) -> T:
        if not self.items:
            raise IndexError("스택이 비어있습니다")
        return self.items[-1]
    
    def is_empty(self) -> bool:
        return len(self.items) == 0

# 정수 스택
int_stack: Stack[int] = Stack()
int_stack.push(1)
int_stack.push(2)
print(int_stack.pop())  # 2

# 문자열 스택
str_stack: Stack[str] = Stack()
str_stack.push("hello")
str_stack.push("world")
print(str_stack.pop())  # world
```


#### Union 타입과 Optional 타입

```python
from typing import Union, Optional

# Union 타입: 여러 타입 중 하나
def process_input(data: Union[str, int, float]) -> str:
    return f"처리된 데이터: {data}"

# Optional 타입: 값이 있거나 None
def find_user(user_id: int) -> Optional[dict]:
    users = {1: {"name": "홍길동"}, 2: {"name": "김철수"}}
    return users.get(user_id)  # 사용자가 없으면 None 반환

# Python 3.10부터는 Union 대신 | 연산자 사용 가능
def process_input_new(data: str | int | float) -> str:
    return f"처리된 데이터: {data}"
```


#### Literal 타입

```python
from typing import Literal

# 특정 리터럴 값만 허용
def set_alignment(align: Literal["left", "center", "right"]) -> None:
    print(f"텍스트 정렬: {align}")

# 올바른 사용
set_alignment("left")    # 텍스트 정렬: left
set_alignment("center")  # 텍스트 정렬: center

# 잘못된 사용 (타입 검사기에서 오류 발생)
# set_alignment("top")  # Error: Argument of type "top" cannot be assigned to parameter "align"
```


#### Protocol 클래스 (구조적 서브타이핑)

```python
from typing import Protocol, List

class Drawable(Protocol):
    def draw(self) -> None:
        ...

class Circle:
    def draw(self) -> None:
        print("원 그리기")

class Square:
    def draw(self) -> None:
        print("사각형 그리기")

class Triangle:
    def draw(self) -> None:
        print("삼각형 그리기")

def draw_shapes(shapes: List[Drawable]) -> None:
    for shape in shapes:
        shape.draw()

# Protocol을 명시적으로 구현하지 않아도 메서드만 일치하면 사용 가능
shapes = [Circle(), Square(), Triangle()]
draw_shapes(shapes)
# 원 그리기
# 사각형 그리기
# 삼각형 그리기
```


### 컨텍스트 관리자 프로토콜

`__enter__`와 `__exit__` 메서드를 구현하여 `with` 문에서 사용할 수 있는 컨텍스트 관리자를 만들 수 있습니다:

```python
class Timer:
    def __enter__(self):
        self.start = time.time()
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        self.end = time.time()
        self.elapsed = self.end - self.start
        print(f"실행 시간: {self.elapsed:.4f}초")

# 컨텍스트 관리자 사용
with Timer():
    # 시간을 측정할 코드
    time.sleep(1.5)
    print("작업 수행 중...")
# 작업 수행 중...
# 실행 시간: 1.5012초
```


### 데이터 클래스

Python 3.7부터 도입된 `dataclasses` 모듈은 데이터를 저장하는 클래스를 쉽게 만들 수 있게 해줍니다:

```python
from dataclasses import dataclass, field
from typing import List

@dataclass
class Point:
    x: float
    y: float
    
    def distance_from_origin(self) -> float:
        return (self.x ** 2 + self.y ** 2) ** 0.5

@dataclass
class Student:
    name: str
    age: int
    grades: List[int] = field(default_factory=list)
    active: bool = True
    
    def average_grade(self) -> float:
        if not self.grades:
            return 0.0
        return sum(self.grades) / len(self.grades)

# 데이터 클래스 사용
p = Point(3.0, 4.0)
print(p)  # Point(x=3.0, y=4.0)
print(p.distance_from_origin())  # 5.0

s = Student("홍길동", 20, [85, 90, 95])
print(s)  # Student(name='홍길동', age=20, grades=[85, 90, 95], active=True)
print(s.average_grade())  # 90.0
```


### 열거형 (Enum)

`enum` 모듈을 사용하여 열거형을 정의할 수 있습니다:

```python
from enum import Enum, auto

class Color(Enum):
    RED = 1
    GREEN = 2
    BLUE = 3

class Status(Enum):
    PENDING = auto()
    RUNNING = auto()
    COMPLETED = auto()
    FAILED = auto()

# 열거형 사용
print(Color.RED)  # Color.RED
print(Color.RED.name)  # RED
print(Color.RED.value)  # 1

# 열거형 반복
for color in Color:
    print(f"{color.name}: {color.value}")
# RED: 1
# GREEN: 2
# BLUE: 3

# 열거형 비교
current_status = Status.RUNNING
if current_status == Status.RUNNING:
    print("작업이 실행 중입니다.")
```


### 타입 검사 도구 (mypy)

Python은 동적 타입 언어이지만, `mypy`와 같은 도구를 사용하여 정적 타입 검사를 수행할 수 있습니다:

```python
# example.py
def add(a: int, b: int) -> int:
    return a + b

result = add("hello", "world")  # 타입 오류
```

터미널에서 다음 명령을 실행하여 타입 검사를 수행할 수 있습니다:

```bash
pip install mypy
mypy example.py
```

결과:

```
example.py:4: error: Argument 1 to "add" has incompatible type "str"; expected "int"
example.py:4: error: Argument 2 to "add" has incompatible type "str"; expected "int"
```

