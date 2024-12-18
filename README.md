# TravelMVCProject

자바 (Eclipse)와 Oracle DB를 사용한 여행 상품 예약 시스템

## 🖥️ 프로젝트 소개

이 프로젝트는 고객이 여행 상품을 예약하고, 예약한 상품에 대해 리뷰를 작성할 수 있는 여행 대행사 프로그램입니다. 고객, 예약, 여행 상품, 가이드 정보 등을 등록하고 관리할 수 있으며, 예약 진행 및 예약 후 리뷰 작성 기능이 포함되어 있습니다. 또한, 사용자는 입력된 정보를 수정하고 삭제할 수 있으며, 특정 기준에 따라 데이터를 정렬하는 기능도 제공합니다.

## 🕰️ 개발 기간

- 2023.11.28일 - 2023.12.01일

### 🧑‍🤝‍🧑 개발자

- [김연아] - 프로젝트 설계, 데이터베이스 구축, 기능 구현, 통합 관리

### ⚙️ 개발 환경

- `Java 17`
- **IDE**: Eclipse
- **Database**: Oracle SQL Developer
- **jdk**: 21.0.4

## 📌 주요 기능

#### 1. 고객 정보 관리

- 고객 정보 등록 및 수정 (이름, 생년월일, 국가, 성별, 이메일, 전화번호 등)
- 고객 정보 조회 및 삭제 기능

#### 2. 여행 상품 관리

- 여행 상품 등록 및 수정 기능 (상품명, 가격, 여행지, 가이드 정보 등)
- 여행 상품에 대한 상세 정보 확인 및 삭제 기능

#### 3. 예약 관리

- 고객이 여행 상품을 선택하고 예약할 수 있는 기능
- 예약 내역 조회 및 예약 취소 기능

#### 4. 리뷰 관리

- 고객이 예약한 여행 상품에 대해 리뷰 작성 기능
- 리뷰에 대한 평균 별점 및 세부 평가 (가이드 및 일정)와 평점 높은 순으로 순위 보기 기능

#### 5. 가이드 관리

- 여행 상품에 소속된 가이드 정보를 관리 (이름, 전화번호, 사용 언어 등)
- 가이드 정보 수정 및 삭제 기능

#### 6. 데이터베이스 설계

- Oracle DB를 사용하여 **고객**, **예약**, **여행 상품**, **가이드**, **리뷰** 테이블을 관리
- 각 테이블에 대한 PK(기본키), FK(외래키), 제약조건 설정
- 시퀀스를 이용하여 자동 증가하는 ID 관리

#### 7. SQL 스크립트

- 데이터베이스 테이블 생성 및 관계 설정을 위한 SQL 스크립트 제공
- 테이블 간 관계 설정 및 제약 조건 설정 (예: 외래키, 기본키 등)

#### 8. ERD 와 테이블표
---

<img src="https://github.com/KimYeonA1/TravelMVCProject/blob/main/Image/travel_erd.png" width = "800px" height = "500px">

---

<img src="https://github.com/KimYeonA1/TravelMVCProject/blob/main/Image/%ED%85%8C%EC%9D%B4%EB%B8%94%ED%91%9C.png" width = "800px" height = "500px">

---

## 🧩 데이터베이스 스키마

프로젝트에서 사용된 주요 테이블과 관계는 아래와 같습니다:

- **CUSTOMERS**: 고객 정보 관리
- **RESERVATION**: 고객 예약 관리
- **PACKAGES**: 여행 상품(패키지상품) 관리
- **GUIDE**: 가이드 정보 관리
- **REVIEW**: 예약한 여행 상품에 대한 리뷰 관리

```sql
-- 고객 테이블
CREATE TABLE CUSTOMER (
    NO NUMBER, --PK
    ID VARCHAR2(30), --UK
    NAME VARCHAR2(50) NOT NULL,
    BIRTH NUMBER NOT NULL,
    NATIONAL VARCHAR2(20) NOT NULL,
    GENDER VARCHAR2(10) NOT NULL,
    EMAIL VARCHAR2(50) NOT NULL,
    PHONE VARCHAR2(14) NOT NULL
);
Alter table CUSTOMER add constraint CUSTOMER_NO_PK primary key(NO);
Alter table CUSTOMER add constraint CUSTOMER_ID_UK UNIQUE(ID); 

CREATE SEQUENCE CUSTOMER_SEQ
START WITH 1
INCREMENT BY 1;
```

