# OJT 교육 문서: 관계형 데이터베이스(RDB) 심화 학습

## **개요**

관계형 데이터베이스(RDB)는 데이터를 테이블 형식으로 저장하여 관계를 정의하고 관리하는 데이터베이스입니다. 본 문서는 컴퓨터공학 4학년 수준으로 RDB의 핵심 개념과 실무 활용 방안을 다룹니다.

---

## **1. 관계형 데이터베이스란?**

### 정의

- 데이터를 **테이블** 형식으로 저장하며, 각 테이블은 행(Row)과 열(Column)로 구성됩니다[^1][^2].
- 테이블 간의 관계를 **키(Key)** 를 통해 정의하여 데이터를 연결하고 중복을 최소화합니다[^1][^3].


### 주요 특징

- **스키마 기반 구조**: 테이블의 구조(열, 데이터 타입 등)가 사전에 정의되어야 합니다.
- **ACID 속성**: 트랜잭션의 안정성과 일관성을 보장합니다.
    - Atomicity(원자성): 트랜잭션은 모두 실행되거나 모두 실패합니다.
    - Consistency(일관성): 데이터는 항상 유효한 상태를 유지합니다.
    - Isolation(격리성): 트랜잭션은 서로 간섭하지 않습니다.
    - Durability(지속성): 트랜잭션 완료 후 데이터는 영구적으로 저장됩니다[^2][^3].
- **SQL 사용**: 데이터를 삽입, 수정, 삭제, 조회하기 위해 구조화된 질의 언어(SQL)를 사용합니다[^2].

---

## **2. 관계형 데이터 모델**

### 테이블 구성 요소

- **행(Row)**: 데이터를 저장하는 단위로, 각 행은 하나의 레코드(또는 튜플)를 나타냅니다[^4].
- **열(Column)**: 데이터의 속성을 나타내며, 각 열은 특정 데이터 타입을 가집니다[^4].
- **도메인(Domain)**: 열에 저장될 수 있는 값의 범위를 정의합니다[^4].


### 키(Key)

- **Primary Key**:
    - 각 행을 고유하게 식별하는 열입니다.
    - 중복 값이 없으며 NULL 값을 허용하지 않습니다[^2][^3].
- **Foreign Key**:
    - 다른 테이블의 Primary Key를 참조하여 테이블 간 관계를 설정합니다[^1][^3].

RDB는 Table간의 Foreign key를 통해 연결하여 하나의 상태가 변하면 다른 상태도 같이 변하게 할 수 있음.
USER Table 의 경우 사용 매우 많이함
info table
idpw table
subscribe table

---

## **3. SQL 기본 개념**

### SQL의 주요 명령어

- **Data Definition Language (DDL)**:
    - CREATE: 새로운 테이블 생성.
    - ALTER: 기존 테이블 수정.
    - DROP: 테이블 삭제.
- **Data Manipulation Language (DML)**:
    - INSERT: 데이터 삽입.
    - UPDATE: 데이터 수정.
    - DELETE: 데이터 삭제.
    - SELECT: 데이터 조회[^2][^8].


### 예제

```sql
-- 고객 테이블 생성
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    CustomerName VARCHAR(100),
    ContactName VARCHAR(50),
    Address VARCHAR(255),
    City VARCHAR(50),
    PostalCode VARCHAR(20),
    Country VARCHAR(50)
);

-- 고객 정보 삽입
INSERT INTO Customers (CustomerID, CustomerName, ContactName, Address, City, PostalCode, Country)
VALUES (1, 'Alfreds Futterkiste', 'Maria Anders', 'Obere Str. 57', 'Berlin', '12209', 'Germany');

-- 고객 정보 조회
SELECT * FROM Customers WHERE City = 'Berlin';
```

---

## **4. 관계형 데이터베이스 관리 시스템(RDBMS)**

### 특징

