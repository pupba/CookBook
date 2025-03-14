# OJT 교육 문서: 데이터베이스 개념 및 유형

## 개요

이 문서는 신입 MLOps 엔지니어를 대상으로 데이터베이스(DB)의 기본 개념과 주요 유형을 이해시키기 위한 교육 자료입니다. 관계형 데이터베이스(RDB)부터 NoSQL, 그리고 벡터 데이터베이스까지 다룹니다.

---

## **1. 관계형 데이터베이스 (RDB)**

### 정의

- 관계형 데이터베이스는 데이터를 **테이블** 형식으로 저장하며, 각 테이블은 행(Row)과 열(Column)로 구성됩니다[^1][^2].
- 데이터를 효율적으로 관리하고 검색하기 위해 **키(Key)** 와 **값(Value)** 의 관계를 기반으로 작동합니다.



### 주요 특징

- **고정된 스키마**: 테이블 구조가 사전에 정의되어야 합니다.
- **ACID 속성**: 트랜잭션의 안정성과 일관성을 보장합니다.[^3]
    - **Atomicity(원자성)** : 모두 실행하거나, 모두 실행 안하거나, 즉! **일부만 성공하는 상태는 없다!**
    - **Consistency(일관성)** : 트랜잭션을 실행하고 이후에는 데이터베이스의 상태는 항상 **일관적** 인 상태여야 한다. (DB가 가지고 있는 규칙을 위배할 수 없음, e.g. SQL Query문의 실패)
    - **Isolation(격리성)** : 동시에 트랜잭션이 실행될 수 없음, 각 트랜잭션은 독립적으로 동작해야함.
    - **Durability(지속성)** : 커밋이된 트랜잭션의 결과는 데이터베이스에 영구적으로 저장(HDD, SSD)되어야 한다. 
- **SQL 사용**: 데이터를 삽입, 수정, 삭제, 조회하기 위해 구조화된 질의 언어(SQL)를 사용합니다. (DDL, DCL, DML)


### 활용 사례

- 금융 시스템, ERP(Enterprise Resource Planning), 고객 관리 시스템(CRM) 등 정형화된 데이터와 복잡한 관계를 처리하는 데 적합합니다.

---

## **2. NoSQL 데이터베이스**

### 정의

- NoSQL은 **스키마가 고정되지 않은** 데이터베이스로, 다양한 데이터 모델(Key-Value, Document, Graph 등)을 지원합니다[^3].

**비정형 데이터** 들을 저장하기 좋습니다.

### 주요 유형 및 설명

#### **2.1 Key-Value 데이터베이스**

- 데이터를 키(Key)와 값(Value) 쌍으로 저장하는 가장 단순한 형태의 DB입니다[^4][^5].
- **특징**:
    - 빠른 읽기/쓰기 성능.
    - 고성능 캐싱 및 세션 관리에 적합.
- **예시**: Redis, DynamoDB.


#### **2.2 Document 데이터베이스**

- 데이터를 JSON 또는 BSON 형식의 문서(Document)로 저장합니다[^7][^15].
- **특징**:
    - 스키마리스 구조로 유연한 데이터 관리 가능.
    - 각 문서는 독립적이며 고유한 구조를 가질 수 있음.
- **예시**: MongoDB, CouchDB.


#### **2.3 Graph 데이터베이스**

- 데이터를 노드(Node), 엣지(Edge), 프로퍼티(Property)를 이용해 그래프 형태로 저장합니다[^10][^11].
- **특징**:
    - 복잡한 관계를 자연스럽게 표현 가능.
    - 네트워크 분석 및 연결 관계 탐색에 적합.
- **예시**: Neo4j.

---

## **3. 객체 지향형 데이터베이스 (OODB)**

### 정의

- 객체 지향 프로그래밍(OOP)의 개념을 도입하여 데이터를 객체 형태로 저장하는 DB입니다[^9].
- 관계형 DB보다 더 직관적이고 유연한 계층 구조를 제공합니다.


### 특징

- 객체 간의 연결과 계층 구조를 통해 복잡한 이벤트를 표현 가능.
- 주로 SmallTalk, C++ 등 객체 지향 언어와 함께 사용됨.

