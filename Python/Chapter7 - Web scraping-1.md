## A basic web scraping tutorials.

- 1. Requests
- 2. BeautifulSoup
- 3. Selenium
- 4. Scrapy

## 1. Requests

Requests는 Python에서 HTTP 요청을 보내기 위한 간단하고 우아한 라이브러리입니다. 웹 스크래핑의 첫 단계는 웹 페이지의 내용을 가져오는 것이며, Requests는 이 작업을 매우 쉽게 수행할 수 있게 해줍니다.

### 설치하기

먼저 Requests 라이브러리를 설치합니다:

```bash
pip install requests
```


### 기본 사용법

가장 기본적인 HTTP GET 요청을 보내는 방법:

```python
import requests

# 웹 페이지 가져오기
response = requests.get('https://www.example.com')

# 상태 코드 확인
print(f"상태 코드: {response.status_code}")

# 응답 내용 출력
print(response.text)
```


### 다양한 HTTP 메서드 사용하기

Requests는 GET 외에도 다양한 HTTP 메서드를 지원합니다:

```python
# GET 요청
response = requests.get('https://httpbin.org/get')

# POST 요청
response = requests.post('https://httpbin.org/post', data={'key': 'value'})

# PUT 요청
response = requests.put('https://httpbin.org/put', data={'key': 'value'})

# DELETE 요청
response = requests.delete('https://httpbin.org/delete')

# HEAD 요청
response = requests.head('https://httpbin.org/get')
```


### 요청 매개변수 추가하기

URL에 쿼리 매개변수를 추가하는 방법:

```python
# 쿼리 매개변수 추가
params = {
    'q': 'python requests',
    'page': 1
}
response = requests.get('https://www.google.com/search', params=params)

# 최종 URL 확인
print(f"요청 URL: {response.url}")
```


### 헤더 추가하기

요청에 사용자 정의 헤더를 추가하는 방법:

```python
# 사용자 에이전트 설정
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36',
    'Accept-Language': 'ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7'
}
response = requests.get('https://www.example.com', headers=headers)
```


### 쿠키 처리하기

요청과 함께 쿠키를 보내고 응답 쿠키를 확인하는 방법:

```python
# 쿠키 보내기
cookies = {'session_id': '12345'}
response = requests.get('https://www.example.com', cookies=cookies)

# 응답 쿠키 확인
print(response.cookies)
```


### 세션 사용하기

여러 요청에 걸쳐 쿠키와 연결을 유지하려면 세션을 사용합니다:

```python
# 세션 생성
session = requests.Session()

# 로그인
session.post('https://www.example.com/login', data={
    'username': 'user',
    'password': 'pass'
})

# 로그인 후 페이지 접근 (쿠키 자동 유지)
response = session.get('https://www.example.com/profile')
```


### 타임아웃 설정하기

요청 타임아웃을 설정하여 응답이 지연될 경우 대응하는 방법:

```python
try:
    # 5초 타임아웃 설정
    response = requests.get('https://www.example.com', timeout=5)
except requests.Timeout:
    print("요청 시간이 초과되었습니다.")
```


### 오류 처리하기

요청 중 발생할 수 있는 다양한 예외를 처리하는 방법:

```python
try:
    response = requests.get('https://www.example.com')
    # 상태 코드 오류 발생시키기
    response.raise_for_status()
except requests.exceptions.HTTPError as err:
    print(f"HTTP 오류 발생: {err}")
except requests.exceptions.ConnectionError:
    print("서버에 연결할 수 없습니다.")
except requests.exceptions.Timeout:
    print("요청 시간이 초과되었습니다.")
except requests.exceptions.RequestException as err:
    print(f"오류 발생: {err}")
```


### 파일 다운로드하기

웹에서 파일을 다운로드하는 방법:

```python
# 이미지 다운로드
response = requests.get('https://www.example.com/image.jpg', stream=True)
if response.status_code == 200:
    with open('image.jpg', 'wb') as f:
        for chunk in response.iter_content(chunk_size=8192):
            f.write(chunk)
```


### JSON 응답 처리하기

API에서 반환된 JSON 데이터를 처리하는 방법:

```python
# JSON API 호출
response = requests.get('https://api.github.com/user', auth=('username', 'password'))

# JSON 응답 파싱
if response.status_code == 200:
    user_data = response.json()
    print(f"사용자 이름: {user_data['name']}")
    print(f"팔로워 수: {user_data['followers']}")
```

Requests는 웹 스크래핑의 첫 단계인 웹 페이지 내용 가져오기를 쉽게 해주지만, HTML을 파싱하고 필요한 데이터를 추출하기 위해서는 BeautifulSoup과 같은 다른 라이브러리와 함께 사용하는 것이 일반적입니다.