- 여러 사용자가 동시에 접근 가능하며, 데이터 무결성을 유지합니다[^2][^3].
- 자동화된 백업 및 복구 기능 제공.
- 성능 최적화를 위한 인덱스와 쿼리 최적화 지원[^2].


### 주요 RDBMS 소프트웨어

| 이름                   | 특징                      |
| :------------------- | :---------------------- |
| MySQL                | 오픈 소스 및 높은 확장성 제공[^8].  |
| Microsoft SQL Server | 윈도우 환경에서 강력한 지원 제공[^8]. |
| Oracle               | 대규모 엔터프라이즈 환경에 적합.      |

---

## **5. 실무 활용 사례**

### 사례 설명

1. **전자상거래 시스템**:
    - 고객 정보와 주문 정보를 별도의 테이블로 저장하고 `CustomerID`와 `OrderID`를 통해 연결하여 효율적으로 관리합니다[^7][^8].

```sql
-- 고객과 주문 데이터를 조인하여 조회
SELECT Customers.CustomerName, Orders.OrderDate, Orders.OrderID
FROM Customers
INNER JOIN Orders ON Customers.CustomerID = Orders.CustomerID;
```

2. **재고 관리**:
    - 제품 정보와 재고 상태를 분리하여 관리하고 필요 시 데이터를 조합해 보고서를 생성합니다.

---

## **6. RDB 설계 원칙**

### 정규화(Normalization)

데이터 중복을 줄이고 무결성을 유지하기 위해 테이블을 분리하는 과정입니다.

- **1NF**: 모든 열이 원자 값만 포함하도록 설계.
- **2NF**: 모든 비프라이머리 속성이 기본 키에 완전히 종속되도록 설계.
- **3NF**: 비프라이머리 속성이 다른 비프라이머리 속성에 종속되지 않도록 설계[^6].

---

## 결론

관계형 데이터베이스는 MLOps 엔지니어가 데이터를 효율적으로 관리하고 분석하는 데 필수적인 도구입니다. 본 문서에서 다룬 개념과 실습 예제를 통해 실무에서 RDB를 효과적으로 활용할 수 있는 기초를 다질 수 있습니다.

<div style="text-align: center">⁂</div>

[^1]: https://cloud.google.com/learn/what-is-a-relational-database

[^2]: https://www.techtarget.com/searchdatamanagement/definition/RDBMS-relational-database-management-system

[^3]: https://www.linode.com/docs/guides/relational-database-overview/

[^4]: https://www.teachoo.com/22089/4579/Relational-Data-Model/category/Concepts/

[^5]: https://www.vssut.ac.in/lecture_notes/lecture1423726199.pdf

[^6]: https://learn.microsoft.com/en-us/training/modules/explore-relational-data-offerings/

[^7]: https://www.ibm.com/think/topics/relational-databases

[^8]: https://www.w3schools.com/mysql/mysql_rdbms.asp

[^9]: https://workforce-central.org/wp-content/uploads/2017/08/wfcojtguidancemanualfinal6.pdf

[^10]: https://adacomputerscience.org/concepts/dbs_concepts

[^11]: http://kamarajcollege.ac.in/Department/Computer Science/II Year/e003 Core 25 Relational Database Management System - IV Sem.pdf

[^12]: https://www.serverwatch.com/storage/relational-database/

[^13]: https://isaaccomputerscience.org/concepts/data_dbs_concepts

[^14]: https://study.com/academy/course/computer-science-107-database-fundamentals.html

[^15]: https://www.udemy.com/course/databases-learnit/

[^16]: https://www.studysmarter.co.uk/explanations/computer-science/databases/relational-databases/

[^17]: https://www.opit.com/magazine/rdbms/

[^18]: https://codeinstitute.net/global/blog/what-is-a-relational-database-management-system/

[^19]: https://www.101computing.net/relational-databases/

[^20]: https://mrcet.com/downloads/digital_notes/CSE/II Year/DBMS.pdf