---

## **4. 벡터 데이터베이스**

### 정의

- 벡터 데이터베이스는 데이터를 고차원 벡터 형태로 저장하며, 비정형 데이터를 처리하는 데 특화된 DB입니다[^12][^13][^14].


### 주요 특징

- **임베딩 지원**: 텍스트, 이미지, 오디오 같은 비정형 데이터를 벡터화하여 저장.
- **유사도 검색(Similarity Search)**:
    - 의미 기반 검색을 통해 관련성이 높은 데이터를 빠르게 찾아냄.
    - 예: "비슷한 이미지 찾기" 또는 "유사한 문장 검색".
- **확장성 및 성능**:
    - 대규모 비정형 데이터를 실시간으로 처리 가능.


### 활용 사례

- AI 모델 학습 결과 저장소, 추천 시스템, 자연어 처리(NLP) 작업 등.

---

## **5. RDB vs NoSQL vs 벡터 DB 비교**

| 항목     | RDB      | NoSQL         | 벡터 DB           |
| :----- | :------- | :------------ | :-------------- |
| 스키마    | 고정됨      | 동적            | 없음              |
| 확장성    | Scale-Up | Scale-Out     | Scale-Out       |
| 데이터 유형 | 정형화된 데이터 | 반정형/비정형 데이터   | 비정형 데이터         |
| 주요 활용  | 트랜잭션 처리  | 빅데이터 및 실시간 처리 | AI 임베딩 및 유사도 검색 |

---

## 결론

이 교육 자료는 신입 개발자가 다양한 유형의 데이터베이스를 이해하고 실무에 활용할 수 있도록 구성되었습니다. 각 DB의 특성과 활용 사례를 명확히 이해하면 MLOps 작업에서 필요한 적절한 솔루션을 선택할 수 있습니다.

<div style="text-align: center">⁂</div>

[^1]: https://jwprogramming.tistory.com/52

[^2]: https://ko.wikipedia.org/wiki/관계형_데이터베이스

[^3]: https://mysterlee.tistory.com/106

[^4]: https://ko.wikipedia.org/wiki/키-값_데이터베이스

[^5]: https://wikidocs.net/190385

[^6]: https://1-day-1-coding.tistory.com/2

[^7]: https://javacpro.tistory.com/66

[^8]: https://dev-coco.tistory.com/73

[^9]: https://terms.tta.or.kr/dictionary/dictionaryView.do?subject=객체+지향형+데이터베이스

[^10]: https://www.elastic.co/kr/blog/vector-database-vs-graph-database

[^11]: https://1004jonghee.tistory.com/entry/그래프-데이터베이스란

[^12]: https://ray5273.tistory.com/entry/Vector-DB란-무엇인가

[^13]: https://velog.io/@tura/vector-databases

[^14]: https://codemanager.tistory.com/151

[^15]: https://aws.amazon.com/ko/nosql/document/

[^16]: https://cloud.google.com/learn/what-is-a-relational-database?hl=ko

[^17]: https://dev-records.tistory.com/entry/Database-RDB와-NoSQL-비교특징-장단점-등

[^18]: https://thefif19wlsvy.tistory.com/147

[^19]: https://www.oracle.com/kr/database/what-is-a-relational-database/

[^20]: https://velog.io/@maestroks/RDB와-NoSQL은-무엇인가요-차이점-또는-장단점-위주로-설명해주세요

[^21]: https://honey-dev.com/db-rdb-rdbms-3가지-쉽게-이해하기/

[^22]: https://www.ibm.com/kr-ko/topics/relational-databases

[^23]: https://blog.naver.com/fbisk/140031988935?viewType=pc

[^24]: https://velog.io/@garam/DE-RDB-RDBMS

[^25]: https://aws.amazon.com/ko/relational-database/

[^26]: https://dev-sohee.tistory.com/24

[^27]: https://blog.naver.com/jysaa5/221783915289

[^28]: https://www.tcpschool.com/mysql/mysql_intro_relationalDB

[^29]: https://aws.amazon.com/ko/nosql/

[^30]: https://spidyweb.tistory.com/166

