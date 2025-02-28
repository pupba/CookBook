## 3. Selenium

Selenium은 웹 브라우저 자동화 도구로, 동적 콘텐츠가 포함된 웹사이트를 스크래핑하는 데 매우 유용합니다. JavaScript로 렌더링되는 콘텐츠를 처리할 수 있어 Requests와 BeautifulSoup만으로는 접근하기 어려운 데이터를 추출할 수 있습니다.

### 설치하기

먼저 Selenium과 웹 드라이버를 설치해야 합니다:

```bash
pip install selenium webdriver-manager
```


### 기본 설정

Selenium을 사용하기 위한 기본 설정은 다음과 같습니다:

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.common.by import By

# 크롬 드라이버 설정
chrome_options = webdriver.ChromeOptions()
driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()), options=chrome_options)

# 웹사이트 방문
driver.get("https://www.example.com")

# 작업 완료 후 브라우저 종료
driver.quit()
```


### 헤드리스 모드 사용하기

브라우저 UI를 표시하지 않는 헤드리스 모드를 사용하면 리소스를 절약하고 스크래핑 속도를 높일 수 있습니다:

```python
chrome_options = webdriver.ChromeOptions()
chrome_options.add_argument("--headless")
driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()), options=chrome_options)
```


### 웹 요소 찾기

Selenium에서는 다양한 방법으로 웹 요소를 찾을 수 있습니다:

```python
# ID로 요소 찾기
element = driver.find_element(By.ID, "element_id")

# 클래스 이름으로 요소 찾기
elements = driver.find_elements(By.CLASS_NAME, "class_name")

# CSS 선택자로 요소 찾기
element = driver.find_element(By.CSS_SELECTOR, "div.product-name")

# XPath로 요소 찾기
element = driver.find_element(By.XPATH, "//div[@class='product-name']")
```


### 요소와 상호작용하기

웹 요소를 찾은 후 다양한 방법으로 상호작용할 수 있습니다:

```python
# 요소 클릭하기
button = driver.find_element(By.ID, "submit_button")
button.click()

# 텍스트 입력하기
input_field = driver.find_element(By.NAME, "search")
input_field.send_keys("검색어")

# 텍스트 가져오기
text = element.text

# 속성 가져오기
href = element.get_attribute("href")
```


### 동적 콘텐츠 처리하기

JavaScript로 렌더링되는 동적 콘텐츠를 처리하기 위해 명시적 대기를 사용할 수 있습니다:

```python
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# 최대 10초 동안 요소가 나타날 때까지 대기
element = WebDriverWait(driver, 10).until(
    EC.presence_of_element_located((By.ID, "dynamic_element"))
)
```


### 스크롤 처리하기

무한 스크롤이나 지연 로딩을 처리하는 방법:

```python
import time

# 페이지 끝까지 스크롤
last_height = driver.execute_script("return document.body.scrollHeight")
while True:
    # 페이지 끝으로 스크롤
    driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
    
    # 새 콘텐츠가 로드될 때까지 대기
    time.sleep(2)
    
    # 새 스크롤 높이 계산
    new_height = driver.execute_script("return document.body.scrollHeight")
    
    # 더 이상 스크롤할 수 없으면 종료
    if new_height == last_height:
        break
    last_height = new_height
```


### 스크린샷 찍기

현재 페이지의 스크린샷을 저장할 수 있습니다:

```python
driver.save_screenshot('screenshot.png')
```


### JavaScript 실행하기

페이지에서 JavaScript 코드를 직접 실행할 수 있습니다:

```python
# JavaScript 실행
driver.execute_script("alert('Hello, World!');")

# 요소까지 스크롤
element = driver.find_element(By.ID, "my_element")
driver.execute_script("arguments[^0].scrollIntoView();", element)
```


### BeautifulSoup과 함께 사용하기

Selenium으로 동적 콘텐츠를 로드한 후 BeautifulSoup으로 파싱하면 더 효율적으로 데이터를 추출할 수 있습니다:

```python
from bs4 import BeautifulSoup

# 페이지 소스 가져오기
html = driver.page_source

# BeautifulSoup으로 파싱
soup = BeautifulSoup(html, 'html.parser')

# BeautifulSoup을 사용하여 데이터 추출
products = soup.find_all('div', class_='product')
for product in products:
    name = product.find('h2', class_='product-name').text
    price = product.find('span', class_='price').text
    print(f"상품명: {name}, 가격: {price}")
```


### 프록시 사용하기

IP 차단을 방지하기 위해 프록시를 사용할 수 있습니다:

```python
chrome_options = webdriver.ChromeOptions()
chrome_options.add_argument('--proxy-server=http://proxy_host:proxy_port')
driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()), options=chrome_options)
```

Selenium은 동적 웹사이트 스크래핑에 강력한 도구이지만, 리소스를 많이 사용하고 속도가 느릴 수 있다는 단점이 있습니다. 그러나 JavaScript 렌더링이 필요한 현대 웹사이트에서는 필수적인 도구입니다.

