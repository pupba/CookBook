# OJT 교육 문서: 벡터 데이터베이스(Vector Database) 심화 학습

## **개요**

벡터 데이터베이스는 데이터를 **고차원 벡터** 형태로 저장하고 관리하며, 유사도 검색 및 의미 기반 검색에 특화된 데이터베이스입니다. 본 문서는 벡터 데이터베이스의 개념, 구조, 주요 특징, 활용 사례를 다루며 MLOps 엔지니어가 이를 실무에서 효과적으로 활용할 수 있도록 작성되었습니다.

---

## **1. 벡터 데이터베이스란?**

### 정의

- 벡터 데이터베이스는 데이터를 **수치 벡터** 형태로 저장하여, 고차원 공간에서의 유사도 검색을 지원합니다.
- 벡터는 텍스트, 이미지, 오디오 등 비정형 데이터를 **임베딩(Embedding)** 기술을 통해 생성된 수치 표현입니다.


### 주요 구성 요소

1. **벡터(Vector)** :
    - 데이터를 수치화한 배열로, 각 차원이 특정 속성을 나타냅니다.
    - 예: 단어 "apple"은 
$$
[0.12, -0.45, 0.34]
$$ 
형태의 벡터로 표현될 수 있습니다.

2. **임베딩(Embedding)** :
    - 데이터를 벡터로 변환하는 과정으로, 텍스트는 Word2Vec, BERT 같은 모델을 통해 임베딩됩니다.
    - 이미지와 오디오는 CNN과 같은 딥러닝 모델을 사용해 임베딩됩니다.
3. **유사도 측정(Similarity Measure)** :
    - 벡터 간의 거리 또는 각도를 계산하여 유사성을 평가합니다.
    - 대표적인 측정 방법:
        - **코사인 유사도(Cosine Similarity)**: 방향성 비교.
        - **유클리드 거리(Euclidean Distance)**: 벡터 간의 직선 거리.
        - **내적(Dot Product)**: 벡터 간의 상관 관계.

---

## **2. 벡터 데이터베이스의 주요 특징**

### 성능 및 확장성

- **빠른 유사도 검색**:
    - Approximate Nearest Neighbor (ANN) 알고리즘을 사용하여 대규모 데이터에서 빠른 검색 가능.
    - 대표적인 알고리즘: HNSW(Hierarchical Navigable Small World), KD-Tree.
- **수평적 확장**:
    - 분산 시스템으로 확장 가능하여 대규모 데이터를 처리할 수 있습니다.


### 하이브리드 검색 기능

- 전통적인 키워드 기반 검색과 벡터 기반 의미 검색을 결합하여 더 정확한 결과 제공.


### 튜닝 가능성

- 인덱스 설정, 메모리 사용량, 쿼리 타임아웃 등을 조정해 성능 최적화 가능.

---

## **3. 주요 벡터 데이터베이스**

| 이름       | 특징                               |
| :------- | :------------------------------- |
| Pinecone | 완전 관리형 서비스로 실시간 유사도 검색 지원.       |
| Milvus   | 오픈소스 솔루션으로 대규모 데이터 처리에 최적화됨.     |
| Weaviate | GraphQL 기반으로 다양한 ML 모델과 통합 가능.   |
| Qdrant   | 고성능 벡터 검색 엔진으로 실시간 업데이트 지원.      |
| Chroma   | AI 애플리케이션 개발에 최적화된 임베딩 관리 기능 제공. |
| Faiss    | Meta에서 만든 속도 최적화 VectorDB        |

---

## **4. 벡터 데이터베이스의 활용 사례**

### 사례 설명

1. **자연어 처리(NLP)**:
    - 텍스트 임베딩을 저장하고 문서 간의 의미적 유사성을 비교하여 검색 수행.

```plaintext
예: 고객 질문에 대한 가장 적절한 답변 찾기 (챗봇 응용).
```

2. **추천 시스템**:
    - 사용자 선호도를 벡터로 저장하고 제품/서비스와 비교하여 맞춤형 추천 제공.

```plaintext
예: Netflix에서 사용자와 영화 간의 유사도를 계산해 추천.
```

3. **이미지 및 비디오 분석**:
    - 이미지 특징을 CNN으로 임베딩하여 유사한 이미지를 빠르게 검색.

