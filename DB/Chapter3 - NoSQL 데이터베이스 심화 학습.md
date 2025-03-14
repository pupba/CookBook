# OJT 교육 문서: NoSQL 데이터베이스 심화 학습

## **개요**

NoSQL 데이터베이스는 "Not Only SQL"을 의미하며, 관계형 데이터베이스(RDB)와는 다른 방식으로 데이터를 저장하고 관리합니다. 이 문서는 NoSQL의 주요 개념, 유형, 활용 사례를 다루며, MLOps 엔지니어가 실무에서 NoSQL을 효과적으로 사용할 수 있도록 돕기 위한 자료입니다.

---

## **1. NoSQL 데이터베이스란?**

### 정의

- NoSQL은 **비관계형 데이터베이스**로, 고정된 스키마 없이 데이터를 저장하고 관리합니다.
- 다양한 데이터 모델을 지원하며, 대규모 데이터 처리와 유연성을 제공합니다.


### 주요 특징

- **스키마리스 구조**: 사전에 테이블 구조를 정의하지 않아도 됩니다.
- **확장성**: 수평적 확장이 가능하여 대규모 데이터를 처리할 수 있습니다.
- **유연한 데이터 모델**: 정형, 반정형, 비정형 데이터를 처리할 수 있습니다.
- **빠른 성능**: 고속 읽기/쓰기 작업에 최적화되어 있습니다.

---

## **2. NoSQL 데이터베이스의 주요 유형**

### **2.1 Key-Value 데이터베이스**

- 데이터를 **키(Key)**와 **값(Value)** 쌍으로 저장하는 가장 단순한 형태의 DB입니다.
- **특징**:
    - 빠른 읽기/쓰기 성능 제공.
    - 캐싱, 세션 관리, 실시간 추천 시스템에 적합.
    - 복잡한 쿼리보다는 단순한 키 기반 조회에 최적화.
- **대표 사례**: Redis, Amazon DynamoDB.

```plaintext
Key: User123
Value: {"Name": "John", "Age": 30, "City": "New York"}
```

---

### **2.2 Document 데이터베이스**

- 데이터를 JSON, BSON 등 문서(Document) 형식으로 저장합니다.
- **특징**:
    - 스키마리스 구조로 유연한 데이터 관리 가능.
    - 복잡한 계층 구조와 다양한 속성을 가진 데이터를 처리할 수 있음.
    - 개별 문서에 대해 복잡한 쿼리 가능.
- **대표 사례**: MongoDB, Couchbase.

```json
{
  "CustomerID": 12345,
  "Name": "Alice",
  "Orders": [
    {"OrderID": 1, "Product": "Laptop", "Price": 1200},
    {"OrderID": 2, "Product": "Phone", "Price": 800}
  ]
}
```

---

### **2.3 Wide-Column 데이터베이스**

- 데이터를 테이블 형식으로 저장하되 열(Column)이 동적으로 구성됩니다.
- **특징**:
    - 대규모 분산 환경에서 효율적인 데이터 저장 및 조회 가능.
    - 분석 작업 및 대량의 비정형 데이터를 처리하는 데 적합.
- **대표 사례**: Apache Cassandra, HBase.

| Name     | Age | Address       | Orders        |
| :------- | :-- | :------------ | :------------ |
| John Doe | 30  | New York      | Laptop, Phone |
| Jane Roe | 25  | San Francisco | Tablet        |

---

### **2.4 Graph 데이터베이스**

- 데이터를 노드(Node), 엣지(Edge), 속성(Property)로 표현하여 관계를 저장합니다.
- **특징**:
    - 복잡한 관계를 자연스럽게 표현 가능.
    - 소셜 네트워크 분석, 추천 시스템 등에 적합.
- **대표 사례**: Neo4j.

```plaintext
Node: User123 (Name: John)
Edge: Follows -> User456 (Name: Alice)
```

---

### **2.5 벡터 데이터베이스**

