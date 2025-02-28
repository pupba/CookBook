## Chapter4 - 예외 처리 기능과 최적화

### 예외처리

예외 처리란 프로그래밍에서 오류를 handling 할때 사용하는 기능입니다.

적절한 예외처리를 통해 프로그램이 예상치 못한 상황에서도 안정적으로 실행 될 수 있습니다.

#### 기본 구조 : try-except 블록

Python에서 예외 처리의 기본 구조는 `try-except` 블록입니다.
```Python
try:
    # 예외가 발생할 수 있는 코드
    result = 10 / 0
except ZeroDivisionError:
    # 특정 예외 처리
    print("0으로 나눌 수 없습니다")

```

`try` 절 내부의 코드가 실행되며, 예외가 발생하지 않으면 `except` 절은 실행되지 않습니다.

하지만 예외가 발생하면 해당 예외 유형과 일치하는 `except` 블록이 실행됩니다.

##### 예외 처리 모범 사례

**1. try 블록을 작고 집중적으로 유지하기**

>예외 처리를 적절히 하려면 `try` 블록을 작고 집중적으로 유지해야 합니다. 전체 코드를 하나의 거대한 `try-except` 블록으로 감싸지 말고, 잠재적으로 오류가 발생할 수 있는 부분만 포함시키세요.


**2. 구체적인 예외 잡기**

>일반적인 `Exception` 클래스 대신 구체적인 예외를 잡아 오류를 구분하세요. 이는 코드의 가독성을 높이고 디버깅을 용이하게 합니다.

```Python
# 잘못된 방법
try:
    file = open('data.txt', 'r')
    content = file.read()
except Exception:
    print("파일 읽기 중 오류가 발생했습니다.")

# 좋은 방법
try:
    file = open('data.txt', 'r')
    content = file.read()
except FileNotFoundError:
    print("파일이 존재하지 않습니다.")
except IOError:
    print("파일 접근 중 오류가 발생했습니다.")
```


**3. else와 finally 절 활용하기**

`else` 절은 예외가 발생하지 않았을 때만 실행되는 코드를 분리하여 가독성을 높입니다.

```Python
try:
    result = perform_operation()
except SomeError as e:
    print(f"오류 발생: {e}")
else:
    print(f"작업 성공: {result}")
```

`finally` 절은 예외 발생 여부와 관계없이 항상 실행되어야 하는 정리 작업에 유용합니다.
```Python
try:
    file = open('data.txt', 'r')
    content = file.read()
except FileNotFoundError:
    print("파일이 존재하지 않습니다.")
finally:
    file.close()  # 항상 파일을 닫음
```


**4. 빈 except 블록 피하기**

빈 `except` 블록(예외 유형을 지정하지 않는 것)은 모든 예외를 잡기 때문에 의도하지 않은 시스템 예외까지 잡을 수 있어 디버깅을 어렵게 만듭니다.

```Python
# 피해야 할 방법
try:
    risky_operation()
except:
    print("문제가 발생했습니다.")

# 좋은 방법
try:
    risky_operation()
except SpecificError:
    print("SpecificError를 처리했습니다.")
```


**5. 예외 체이닝 활용하기**

새 예외를 발생시킬 때 `from e` 구문을 사용하여 원래 예외의 스택 트레이스를 보존하세요. 이는 오류의 근본 원인을 추적하는 데 도움이 됩니다

```Python
try:
    # 위험한 작업
    result = 10 / 0
except ZeroDivisionError as e:
    raise ValueError("유효하지 않은 입력값") from e
```

**6. 동시 실행 코드에서 예외 그룹 사용하기**

Python 3.11에서 도입된 예외 그룹은 동시에 여러 예외가 발생할 수 있는 병렬 작업에서 특히 유용합니다.