[^31]: https://datamoney.tistory.com/298

[^32]: https://www.ibm.com/kr-ko/think/topics/nosql-databases

[^33]: https://it-stargazer.com/nosql-데이터베이스-개념-유형-장단점-및-실제-활용-사례/

[^34]: https://sjh836.tistory.com/97

[^35]: https://www.oracle.com/kr/database/nosql/what-is-nosql/

[^36]: https://f-lab.kr/insight/understanding-and-utilizing-nosql-databases

[^37]: https://www.mongodb.com/ko-kr/resources/basics/databases/nosql-explained

[^38]: https://velog.io/@chosj1526/NoSQL이란

[^39]: http://www.itdaily.kr/news/articleView.html?idxno=227513

[^40]: https://12bme.tistory.com/323

[^41]: https://blog.naver.com/bbbisskk2/222939617745

[^42]: https://velog.io/@park2348190/언제-NoSQL을-사용하는게-좋을까

[^43]: https://ojava.tistory.com/129

[^44]: https://cloud.google.com/discover/what-is-nosql?hl=ko

[^45]: https://kominjae.tistory.com/96

[^46]: https://smoh.tistory.com/372

[^47]: https://azderica.github.io/00-db-nosql/

[^48]: https://hogantechs.com/ko/키-값-저장소-데이터베이스-nosql-sql-hogantech/

[^49]: https://hxnsxxm.tistory.com/20

[^50]: https://velog.io/@gidskql6671/Key-Value-저장소-설계

[^51]: https://hwangwoosam.github.io/posts/데이터베이스-개념-정리/

[^52]: https://blog.voidmainvoid.net/232

[^53]: https://smoh.tistory.com/373

[^54]: https://www.integrate.io/ko/blog/which-database-ko/

[^55]: https://adjh54.tistory.com/257

[^56]: https://inpa.tistory.com/entry/DB-📚-데이터베이스-기초-개념

[^57]: https://yechankk.tistory.com/27

[^58]: https://docs.aws.amazon.com/ko_kr/documentdb/latest/developerguide/document-database-use-cases.html

[^59]: https://ryean.tistory.com/110

[^60]: https://ko.wikipedia.org/wiki/문서_지향_데이터베이스

[^61]: https://velog.io/@ouk/NoSQL-데이터베이스의-유형과-사용-사례를-설명해주세요

[^62]: https://docs.aws.amazon.com/ko_kr/documentdb/latest/developerguide/what-is-document-db.html

[^63]: https://laoching.tistory.com/entry/문서-데이터베이스

[^64]: https://yozm.wishket.com/magazine/detail/2541/

[^65]: https://www.tmaxsoft.com/upload/nas/technet/technet/upload/download/online/proobject/pver-20200130-000001/developer-guide/chapter_data_object.html

[^66]: https://sesoc.tistory.com/327

[^67]: https://www.comworld.co.kr/news/articleView.html?idxno=48790

[^68]: https://velog.io/@been/DB-데이터베이스-객체

[^69]: https://blog.naver.com/mhanaro728/10051917706

[^70]: https://25gstory.tistory.com/62

[^71]: https://www.ibm.com/docs/ko/db2/11.5?topic=administration-database-objects

[^72]: https://f-lab.kr/insight/understanding-orm

[^73]: https://whdgus928.tistory.com/99

[^74]: https://velog.io/@donghoim/SQL-첫걸음-25강.-데이터베이스-객체

[^75]: https://balang.tistory.com/54

[^76]: https://aws.amazon.com/ko/compare/the-difference-between-graph-and-relational-database/

[^77]: https://continuous-development.tistory.com/entry/Graph-DB그래프-데이터-베이스Graph-Database란-정의-장점-사례

[^78]: https://bitnine.tistory.com/390

[^79]: https://bitnine.tistory.com/389

[^80]: https://upload.wikimedia.org/wikipedia/commons/thumb/3/3a/GraphDatabase_PropertyGraph.png/308px-GraphDatabase_PropertyGraph.png?sa=X\&ved=2ahUKEwjWu6yy04iMAxVec_UHHQPdCecQ_B16BAgBEAI

