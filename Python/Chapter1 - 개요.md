## Chapter1 - 개요

### Python 언어 간단 설명

Python은 간결하고 읽기 쉬운 문법을 가진 고수준 프로그래밍 언어입니다.

Python 언어는 다음과 같은 특징을 가지고 있습니다.
- **쉬운 문법** : 영어와 유사한 문법 구조로 배우기 쉽습니다.
- **동적 타이핑** : 변수 선언 시 타입을 명시할 필요가 없습니다.
- **인터프리터 언어** : 컴파일 없이 코드를 바로 실행할 수 있습니다.
- **다양한 라이브러리** : 풍부한 표준 라이브러리와 외부 패키지를 제공합니다.

본 튜토리얼에서는 [Python 3.11.9](https://www.python.org/downloads/release/python-3119/)를 사용합니다.

### Python 인터프리터

Python 인터프리터는 코드를 실행하기 전에 **바이트코드** 로 컴파일 합니다.

바이트코드는 일반 Python 코드보다 더 기본적인 명령어로 구성되어 있으며, 각 Python 코드 라인은 여러 바이트 코드 명령어로 변환됩니다.

Python 코드가 컴파일된 바이트코드를 파이썬 **가상머신(Py VM)** 이 실행하여 코드가 실행된다.
![Python-1-1.png](python/images/Python-1-1.png)

### 3.11.9 에서의 변경 및 개선 사항

#### 성능 향상

Python 3.11은 Python 3.10 보다 평균적으로 **1.22배** 빠르며, 경우에 따라 10-60% 성능이 향상 되었습니다. 이는 [Faster CPython](https://github.com/faster-cpython/) 프로젝트의 결과물로, 다음과 같은 최적화가 적용되었습니다.
- [**인터프리터 시작 속도 향상**](https://docs.python.org/3/whatsnew/3.11.html) : 10-15% 더 빠른 시작 속도를 제공합니다.
- [**지연 Python 프레임**](https://docs.python.org/3/whatsnew/3.11.html) : 프레임 생성 과정을 간소화하고 메모리 할당을 최적화했습니다.
- [**Python 함수 호출 인라이닝**](https://docs.python.org/3/whatsnew/3.11.html) : Python 함수 호출 시 C 스택 공간을 소비하지 않도록 최적화했습니다.
- [**특수화 적응형 인터프리터(PEP 659)**](https://realpython.com/python311-new-features/) : 코드 실행 중 자주 사용되는 연산을 최적화합니다.

#### 향상된 오류 메시지

Python 3.11은 [오류 발생 위치를 더 정확하게](https://flyaps.com/blog/python-3-11-release-features-overview/) 보여줍니다. 
이전 버전에서는 오류가 발생한 줄만 표시했지만, 3.11에서는 정확한 위치를 가리킵니다.

*이는 중첩 딕셔너리 객체나 여러 함수 호출을 다룰 때 유용합니다.*

#### 표준 라이브러리 개선

Python 3.11에는 여러 표준 라이브러리 [개선 사항](https://www.datacamp.com/blog/whats-new-in-python-311-and-should-you-even-bother-with-it)이 포함되어 있습니다.
- **math 모듈 추가 함수**: `math.cbrt()` (세제곱근) 및 `math.exp2()` (2의 거듭제곱) 함수가 추가되었습니다.
- **fractions 모듈 개선**: 문자열에서 분수를 생성하는 기능이 추가되었습니다.
- **tomllib 모듈 추가**: TOML 파일 형식을 파싱하기 위한 새로운 모듈이 추가되었습니다.