```Python
def process_concurrent_tasks():
    try:
        # 동시에 실패할 수 있는 여러 작업
        results = run_parallel_tasks()
    except* ExceptionGroup as eg:
        # 유형별로 예외 처리
        for exc in eg.exceptions:
            if isinstance(exc, ValueError):
                handle_value_error(exc)
            elif isinstance(exc, ConnectionError):
                handle_connection_error(exc)
```

### 예외 그룹 (Exception Groups)

```python
from typing import List
import asyncio

async def fetch_url(url: str) -> str:
    """URL에서 데이터를 가져오는 비동기 함수"""
    # 실제 구현에서는 aiohttp 등을 사용
    if "error" in url:
        raise ValueError(f"Invalid URL: {url}")
    return f"Data from {url}"

async def fetch_all_urls(urls: List[str]) -> List[str]:
    """여러 URL에서 데이터를 병렬로 가져오는 함수"""
    tasks = [fetch_url(url) for url in urls]
    
    try:
        return await asyncio.gather(*tasks)
    except* ValueError as exc_group:
        # 모든 ValueError를 그룹으로 처리
        print(f"발생한 오류 개수: {len(exc_group.exceptions)}")
        for i, exc in enumerate(exc_group.exceptions):
            print(f"오류 {i+1}: {exc}")
        # 일부 오류가 있어도 성공한 결과 반환
        return [f"Error for {urls[i]}" if isinstance(e, ValueError) else e 
                for i, e in enumerate(await asyncio.gather(*tasks, return_exceptions=True))]

async def main():
    urls = [
        "https://example.com/data1",
        "https://example.com/error1",
        "https://example.com/data2",
        "https://example.com/error2"
    ]
    
    results = await fetch_all_urls(urls)
    print("결과:", results)

# Python 3.11에서 실행
if __name__ == "__main__":
    asyncio.run(main())
```


### 새로운 TOML 파서 (tomllib)

Python 3.11에서는 TOML 파일을 파싱하기 위한 표준 라이브러리 `tomllib`이 추가되었습니다:

```python
import tomllib  # Python 3.11 이상에서만 사용 가능

def load_config(config_file: str) -> dict:
    """
    TOML 설정 파일을 로드합니다.
    
    Args:
        config_file: TOML 파일 경로
        
    Returns:
        설정 딕셔너리
    """
    with open(config_file, "rb") as f:
        return tomllib.load(f)

# 사용 예시
config = load_config("config.toml")
print(f"서버 설정: {config['server']}")
print(f"데이터베이스 설정: {config['database']}")
```

예시 `config.toml` 파일:

```toml
[server]
host = "localhost"
port = 8080
debug = true

[database]
url = "postgresql://user:password@localhost/dbname"
pool_size = 5

[[users]]
name = "관리자"
role = "admin"

[[users]]
name = "사용자"
role = "user"
```


### 타입 변수의 새로운 기능

Python 3.11에서는 타입 변수에 대한 새로운 기능이 추가되었습니다:

```python
from typing import TypeVar, Generic, List, Callable

# 경계가 있는 타입 변수
T_co = TypeVar('T_co', covariant=True)
T_contra = TypeVar('T_contra', contravariant=True)

# 타입 변수에 경계 지정
NumT = TypeVar('NumT', int, float)

def add_numbers(a: NumT, b: NumT) -> NumT:
    return a + b

# 사용 예시
result1 = add_numbers(1, 2)  # OK
result2 = add_numbers(1.5, 2.5)  # OK
# result3 = add_numbers(1, "2")  # 타입 오류: "2"는 int나 float가 아님

# Self 타입
from typing import Self

class ChainableList(list[T_co]):
    def append_and_return(self, item: T_co) -> Self:
        self.append(item)
        return self

# 사용 예시
chain = ChainableList([1, 2, 3])
result = chain.append_and_return(4).append_and_return(5)
print(result)  # [1, 2, 3, 4, 5]
```


### 성능 최적화: 특수화 적응형 인터프리터