```plaintext
예: 얼굴 인식 시스템에서 저장된 얼굴과 입력된 얼굴 비교.
```

4. **이상 탐지(Anomaly Detection)**:
    - 정상 행동 패턴을 벡터로 저장하고 새로운 데이터와 비교하여 이상 징후 탐지.

```plaintext
예: 금융 거래에서 사기 탐지.
```

5. **멀티모달 검색(Multi-modal Search)**:
    - 텍스트와 이미지 등 여러 유형의 데이터를 동시에 검색 가능.

```plaintext
예: 소셜 미디어에서 텍스트와 이미지 기반으로 게시물 찾기.
```


---

## **5. 벡터 데이터베이스 설계 시 고려사항**

### 설계 원칙

1. **임베딩 품질**:
    - 고품질 임베딩 생성이 중요하며, 도메인에 적합한 모델 선택 필요(BERT, CLIP 등).
2. **인덱싱 알고리즘 선택**:
    - 데이터 규모와 쿼리 속도 요구사항에 따라 적합한 ANN 알고리즘 선택(HNSW, LSH 등).
3. **메타데이터 관리**:
    - 추가적인 필터링 조건(예: 날짜, 카테고리)을 위한 메타데이터 저장 필요.
4. **확장성 계획**:
    - 샤딩(Sharding)을 통해 분산 환경에서 효율적으로 데이터 관리.

---

## **6. 실습 코드 예제 (Milvus 기반)**

### 6.1 설치 및 초기화

```python
from pymilvus import connections, CollectionSchema, FieldSchema, DataType

# Milvus 연결
connections.connect(host="localhost", port="19530")

# 컬렉션 스키마 정의
field_id = FieldSchema(name="id", dtype=DataType.INT64, is_primary=True)
field_vector = FieldSchema(name="embedding", dtype=DataType.FLOAT_VECTOR, dim=128)
schema = CollectionSchema(fields=[field_id, field_vector], description="Example collection")

# 컬렉션 생성
from pymilvus import Collection
collection = Collection(name="example_collection", schema=schema)
```


### 6.2 데이터 삽입 및 검색

```python
import numpy as np

# 데이터 삽입 (벡터 생성)
data = [
    [i for i in range(100)],  # ID 값
    np.random.random((100, 128)).tolist()  # 임베딩 값
]
collection.insert(data)

# 유사도 검색 수행
from pymilvus import utility

query_vector = np.random.random((1, 128)).tolist()
search_params = {"metric_type": "L2", "params": {"nprobe": 10}}
results = collection.search(data=query_vector, anns_field="embedding", param=search_params, limit=5)
print(results)
```

---

## 결론

벡터 데이터베이스는 AI 및 ML 응용 프로그램에서 비정형 데이터를 효율적으로 관리하고 의미 기반 검색을 수행하는 데 필수적인 도구입니다. 자연어 처리(NLP), 추천 시스템, 이상 탐지 등 다양한 분야에서 활용 가능하며 MLOps 엔지니어가 대규모 데이터를 다룰 때 강력한 솔루션을 제공합니다.

<div style="text-align: center">⁂</div>

[^1]: https://www.instaclustr.com/education/vector-databases-explained-use-cases-algorithms-and-key-features/

[^2]: https://www.databricks.com/glossary/vector-database

[^3]: https://nexla.com/ai-infrastructure/vector-databases/

[^4]: https://www.instaclustr.com/education/vector-database-13-use-cases-from-traditional-to-next-gen/

[^5]: https://www.cloudraft.io/blog/top-5-vector-databases

[^6]: https://www.instaclustr.com/education/top-10-open-source-vector-databases/

[^7]: https://www.decube.io/post/vector-database-concept

[^8]: https://www.cybrosys.com/blog/what-is-a-vector-database-and-it-s-top-7-key-features

[^9]: https://en.wikipedia.org/wiki/Vector_database

[^10]: https://www.oracle.com/pl/database/vector-database/

[^11]: https://www.youtube.com/watch?v=t9IDoenf-lo

[^12]: https://myscale.com/blog/key-features-vector-databases-developers/

[^13]: https://www.mongodb.com/resources/basics/vector-databases

[^14]: https://www.ibm.com/think/topics/vector-database

