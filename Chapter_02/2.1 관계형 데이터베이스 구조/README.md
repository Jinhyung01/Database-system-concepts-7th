## 2.1 관계형 데이터베이스 구조

관계형 DB는 **테이블**(table)의 모임으로 구성되며, 각 테이블은 고유한 이름을 가지고 있다.

<br/>

#### 용어

`릴레이션(Relation)` : 행과 열로 이루어진 테이블

<br/>

`릴레이션 스키마(Relation Schema)` : 릴레이션에 어떤 정보가 담길지(헤더)를 정의하는 역할

<br/>

`릴레이션 인스턴스(Relation instance)` : 릴레이션 스키마에 실제로 저장되는 데이터의 집합

<br/>

`속성(attribute)` : 테이블의 열

<br/>

`튜플(tuple)` : 테이블의 행

<br/>

`도메인(domain)` : 허가된 값의 집합(속성이 가질 수 있는 정보의 범위)

>  instuctor 릴레이션의 name속성의 도메인은 가능한 모든 교수의 이름
>
>  instuctor 릴레이션의  salary 속성의 도메인은 가능한 모든 salary값의 집합

<br/>

`널 값(null value)` : 알려지지 않거나 존재하지 않은 값을 의미하는 특별한 값

<br/>

**instructor 릴레이션**

- 각 행은 교수의 ID,name, dept_name, salary로 구성된 한 명의 교수에 대한 정보를 저장하고 있다.
- 교수 개개인은 ID값에 의해서 구별됨

![그림 2 1 instructor 릴레이션](https://github.com/user-attachments/assets/7bff7b78-249f-4ab6-a7e8-7de7ac8c54ab)

<br/>**course 릴레이션**

- 각각의 수업에 대한 course_id, title, dept_name, credits와 같은 수업 정보를 저장하고 있다.
- 각 과목은 course_id에 의해서 구별됨

![그림 2 2 course 릴레이션](https://github.com/user-attachments/assets/7892b6b4-bf62-4c03-9cce-db891d5bf923)

<br/>

**prereq 릴레이션**

- 각 수업의 선행 과목 정보를 저장한 테이블
- 두 번째 열의 수업이 첫 번째 열의 선행 과목이다.

![그림 2 3 prereq 릴레이션](https://github.com/user-attachments/assets/a76ba1ca-e253-4e2c-9aba-be1f9447faca)

<br/>

모든 릴레이션(r)의 모든 속성의 도메인은 **원자적**(atomic)이어야 한다.

> 원자적 : 도메인의 요소(element)가 더 이상 나뉠 수 없는 단위라는 것을 의미