- 데이터를 고차원 벡터 형태로 저장하며 AI 모델과 관련된 임베딩 정보를 관리합니다.
- **특징**:
    - 유사도 검색(Similarity Search)에 특화됨.
    - 텍스트, 이미지 등 비정형 데이터를 벡터화하여 저장 및 검색 가능.
- **대표 사례**: Pinecone, Weaviate.

```plaintext
Vector: [0.12, -0.45, 0.34] -> Represents the embedding of a sentence or image
```

---

## **3. NoSQL의 장단점**

### 장점

1. **확장성**:
    - 수평적 확장이 가능하여 대규모 데이터를 처리할 수 있습니다.
2. **유연성**:
    - 다양한 유형의 데이터를 처리할 수 있습니다(정형/비정형).
3. **고속 처리**:
    - 읽기/쓰기 작업이 빠르며 실시간 응용 프로그램에 적합합니다.

### 단점

1. **복잡한 관계 처리 어려움**:
    - RDB에 비해 관계성이 복잡한 데이터 처리에는 부적합할 수 있습니다.
2. **표준화 부족**:
    - 각 NoSQL DB마다 쿼리 언어와 사용 방식이 다릅니다.

---

## **4. 실무 활용 사례**

### 활용 사례 설명

1. **실시간 추천 시스템**:
    - Key-value DB를 사용해 사용자 행동 데이터를 저장하고 빠르게 조회하여 추천 제공.
2. **전자상거래 플랫폼**:
    - Document DB를 사용해 제품 정보와 고객 주문을 JSON 형식으로 저장하여 유연하게 관리.
3. **소셜 네트워크 분석**:
    - Graph DB를 사용해 사용자 간 관계를 분석하고 연결성을 기반으로 추천 기능 구현.
4. **AI 임베딩 관리 및 검색**:
    - 벡터 DB를 사용해 텍스트 또는 이미지 임베딩을 저장하고 유사도 기반 검색 수행.

---

## 결론

NoSQL은 MLOps 엔지니어가 비정형 데이터를 효율적으로 관리하고 실시간 처리를 구현하는 데 필수적인 도구입니다. 각 유형의 특성과 활용 사례를 이해하면 프로젝트 요구사항에 맞는 최적의 솔루션을 선택할 수 있습니다.

<div style="text-align: center">⁂</div>

[^1]: https://www.mongodb.com/resources/basics/databases/nosql-explained

[^2]: https://www.instaclustr.com/education/nosql-databases-types-use-cases-and-8-databases-to-try-in-2024/

[^3]: https://www.nobleprog.co.kr/cc/nosql

[^4]: https://www.couchbase.com/resources/why-nosql/

[^5]: https://docs.aws.amazon.com/whitepapers/latest/choosing-an-aws-nosql-database/types-of-nosql-databases.html

[^6]: https://www.adaface.com/blog/skills-required-for-nosql-developer/

[^7]: https://planetcassandra.org/post/architects-guide-to-using-nosql-for-real-time-ai-part-1/

[^8]: https://www.altexsoft.com/blog/nosql-databases/

[^9]: https://www.nobleprog.co.kr/en/cc/nosql

[^10]: https://www.coursera.org/learn/introduction-to-nosql-databases

[^11]: https://blazeclan.com/blog/dive-deep-types-nosql-databases/

[^12]: https://github.com/pregismond/working-with-nosql-databases

[^13]: https://www.datacamp.com/blog/nosql-databases-what-every-data-scientist-needs-to-know

[^14]: https://airbyte.com/top-etl-tools-for-sources/types-of-nosql-databases

[^15]: https://www.youtube.com/watch?v=xh4gy1lbL2k

[^16]: https://www.thoughtworks.com/insights/blog/nosql-databases-overview

[^17]: https://redis.io/nosql/what-is-nosql/

[^18]: https://octovion.com/online-training-nosql-certification-courses.html

[^19]: https://www.youtube.com/watch?v=Xx3QYuLUgE0

[^20]: https://www.testgorilla.com/test-library/programming-skills-tests/no-sql-databases-test/