[^15]: https://www.oracle.com/gr/database/vector-database/

[^16]: https://learn.microsoft.com/en-us/data-engineering/playbook/solutions/vector-database/

[^17]: https://blog.det.life/vector-database-concepts-and-examples-f73d7e683d3e

[^18]: https://thedataquarry.com/blog/vector-db-2

[^19]: https://datascience.stackexchange.com/questions/123181/how-do-vector-databases-work-for-the-lay-coder

[^20]: https://www.cloudflare.com/learning/ai/what-is-vector-database/

[^21]: https://www.intersystems.com/resources/what-are-vector-databases-and-how-do-they-work/

[^22]: https://www.youtube.com/watch?v=dN0lsF2cvm4

[^23]: https://www.pinecone.io/learn/vector-database/

[^24]: https://www.deeplearning.ai/short-courses/building-applications-vector-databases/

[^25]: https://stackoverflow.blog/2023/10/09/from-prototype-to-production-vector-databases-in-generative-ai-applications/

[^26]: https://www.linkedin.com/pulse/top-8-vector-database-use-cases-2023-sarfraz-nawaz

[^27]: https://qdrant.tech/use-cases/

[^28]: https://lakefs.io/blog/12-vector-databases-2023/

[^29]: https://www.techtarget.com/searchdatamanagement/tip/Top-industry-use-cases-for-vector-databases

[^30]: https://weaviate.io/blog/what-is-a-vector-database

[^31]: https://www.rockset.com/blog/5-use-cases-for-vector-search/

[^32]: https://www.contentful.com/blog/what-are-vector-databases/

[^33]: https://qdrant.tech/articles/what-is-a-vector-database/

[^34]: https://www.timescale.com/blog/how-to-choose-a-vector-database

[^35]: https://www.reddit.com/r/rust/comments/18bradi/building_an_open_source_vector_database_looking/

[^36]: https://myscale.com/blog/develop-vector-database-best-practices/

[^37]: https://milvus.io/blog/deep-dive-1-milvus-architecture-overview.md

[^38]: https://dev.to/sebastiandevelops/understanding-vector-databases-a-beginners-guide-20nj

[^39]: https://www.elastic.co/blog/how-to-choose-a-vector-database

[^40]: https://zilliz.com/learn/what-is-vector-database

[^41]: https://www.youtube.com/watch?v=8KrTO9bS91s

[^42]: https://www.pinecone.io/learn/an-opinionated-checklist-to-choose-a-vector-database/

[^43]: https://qdrant.tech

[^44]: https://milvus.io

[^45]: https://db-engines.com/en/ranking/vector+dbms

[^46]: https://www.reddit.com/r/LangChain/comments/170jigz/my_strategy_for_picking_a_vector_database_a/

[^47]: https://media.datacamp.com/legacy/v1694512267/image4_fb2b10e9ca.png?sa=X\&ved=2ahUKEwieh4un2YiMAxWW1DgGHTpoIrAQ_B16BAgHEAI

[^48]: https://www.splunk.com/en_us/blog/learn/vector-databases.html

[^49]: https://www.elastic.co/what-is/vector-database

[^50]: https://aws.amazon.com/what-is/vector-databases/

[^51]: https://docs.langflow.org/components-vector-stores

[^52]: https://aerospike.com/blog/what-is-vector-database/

[^53]: https://www.algolia.com/blog/ai/how-does-a-vector-database-work-a-quick-tutorial

[^54]: https://www.v7labs.com/blog/vector-databases

[^55]: https://lakefs.io/blog/what-is-vector-databases/

[^56]: https://research.aimultiple.com/vector-database-use-cases/

[^57]: https://www.datacamp.com/blog/the-top-5-vector-databases

[^58]: https://zilliz.com/learn/beginner-guide-to-implementing-vector-databases

[^59]: https://docs.datarobot.com/en/docs/gen-ai/vector-database/vector-dbs.html

[^60]: https://www.digitalocean.com/community/conceptual-articles/how-to-choose-the-right-vector-database

[^61]: https://www.linkedin.com/pulse/vector-databases-demystified-part-2-building-your-own-adie-kaye

[^62]: https://www.techtarget.com/searchdatamanagement/tip/Top-vector-database-options-for-similarity-searches

