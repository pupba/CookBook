## 2. BeautifulSoup

BeautifulSoup은 HTML 및 XML 문서를 파싱하기 위한 Python 라이브러리로, 웹 페이지에서 데이터를 추출하는 데 매우 유용합니다. Requests로 웹 페이지를 가져온 후, BeautifulSoup을 사용하여 HTML을 파싱하고 원하는 데이터를 추출할 수 있습니다.

### 설치하기

먼저 BeautifulSoup 라이브러리를 설치합니다:

```bash
pip install beautifulsoup4
```

Requests 라이브러리도 함께 설치해야 합니다:

```bash
pip install requests
```


### 기본 사용법

BeautifulSoup의 기본적인 사용 방법은 다음과 같습니다:

```python
import requests
from bs4 import BeautifulSoup

# 웹 페이지 가져오기
url = 'https://www.example.com'
response = requests.get(url)

# BeautifulSoup 객체 생성
soup = BeautifulSoup(response.content, 'html.parser')

# 페이지 제목 출력
print(soup.title.text)
```


### HTML 요소 찾기

BeautifulSoup에서는 다양한 방법으로 HTML 요소를 찾을 수 있습니다:

#### 태그로 찾기

```python
# 첫 번째 p 태그 찾기
first_paragraph = soup.find('p')
print(first_paragraph.text)

# 모든 p 태그 찾기
all_paragraphs = soup.find_all('p')
for paragraph in all_paragraphs:
    print(paragraph.text)
```


#### 클래스나 ID로 찾기

```python
# 특정 클래스를 가진 요소 찾기
element = soup.find(class_='container')
print(element.text)

# 특정 ID를 가진 요소 찾기
element = soup.find(id='header')
print(element.text)
```


#### CSS 선택자 사용하기

```python
# CSS 선택자로 요소 찾기
elements = soup.select('div.article h2')
for element in elements:
    print(element.text)

# 첫 번째 일치하는 요소만 찾기
element = soup.select_one('div.article h2')
print(element.text)
```


### 요소 속성 접근하기

HTML 요소의 속성에 접근하는 방법:

```python
# 모든 링크 찾기
links = soup.find_all('a')
for link in links:
    # href 속성 가져오기
    href = link.get('href')
    # 링크 텍스트 가져오기
    text = link.text
    print(f"{text}: {href}")
```


### 요소 탐색하기

BeautifulSoup은 HTML 트리를 탐색하는 다양한 방법을 제공합니다:

```python
# 부모 요소 접근
element = soup.find('li')
parent = element.parent
print(parent.name)

# 자식 요소 접근
children = element.contents
for child in children:
    print(child)

# 형제 요소 접근
next_sibling = element.next_sibling
previous_sibling = element.previous_sibling
```


### 텍스트 추출하기

요소에서 텍스트를 추출하는 방법:

```python
# 텍스트만 추출
text = element.text
print(text)

# 텍스트 및 공백 처리
text = element.get_text(strip=True)
print(text)
```


### 실전 예제: 뉴스 기사 스크래핑

다음은 Hacker News의 기사를 스크래핑하는 예제입니다:

```python
import requests
from bs4 import BeautifulSoup

# 웹 페이지 가져오기
url = 'https://news.ycombinator.com/'
response = requests.get(url)
soup = BeautifulSoup(response.content, 'html.parser')

# 모든 기사 찾기
articles = soup.find_all(class_='athing')

# 각 기사의 정보 추출
for article in articles:
    # 제목과 링크 추출
    title_element = article.find(class_='titleline')
    title = title_element.text
    link = title_element.find('a')['href']
    
    # 순위 추출
    rank = article.find(class_='rank').text.replace('.', '')
    
    print(f"순위: {rank}")
    print(f"제목: {title}")
    print(f"링크: {link}")
    print('-' * 50)
```


### 페이지네이션 처리하기

여러 페이지에 걸쳐 데이터를 스크래핑하는 방법:

```python
import requests
from bs4 import BeautifulSoup

is_scraping = True
current_page = 1
scraped_data = []

while is_scraping:
    # 현재 페이지 가져오기
    url = f"https://news.ycombinator.com/?p={current_page}"
    response = requests.get(url)
    soup = BeautifulSoup(response.content, 'html.parser')
    
    print(f"페이지 {current_page} 스크래핑 중: {url}")
    
    # 기사 찾기
    articles = soup.find_all(class_='athing')
    
    # 기사가 있으면 데이터 추출
    if articles:
        for article in articles:
            data = {
                "URL": article.find(class_='titleline').find('a')['href'],
                "title": article.find(class_='titleline').text,
                "rank": article.find(class_='rank').text.replace('.', '')
            }
            scraped_data.append(data)
    
    # 다음 페이지 링크 확인
    next_page_link = soup.find(class_='morelink')
    if next_page_link:
        current_page += 1
    else:
        is_scraping = False

print(f"스크래핑 완료! 총 {current_page}페이지 스크래핑")
print(f"총 {len(scraped_data)}개 기사 스크래핑")
```

BeautifulSoup은 웹 스크래핑에 필수적인 도구로, HTML 문서를 쉽게 파싱하고 필요한 데이터를 추출할 수 있게 해줍니다.


