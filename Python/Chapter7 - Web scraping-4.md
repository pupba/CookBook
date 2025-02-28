## 4. Scrapy

Scrapy는 강력하고 확장 가능한 웹 스크래핑 및 크롤링 프레임워크로, 대규모 웹 스크래핑 프로젝트에 특히 적합합니다. 단순한 라이브러리가 아닌 완전한 프레임워크로서 HTTP 요청, 데이터 파싱, 내보내기 등 웹 스크래핑에 필요한 모든 기능을 제공합니다.

### 설치하기

먼저 Scrapy를 설치합니다:

```bash
pip install scrapy
```


### 프로젝트 생성하기

Scrapy 프로젝트를 생성하는 방법:

```bash
# 프로젝트 생성
scrapy startproject myproject

# 프로젝트 디렉토리로 이동
cd myproject

# 스파이더 생성
scrapy genspider example example.com
```

이 명령은 다음과 같은 프로젝트 구조를 생성합니다:

```
myproject/
├── scrapy.cfg            # 프로젝트 설정 파일
└── myproject/
    ├── __init__.py
    ├── items.py          # 아이템 정의
    ├── middlewares.py    # 미들웨어 컴포넌트
    ├── pipelines.py      # 아이템 파이프라인
    ├── settings.py       # 프로젝트 설정
    └── spiders/          # 스파이더 디렉토리
        └── __init__.py
```


### 첫 번째 스파이더 만들기

스파이더는 Scrapy에서 웹사이트를 크롤링하고 데이터를 추출하는 클래스입니다:

```python
import scrapy

class MySpider(scrapy.Spider):
    name = "example"  # 스파이더 이름
    allowed_domains = ["example.com"]  # 허용된 도메인
    start_urls = ["https://www.example.com"]  # 시작 URL

    def parse(self, response):
        # 페이지에서 데이터 추출
        title = response.css('title::text').get()
        yield {
            'title': title
        }
```


### 스파이더 실행하기

스파이더를 실행하는 방법:

```bash
scrapy crawl example
```


### 데이터 추출하기

Scrapy는 CSS 선택자와 XPath 표현식을 사용하여 데이터를 추출할 수 있습니다:

```python
def parse(self, response):
    # CSS 선택자 사용
    products = response.css('li.product')
    
    for product in products:
        yield {
            'name': product.css('h2::text').get(),
            'price': product.css('.price *::text').getall(),
            'url': product.css('a').attrib['href'],
            'image': product.css('img').attrib['src']
        }
```


### 페이지네이션 처리하기

여러 페이지에 걸쳐 데이터를 스크래핑하는 방법:

```python
def parse(self, response):
    # 현재 페이지의 데이터 추출
    for product in response.css('li.product'):
        yield {
            'name': product.css('h2::text').get(),
            'price': product.css('.price::text').get()
        }
    
    # 다음 페이지 링크 찾기
    next_page = response.css('a.next::attr(href)').get()
    if next_page:
        yield response.follow(next_page, self.parse)
```


### 크롤 스파이더 사용하기

복잡한 크롤링 규칙을 정의하려면 CrawlSpider 클래스를 사용할 수 있습니다:

```python
from scrapy.spiders import CrawlSpider, Rule
from scrapy.linkextractors import LinkExtractor

class MySpider(CrawlSpider):
    name = "crawler"
    allowed_domains = ["example.com"]
    start_urls = ["https://www.example.com"]
    
    rules = (
        # 제품 페이지 링크를 추출하고 parse_item 콜백 함수 호출
        Rule(LinkExtractor(allow=r'items/\d+'), callback='parse_item'),
        # 카테고리 페이지 링크를 추출하고 계속 크롤링
        Rule(LinkExtractor(allow=r'category/'), follow=True),
    )
    
    def parse_item(self, response):
        yield {
            'title': response.css('h1::text').get(),
            'price': response.css('.price::text').get()
        }
```


### 아이템 파이프라인 사용하기

추출한 데이터를 처리하고 저장하기 위해 아이템 파이프라인을 사용할 수 있습니다:

```python
# items.py
import scrapy

class Product(scrapy.Item):
    name = scrapy.Field()
    price = scrapy.Field()
    url = scrapy.Field()

# pipelines.py
class PricePipeline:
    def process_item(self, item, spider):
        # 가격 문자열에서 통화 기호 제거
        if 'price' in item:
            item['price'] = item['price'].replace('$', '')
        return item
```

파이프라인을 활성화하려면 settings.py 파일에 추가해야 합니다:

```python
# settings.py
ITEM_PIPELINES = {
    'myproject.pipelines.PricePipeline': 300,
}
```


### 데이터 저장하기

Scrapy에서는 다양한 형식으로 데이터를 저장할 수 있습니다:

```bash
# JSON 형식으로 저장
scrapy crawl example -o data.json

# CSV 형식으로 저장
scrapy crawl example -o data.csv

# XML 형식으로 저장
scrapy crawl example -o data.xml
```


### Python 스크립트에서 Scrapy 실행하기

Scrapy를 Python 스크립트에서 직접 실행할 수도 있습니다:

```python
from scrapy.crawler import CrawlerProcess
from scrapy.utils.project import get_project_settings
from myproject.spiders.example import MySpider

# 프로젝트 설정 가져오기
settings = get_project_settings()

# 크롤러 프로세스 생성
process = CrawlerProcess(settings)

# 스파이더 실행
process.crawl(MySpider)
process.start()
```

Scrapy는 대규모 웹 스크래핑 프로젝트에 적합한 강력한 프레임워크로, 다양한 기능과 확장성을 제공합니다. 이 튜토리얼을 통해 Scrapy의 기본 사용법을 익히고, 더 복잡한 웹 스크래핑 작업을 수행할 수 있는 기반을 마련하세요.
