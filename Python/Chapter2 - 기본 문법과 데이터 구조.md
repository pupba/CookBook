## Chapter2 - Python 기본 문법과 데이터 구조

### 변수와 기본 데이터 타입

Python에서는 변수를 선언할 때 타입을 명시적으로 지정할 필요가 없습니다. 변수는 값을 할당하는 순간 해당 값의 타입을 가지게 됩니다.

```python
# 정수형(int)
age = 25

# 실수형(float)
height = 175.5

# 문자열(str)
name = "홍길동"

# 불리언(bool)
is_student = True
```

타입 어노테이션을 사용하면 다음과 같이 작성할 수 있습니다:

```python
age: int = 25
height: float = 175.5
name: str = "홍길동"
is_student: bool = True
```


### 컬렉션 데이터 타입

Python은 다양한 컬렉션 데이터 타입을 제공합니다:

#### 리스트(List)

순서가 있고 변경 가능한 컬렉션입니다:

```python
# 리스트 생성
fruits: list[str] = ["사과", "바나나", "딸기"]

# 요소 접근
print(fruits[0])  # 사과

# 요소 추가
fruits.append("오렌지")

# 요소 삭제
fruits.remove("바나나")

# 리스트 슬라이싱
print(fruits[1:3])  # ['딸기', '오렌지']
```


#### 튜플(Tuple)

순서가 있고 변경 불가능한 컬렉션입니다:

```python
# 튜플 생성
coordinates: tuple[float, float] = (37.5665, 126.9780)

# 요소 접근
print(coordinates[0])  # 37.5665

# 튜플 언패킹
latitude, longitude = coordinates
```


#### 딕셔너리(Dictionary)

키-값 쌍을 저장하는 컬렉션입니다:

```python
# 딕셔너리 생성
person: dict[str, str | int] = {
    "name": "홍길동",
    "age": 25,
    "city": "서울"
}

# 값 접근
print(person["name"])  # 홍길동

# 키-값 쌍 추가
person["job"] = "개발자"

# 키-값 쌍 삭제
del person["city"]
```

타입 어노테이션을 더 정확하게 표현하려면 `TypedDict`를 사용할 수 있습니다:

```python
from typing import TypedDict

class Person(TypedDict):
    name: str
    age: int
    city: str
    job: str | None

person: Person = {
    "name": "홍길동",
    "age": 25,
    "city": "서울",
    "job": None
}
```


#### 집합(Set)

중복이 없는 요소들의 컬렉션입니다:

```python
# 집합 생성
unique_numbers: set[int] = {1, 2, 3, 4, 5, 3, 2}  # {1, 2, 3, 4, 5}

# 요소 추가
unique_numbers.add(6)

# 요소 삭제
unique_numbers.remove(1)

# 집합 연산
set_a: set[int] = {1, 2, 3, 4}
set_b: set[int] = {3, 4, 5, 6}
print(set_a.union(set_b))  # {1, 2, 3, 4, 5, 6}
print(set_a.intersection(set_b))  # {3, 4}
```


### 제어 흐름

#### 조건문

```python
age: int = 18

if age >= 18:
    print("성인입니다.")
elif age >= 13:
    print("청소년입니다.")
else:
    print("어린이입니다.")
```


#### 반복문

```python
# for 반복문
fruits: list[str] = ["사과", "바나나", "딸기"]
for fruit in fruits:
    print(fruit)

# range를 사용한 반복
for i in range(5):  # 0부터 4까지
    print(i)

# while 반복문
count: int = 0
while count < 5:
    print(count)
    count += 1
```


### 함수 정의와 사용

Python에서 함수는 `def` 키워드를 사용하여 정의합니다:

```python
def greet(name: str) -> str:
    """
    인사말을 반환하는 함수
    
    Args:
        name: 인사할 사람의 이름
        
    Returns:
        인사말 문자열
    """
    return f"안녕하세요, {name}님!"

# 함수 호출
message: str = greet("홍길동")
print(message)  # 안녕하세요, 홍길동님!
```


#### 기본 매개변수

```python
def power(base: int, exponent: int = 2) -> int:
    """
    거듭제곱을 계산하는 함수
    
    Args:
        base: 밑수
        exponent: 지수 (기본값: 2)
        
    Returns:
        거듭제곱 결과
    """
    return base ** exponent

# 기본 매개변수 사용
print(power(3))  # 9 (3^2)
print(power(3, 3))  # 27 (3^3)
```


#### 가변 인자

```python
def sum_all(*args: int) -> int:
    """
    모든 인자의 합을 반환하는 함수
    
    Args:
        *args: 합산할 정수들
        
    Returns:
        모든 인자의 합
    """
    return sum(args)

# 가변 인자 사용
print(sum_all(1, 2, 3, 4, 5))  # 15
```


#### 키워드 가변 인자

```python
def create_profile(**kwargs: str | int) -> dict[str, str | int]:
    """
    프로필 정보를 생성하는 함수
    
    Args:
        **kwargs: 프로필 정보 (키-값 쌍)
        
    Returns:
        프로필 정보 딕셔너리
    """
    return kwargs

# 키워드 가변 인자 사용
profile = create_profile(name="홍길동", age=25, city="서울")
print(profile)  # {'name': '홍길동', 'age': 25, 'city': '서울'}
```


### 클래스와 객체 지향 프로그래밍

Python은 객체 지향 프로그래밍을 지원합니다. 클래스는 `class` 키워드를 사용하여 정의합니다:

