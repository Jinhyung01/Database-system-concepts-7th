## 3.1 SQL 질의어의 개요

SQL 언어는 다음과 같은 여러 부분으로 구성되어 있다.

- **데이터 정의 언어(Data-definition language, DDL)**
  - SQL DDL은 **릴레이션 스키마를 정의, 수정하고 , 릴레이션을 삭제**하는 명령어를 제공한다.

<br/>

- **데이터 조작 언어(Data-manipulation language, DML)**
  - SQL DML은 정보를 찾기 위해 **DB에 질의하고, 튜플을 삽입,삭제,수정**하는 기능을 제공한다.

<br/>

- **무결성(Integrity)**
  - SQL DDL은 DB에 저장될 데이터가 반드시 만족해야 하는 **무결성 제약 조건을 명시하는 명령어를 포함한다**. 
  - DBMS는 무결성 제약 조건을 위반하는 갱신을 허용하지 않는다.

<br/>

- **뷰 정의(View definition)**
  - SQL DDL은 뷰를 정의할 수 있는 명령어를 포함한다.

<br/>

- **트랜잭션 제어(Transaction control)**
  - SQL은 트랜잭션의 시작과 끝을 명시하는 명령어를 포함한다.

<br/>

- **내장 SQL(Embedded SQL)과 동적 SQL(Dynamic SQL)**
  - 내장 SQL과 동적 SQL은 C, C++, Java와 같은 범용 프로그래밍 언어에 내장되어 사용될 수 있다.

<br/>

- **권한 부여(Authorization)**
  - SQL DDL은 릴레이션과 뷰에 접근할 권한을 부여하는 명령어를 포함한다.

<br/>

<br/>

> 이번 장은 SQL의 DML과 기본적인 DDL의 특성에 대해 배운다.
>
> DBMS는 이번 장에서 설명하는 대부분의 SQL 표준 특성을 지원한다. 
>
> SQL을 특정 DBMS가 이해할 수 있도록 표현하는 것은 DBMS별로 차이가 있다.