```sql
-- 여행상품 테이블
CREATE TABLE PACKAGE (
    NO NUMBER, --PK
    ID VARCHAR2(30),--UK
    NAME VARCHAR2(50) NOT NULL,
--    PCAPACITY NUMBER,
    NATIONAL VARCHAR2(20) NOT NULL,
    PRICE NUMBER NOT NULL,
    GUIDE_ID VARCHAR2(30), --FK
    SDATE DATE DEFAULT SYSDATE,
    EDATE DATE DEFAULT SYSDATE
);
Alter table PACKAGE add constraint PACKAGE_NO_PK primary key(NO);
Alter table PACKAGE add constraint PACKAGE_ID_UK UNIQUE(ID);   
Alter table PACKAGE add constraint PACKAGE_GUIDE_ID_FK 
    FOREIGN key(GUIDE_ID) References GUIDE(ID) on delete set null;
 

CREATE SEQUENCE PACKAGE_SEQ
START WITH 1
INCREMENT BY 1;

```

```sql
-- 예약 테이블
CREATE TABLE RESERVATION(
    NO NUMBER, --PK
    ID VARCHAR2(30), --UK
    CUST_ID VARCHAR2(30), --FK
    PACK_ID VARCHAR2(30), --FK
--    RCAPACITY NUMBER NOT NULL,
    METHOD VARCHAR2(20) NOT NULL,
    RDATE DATE DEFAULT SYSDATE
);
Alter table RESERVATION add constraint RESERVATION_NO_PK primary key(NO);
Alter table RESERVATION add constraint RESERVATION_ID_UK UNIQUE(ID);  
Alter table RESERVATION add constraint RESERVATION_CUST_ID_FK 
    FOREIGN key(CUST_ID) References CUSTOMER(ID) on delete set null;
Alter table RESERVATION add constraint RESERVATION_PACK_ID_FK 
    FOREIGN key(PACK_ID) References PACKAGE(ID) on delete set null;   
ALTER TABLE RESERVATION MODIFY RDATE DEFAULT SYSDATE;    
       
CREATE SEQUENCE RESERVATION_SEQ
START WITH 1
INCREMENT BY 1;

```

```sql
-- 가이드 테이블
CREATE TABLE GUIDE (
    NO NUMBER, --PK
    ID VARCHAR2(30), --UK
    NAME VARCHAR(50) NOT NULL,
    PHONE VARCHAR(15) NOT NULL,
    LANGUAGES VARCHAR(100) NOT NULL
);
Alter table GUIDE add constraint GUIDE_NO_PK primary key(NO);
Alter table GUIDE add constraint GUIDE_ID_UK UNIQUE(ID); 

CREATE SEQUENCE GUIDE_SEQ
START WITH 1
INCREMENT BY 1;
```

```sql
-- 리뷰 테이블
CREATE TABLE REVIEW(
    NO NUMBER, --PK
    RESERV_ID VARCHAR2(30), --FK
    GUIDE_REVIEW NUMBER(2) NOT NULL,
    SCHE_REVIEW NUMBER(2) NOT NULL,
    AVG_REVIEW NUMBER(3, 1)
);
Alter table REVIEW add constraint REVIEW_NO_PK primary key(NO);
Alter table REVIEW add constraint REVIEW_RESERV_ID_FK 
    FOREIGN key(RESERV_ID) References RESERVATION(ID) on delete set null;
  
--  drop sequence review_seq;    
CREATE SEQUENCE REVIEW_SEQ
START WITH 1
INCREMENT BY 1;

COMMIT;

--===============================================================
-- 리뷰의 평균값 구하는 트리거
CREATE OR REPLACE TRIGGER REVIEW_TRIGGER
BEFORE INSERT OR UPDATE ON REVIEW
FOR EACH ROW
BEGIN
    IF :NEW.GUIDE_REVIEW IS NOT NULL AND :NEW.SCHE_REVIEW IS NOT NULL THEN
        :NEW.AVG_REVIEW := (:NEW.GUIDE_REVIEW + :NEW.SCHE_REVIEW) / 2;
    ELSE
        -- 기본값 설정
        :NEW.AVG_REVIEW := 0;
    END IF;
END;
/
```