```python
class Person:
    def __init__(self, name: str, age: int):
        """
        Person 클래스 생성자
        
        Args:
            name: 이름
            age: 나이
        """
        self.name = name
        self.age = age
        
    def greet(self) -> str:
        """
        인사말을 반환하는 메서드
        
        Returns:
            인사말 문자열
        """
        return f"안녕하세요, 저는 {self.name}이고 {self.age}살입니다."
    
    def have_birthday(self) -> None:
        """
        생일을 맞이하여 나이를 1 증가시키는 메서드
        """
        self.age += 1

# 객체 생성
person: Person = Person("홍길동", 25)
print(person.greet())  # 안녕하세요, 저는 홍길동이고 25살입니다.

# 메서드 호출
person.have_birthday()
print(person.age)  # 26
```


#### 상속

```python
class Student(Person):
    def __init__(self, name: str, age: int, student_id: str):
        """
        Student 클래스 생성자
        
        Args:
            name: 이름
            age: 나이
            student_id: 학번
        """
        super().__init__(name, age)
        self.student_id = student_id
        
    def study(self, subject: str) -> str:
        """
        공부하는 메서드
        
        Args:
            subject: 과목
            
        Returns:
            공부 상태 메시지
        """
        return f"{self.name}이(가) {subject}을(를) 공부하고 있습니다."

# 상속 클래스 객체 생성
student: Student = Student("김철수", 20, "2023001")
print(student.greet())  # 안녕하세요, 저는 김철수이고 20살입니다.
print(student.study("Python"))  # 김철수이(가) Python을(를) 공부하고 있습니다.
```


### 예외 처리

Python에서는 `try`, `except`, `finally` 블록을 사용하여 예외를 처리합니다:

```python
def divide(a: int, b: int) -> float:
    """
    두 수를 나누는 함수
    
    Args:
        a: 분자
        b: 분모
        
    Returns:
        나눗셈 결과
        
    Raises:
        ZeroDivisionError: 분모가 0인 경우
    """
    try:
        return a / b
    except ZeroDivisionError:
        print("0으로 나눌 수 없습니다.")
        return float('inf')  # 무한대 반환
    finally:
        print("나눗셈 연산 완료")

# 예외 처리 테스트
print(divide(10, 2))  # 5.0
print(divide(10, 0))  # 0으로 나눌 수 없습니다. inf
```


### 모듈과 패키지

Python에서는 코드를 모듈과 패키지로 구성할 수 있습니다:

#### 모듈 생성 및 사용

```python
# math_utils.py 파일
def add(a: int, b: int) -> int:
    return a + b

def subtract(a: int, b: int) -> int:
    return a - b

# 모듈 사용
import math_utils

result: int = math_utils.add(5, 3)
print(result)  # 8

# 특정 함수만 가져오기
from math_utils import subtract

result: int = subtract(10, 4)
print(result)  # 6
```


#### 패키지 구조

```
my_package/
├── __init__.py
├── module1.py
└── subpackage/
    ├── __init__.py
    └── module2.py
```

```python
# 패키지 사용
import my_package.module1
from my_package.subpackage import module2
```


### 파일 입출력

Python에서는 파일을 쉽게 읽고 쓸 수 있습니다:

```python
# 파일 쓰기
def write_to_file(filename: str, content: str) -> None:
    """
    파일에 내용을 쓰는 함수
    
    Args:
        filename: 파일 이름
        content: 파일에 쓸 내용
    """
    with open(filename, 'w', encoding='utf-8') as file:
        file.write(content)

# 파일 읽기
def read_from_file(filename: str) -> str:
    """
    파일의 내용을 읽는 함수
    
    Args:
        filename: 파일 이름
        
    Returns:
        파일 내용
    """
    with open(filename, 'r', encoding='utf-8') as file:
        return file.read()

# 파일 입출력 테스트
write_to_file('test.txt', '안녕하세요, Python!')
content: str = read_from_file('test.txt')
print(content)  # 안녕하세요, Python!
```


### 컨텍스트 매니저

`with` 문을 사용하여 리소스를 자동으로 관리할 수 있습니다:

```python
class FileManager:
    def __init__(self, filename: str, mode: str):
        self.filename = filename
        self.mode = mode
        self.file = None
        
    def __enter__(self):
        self.file = open(self.filename, self.mode, encoding='utf-8')
        return self.file
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        if self.file:
            self.file.close()

# 컨텍스트 매니저 사용
with FileManager('test.txt', 'w') as file:
    file.write('컨텍스트 매니저 테스트')
```


### 함수형 프로그래밍

Python은 함수형 프로그래밍 패러다임도 지원합니다:

```python
from typing import Callable, List

# map 함수
numbers: list[int] = [1, 2, 3, 4, 5]
squared: list[int] = list(map(lambda x: x**2, numbers))
print(squared)  # [1, 4, 9, 16, 25]

# filter 함수
even_numbers: list[int] = list(filter(lambda x: x % 2 == 0, numbers))
print(even_numbers)  # [2, 4]

# reduce 함수
from functools import reduce
sum_all: int = reduce(lambda x, y: x + y, numbers)
print(sum_all)  # 15

# 고차 함수
def create_multiplier(factor: int) -> Callable[[int], int]:
    """
    곱셈 함수를 반환하는 고차 함수
    
    Args:
        factor: 곱할 인자
        
    Returns:
        인자를 factor와 곱하는 함수
    """
    def multiply(x: int) -> int:
        return x * factor
    return multiply

double: Callable[[int], int] = create_multiplier(2)
print(double(5))  # 10
```