Python 3.11에서는 특수화 적응형 인터프리터(Specializing Adaptive Interpreter)가 도입되어 성능이 크게 향상되었습니다. 이 기능은 코드가 실행되는 동안 자주 사용되는 연산을 최적화합니다.

```python
def calculate_sum(n: int) -> int:
    """1부터 n까지의 합을 계산합니다."""
    total = 0
    for i in range(1, n + 1):
        total += i
    return total

# 성능 측정
import time

def measure_performance(func, *args, iterations: int = 100):
    """함수의 성능을 측정합니다."""
    start_time = time.time()
    for _ in range(iterations):
        func(*args)
    end_time = time.time()
    return end_time - start_time

# Python 3.10과 3.11 비교 (실제로는 같은 버전에서 실행할 수 없음)
time_310 = measure_performance(calculate_sum, 10000, iterations=1000)
time_311 = measure_performance(calculate_sum, 10000, iterations=1000)

print(f"Python 3.10 실행 시간: {time_310:.6f}초")
print(f"Python 3.11 실행 시간: {time_311:.6f}초")
print(f"성능 향상: {time_310 / time_311:.2f}배")
```


### 새로운 함수 파라미터 구문

Python 3.11에서는 타입 어노테이션에서 `|` 연산자를 사용하여 Union 타입을 더 간결하게 표현할 수 있습니다:

```python
# Python 3.10 이전
from typing import Union, Optional

def process_data(data: Union[str, int, float], debug: Optional[bool] = None) -> str:
    if debug:
        print(f"처리 중: {data}")
    return str(data)

# Python 3.11
def process_data_new(data: str | int | float, debug: bool | None = None) -> str:
    if debug:
        print(f"처리 중: {data}")
    return str(data)
```


### 향상된 비동기 프로그래밍

Python 3.11에서는 비동기 프로그래밍 기능이 향상되었습니다:

```python
import asyncio
from typing import List, Dict, Any

async def fetch_data(url: str) -> Dict[str, Any]:
    """URL에서 데이터를 비동기적으로 가져옵니다."""
    print(f"{url} 요청 중...")
    await asyncio.sleep(1)  # 네트워크 요청 시뮬레이션
    return {"url": url, "data": f"Data from {url}"}

async def process_urls(urls: List[str]) -> List[Dict[str, Any]]:
    """여러 URL에서 데이터를 동시에 가져옵니다."""
    # TaskGroup 사용 (Python 3.11의 새로운 기능)
    async with asyncio.TaskGroup() as tg:
        tasks = [tg.create_task(fetch_data(url)) for url in urls]
    
    # 모든 작업이 완료되면 결과 반환
    return [task.result() for task in tasks]

async def main():
    urls = [
        "https://example.com/api/data1",
        "https://example.com/api/data2",
        "https://example.com/api/data3"
    ]
    
    results = await process_urls(urls)
    for result in results:
        print(f"URL: {result['url']}, 데이터: {result['data']}")

# Python 3.11에서 실행
if __name__ == "__main__":
    asyncio.run(main())
```


### 새로운 표준 라이브러리 기능

Python 3.11에서는 표준 라이브러리에 새로운 기능이 추가되었습니다:

```python
# 새로운 math 함수
import math

# 세제곱근 계산
cube_root = math.cbrt(27)  # 3.0

# 2의 거듭제곱 계산
power_of_two = math.exp2(10)  # 1024.0

# 향상된 fractions 모듈
from fractions import Fraction

# 문자열에서 분수 생성
frac1 = Fraction("1/3")  # Fraction(1, 3)
frac2 = Fraction("2.25")  # Fraction(9, 4)

# 향상된 datetime 모듈
from datetime import datetime, timezone

# 시간대 정보가 있는 현재 시간
now = datetime.now(timezone.utc)
print(f"현재 UTC 시간: {now}")

# 문자열 형식 지정
formatted = now.strftime("%Y-%m-%d %H:%M:%S %Z")
print(f"형식화된 시간: {formatted}")
```