[^81]: https://www.cio.com/article/3533278/그래프-데이터베이스란-무엇인가-어떻게-활용하나.html

[^82]: https://bitnine.tistory.com/323

[^83]: https://wikidocs.net/50747

[^84]: https://datainsider.tistory.com/entry/Graph-DB-Neo4j

[^85]: https://www.oracle.com/a/ocom/img/rc24-what-is-graph-database-4.png?sa=X\&ved=2ahUKEwjhgKiy04iMAxXWzTgGHZfIIMQQ_B16BAgFEAI

[^86]: https://www.comworld.co.kr/news/articleView.html?idxno=51034

[^87]: https://familia-89.tistory.com/90

[^88]: https://aws.amazon.com/ko/what-is/vector-databases/

[^89]: https://www.gnict.org/게시판/ai연구회/벡터-데이터베이스란-무엇-입니까/

[^90]: https://hotorch.tistory.com/407

[^91]: https://www.mongodb.com/ko-kr/resources/basics/databases/vector-databases

[^92]: https://wikidocs.net/265572

[^93]: https://blog.kbanknow.com/66

[^94]: https://meetcody.ai/ko/blog/2024년에-시도해-볼-만한-상위-5가지-벡터-데이터베이스/

[^95]: https://digitalbourgeois.tistory.com/159

[^96]: https://images.contentstack.io/v3/assets/bltefdd0b53724fa2ce/bltb88231adca6b0a23/66cf8011e49f550754eebadc/vector-database-architecture-infographic-3-vector-embeddings.jpg?sa=X\&ved=2ahUKEwiuvre004iMAxXwsVYBHYcfJxgQ_B16BAgBEAI

[^97]: https://familia-89.tistory.com/89

[^98]: https://dirtycoders.net/what-about-vector-database/

[^99]: https://www.devkobe24.com/DB/2024-10-13-what-is-the-rdb.html

[^100]: https://www.devkobe24.com/CS/2024/2024-09-30-RDB.html

[^101]: https://azure.microsoft.com/ko-kr/resources/cloud-computing-dictionary/what-is-a-relational-database

[^102]: https://f-lab.kr/insight/redis-vs-rdb

[^103]: https://danhandev.tistory.com/entry/DB-RDB-관계형-데이터베이스란-무엇일까

[^104]: https://shuu.tistory.com/135

[^105]: https://wikidocs.net/190254

[^106]: https://wikidocs.net/book/7975

[^107]: https://newtoner.tistory.com/23

[^108]: https://www.couchbase.com/ko/resources/concepts/key-value-database/

[^109]: https://aws.amazon.com/ko/nosql/key-value/

[^110]: https://bbaktaeho-95.tistory.com/108

[^111]: https://mysterlee.tistory.com/107

[^112]: https://wikidocs.net/190277

[^113]: https://ryu-e.tistory.com/2

[^114]: https://blog.voidmainvoid.net/238

[^115]: https://eun-jeong.tistory.com/31

[^116]: https://dana-study-log.tistory.com/entry/DB-데이터베이스-기초

[^117]: https://ko.wikipedia.org/wiki/객체_관계_데이터베이스

[^118]: http://blog.naver.com/imdkkang/120090738638

[^119]: https://huisam.tistory.com/entry/DataBase1

[^120]: https://incodom.kr/데이터베이스_객체

[^121]: https://www.oss.kr/news/show/8d6fe1b5-d998-4f62-8b77-b724b9680f9b

[^122]: https://bitnine.net/introduction-to-graph-database-kor/

[^123]: https://www.oracle.com/kr/autonomous-database/what-is-graph-database/

[^124]: https://hyowong.tistory.com/entry/그래프-DB로-구축한-시스템-사례

[^125]: https://aws.amazon.com/ko/nosql/graph/

[^126]: https://bitnine.tistory.com/517

[^127]: https://gruuuuu.github.io/ai/vector-store/

[^128]: https://www.cloudflare.com/ko-kr/learning/ai/what-is-vector-database/

[^129]: https://www.elastic.co/kr/what-is/vector-database

[^130]: https://www.ibm.com/kr-ko/think/topics/vector-database

