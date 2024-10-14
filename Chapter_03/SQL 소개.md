# Chapter 03 SQL 소개

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

<br/>

<br/>

<br/>

## 3.2 SQL 데이터 정의

DDL을 이용하면 DB의 릴레이션을 정의할 수 있다. 

SQL DDL은 릴레이션의 집합뿐만 아니라 다음과 같은 각 릴레이션에 관한 정보도 명세할 수 있게 해준다.

- 각 릴레이션의 스키마
- 각 속성값의 타입
- 무결성 제약 조건
- 각 릴레이션을 위해 만들어진 인덱스의 집합
- 각 릴레이션에 대한 보안 및 권한 부여 정보
- 디스크에 저장된 릴레이션의 물리적 저장 구조

<br/>

> 이 절에서는 스키마 정의와 타입 값에 대해서만 논의하겠다.

<br/>

<br/>

## 3.2.1 기본타입

SQL 표준은 다음의 다양한 내장 타입을 지원한다.

**``char(n)``**

- 사용자가 지정한 길이 n을 갖는 고정 길이 문자. 
- char 대신 character가 사용될 수도 있다.

<br/>

**``varchar(n)``**

- 사용자가 지정한 최대 길이 n을 갖는 가변 길이 문자.
- varchar 대신 character varying이 사용될 수도 있다.

<br/>

**``int``**

- 정수(시스템에 따라 달라질 수 있는 정수의 부분집합).
- int대신 integer가 사용될 수도 있다.

<br/>

**``smallint``**

- 작은 정수(시스템에 따라 달라질 수 있는 정수타입의 부분집합)

<br/>

**``numeric(p,d)``**

- 사용자가 지정한 정밀도(precision)를 갖는 고정 소수점 수(fixed-point number). 
- 수는 p개의 숫자(및 부호)로 구성되며, p개의 숫자 중 d개는 소수점 이하에 있는 숫자의 개수를 나타낸다. 
- 따라서 numeric(3,1)로 지정된 속성 혹은 열의 경우 44.5라는 값은 정확히 저장될 수 있지만 444.5나 0.32는 정확히 저장될 수 없다.

<br/>

**``real``**

- 시스템에 따라 달라질 수 있는 정밀도를 가지는 부동 소수점 수

<br/>

**``double precision``**

- 시스템에 따라 달라질 수 있는 정밀도를 가지는 배정밀도(double-precision) 부동 소수점 수

<br/>

**``float(n)``**

- 적어도 n개의 숫자로 나타낼 수 있는 정밀도를 갖는 부동 소수점 수

<br/>

> - [x] 각각의 타입은 null 값을 포함할 수 있다. 어떤 경우에는 널 값 입력 자체가 금지될 수도 있다.

> 추가적인 타입은 4.5절에서 다룬다.

<br/><br/>

### char타입과 varchar타입

``char`` 데이터 타입은 **고정 길이의 문자열**을 저장한다. 

예를들어, `char(10)` 타입의 속성 A에 `"Avi"`라는 문자열을 저장하면, 10문자 길이를 맞추기 위해 **7개의 공백이 추가된다**. 

<br/>

<br/>

``varchar`` 데이터 타입은 **가변 길이의 문자열**을 저장한다.

`varchar(10)` 타입의 속성 B에 `"Avi"`를 저장하면, **공백이 추가되지 않는다**. 

<br/>
<br/>

**``char`` 타입의 두 값을 비교할 때** 두 값의 길이가 다르면, 길이가 **짧은 값에 공백이 자동으로 추가**되어 두 값이 같은 크기가 되도록 만든다.

<br/>

그러나 **``char`` 타입과 ``varchar`` 타입을 비교할 때**는 **DB 시스템에 따라 ``varchar`` 타입의 값에 공백을 추가할 수도 안할 수도** 있어서 속성 A,B에 `"Avi"`라는 같은 값을 저장했다 하더라도 **A = B 비교 결과는 false가 될 수 있다**. 

<br/>
<br/>

이러한 문제를 없애기 위해 항상 char 타입 대신 **``varchar`` 타입을 사용하는 것을 추천**한다.

<br/>

또한 SQL은 ``nvarchar``타입을 제공해 **Unicode를 통해 다국어 데이터를 저장**할 수 있다.

많은 데이터베이스에서 사용하는 **`varchar`** 타입도 **UTF-8**을 통해 유니코드 문자를 지원하기에 다국어 데이터를 저장할 수 있다

<br/>

<br/>

## 3.2.2 기본 스키마 정의

SQL의 **create table** 문(statement)을 사용하여 **어떤 릴레이션을 정의**할 수 있다.

다음의 명령어는 department 릴레이션을 데이터베이스에 생성한다.

```sql
create table department
    (dept_name varchar(20),
     building  varchar(15),
     budget    numeric(12,2),
     primary key (dept_name));
```

위에서 만든 릴레이션은 세 개의 속성을 가지고 있다.

- dept_name : 최대 길이가 20인 문자열
- building : 최대 길이가 15인 문자열
- budget : 12개의 숫자로 구성되는데, 그중 두개는 소수점 이하 숫자

<br/>

create table문은 또한 dept_name이 릴레이션의 **주 키**임을 나타낸다.

<br/>
<br/>

create table 문의 **일반 구문**은 다음과 같다.

```sql
create table r
	(A1, D1,
	 A2, D2,
	 ...
	 An, Dn,
	 <integritiy-constraint1>
	 ...,
	 <integritiy-constratintk>
    );
```

**r** : 릴레이션 이름

**Ai** : 릴레이션 r의 스키마 속성의 이름

**Di** : 속성 Ai의 도메인

>  create table 문의 끝에 있는 **세미콜론(;)**은 DBMS에 따라 필요할 수도 있고 필요 없을 수도 있다.

<br/>

<br/>

SQL은 서로 다른 다수의 **무결성 제약 조건**을 지원한다.

> 이 절은 그중 일부만 다룬다.

- **primary key**(Aj1, Aj2, ..., Ajm) 
  - 이 주 키(primary key) 명세는 속성 Aj1, Aj2, ..., Ajm 등이 **릴레이션의 주 키를 구성**한다는 것을 나타낸다.
  - 주 키 속성은 **null이 아니거나 유일**해야한다.

<br/>

- **foreign key**(Ak1, Ak2, ..., Akn) **references s**
  - 외래 키(foreign key) 명세는 **릴레이션의 어떤 튜플에 대한 속성값**(Ak1, Ak2, ..., Akn)이 반드시 (상대)릴레이션 s가 갖고 있는 일부 튜플의 **주 키 속성값에 상응**해야한다.
  - course릴레이션에서 외래 키 선언은 각 course 튜플에 대해 그 튜플이 지정한 학과 이름은 반드시 department 릴레이션의 주 키 속성인 (dept_name)에 존재해야 한다는 것을 의미한다. 

<br/>

- **not null**
  - 속성에 대한 not null 제약 조건은 **그 속성의 도메인에서 null 값을 제외**한다.
  - instructor 릴레이션의 name 속성에 대한 **not null 제약 조건**은 **교수의 이름이 null이 될 수 없음을 보장**한다.

<br/>

#### 대학교 데이터베이스의 일부 SQL DDL 정의

![그림 3 1 대학교 데이터베이스의 일부분에 대한 SQL 데이터 정의](https://github.com/user-attachments/assets/72aa357d-c170-4556-8a8c-a0313bf44909)

 <br/>

<br/>

SQL은 **무결성 제약 조건을 위반하는 모든 DB 갱신을 막는다**.

> 예시 :  릴레이션에 새롭게 추가되거나 수정된 튜플이 주 키 속성에 대해 null 값을 가지거나, 혹은 튜플이 주 키 속성에 대해 릴레이션 내의 다른 튜플과 같은 값을 가지게 되면 SQL이 오류를 표시하고 해당 갱신을 막는다. 유사하게 department 릴레이션에 없는 dept_name을 갖는 course 튜플의 삽입은 course에 대한 외래 키 제약 조건 때문에 거절된다.

<br/>

<br/>

### 데이터 조작 명령어

- 튜플 삽입 : `insert`
- 튜플 갱신 : `update`
- 튜플 삭제 : `delete`

> 새로 생성되는 릴레이션은 초기에는 비어 있다.

<br/>

<br/>

### 릴레이션을 제거하기 위한 명령문

**`drop table` 문** 

- drop table은 데이터베이스에서 제거될 릴레이션과 관련된 모든 정보를 삭제한다.

- 사용 예시

  - ```sql
    drop table r;
    ```

<br/>

<br/>

**`delete from r;`과 차이점** 

- `delete from r;`
  - 릴레이션 **r의 모든 튜플을 제거**하지만, **릴레이션 자체는 남아 있음**.

<br/>

- `drop table r;`

  - 릴레이션 **r의 모든 튜플과 스키마까지 삭제**.

  - 릴레이션이 삭제되면 다시 `create table`을 사용하지 않으면 튜플 삽입 불가.

<br/>

<br/>

### 릴레이션 스키마(속성) 수정 명령어

**`alter table`문**

- 이미 존재하는 릴레이션에 속성을 추가할 수 있게 한다.

- 릴레이션의 모든 튜플에 새로운 속성값으로 null이 할당된다.

- alter table 구문은 다음과 같다.

  ```sql
  alter table r add A D;
  ```

  - **r**: 릴레이션 이름

    **A**: 추가될 속성의 이름

    **D**: 그 속성의 도메인

<br/>

- 릴레이션에 속성을 제거하려면 다음 구문을 사용할 수 있다.

  ```sql
  alter table r drop A;
  ```

  - **r**: 릴레이션 이름

    **A**: 제거될 속성의 이름

<br/>

<br/>

<br/>

## 3.3 SQL 질의의 구본 구조

SQL 표현의 기본 구조는 `select`, `from`, `where`의 세 개의 절로 이루어진다.

- **`from` 절**: 질의에 사용할 릴레이션을 지정

- **`where` 절**: 조건을 지정해 튜플 필터링

- **`select` 절**: 결과로 출력할 속성 선택

<br/>

<br/>

## 3.3.1 단일 릴레이션에 대한 질의

![그림 2 1 instructor 릴레이션](https://github.com/user-attachments/assets/7bff7b78-249f-4ab6-a7e8-7de7ac8c54ab)

### 모든 교수의 이름을 찾아라 라는 질의

```sql
select name
from instructor;
```

<br/>

위 질의의 **결과**

- name이라는 단일 속성만으로 구성된 릴레이션

![그림 3 2 select name from instructor의 결과](https://github.com/user-attachments/assets/11144162-5377-48ea-8813-3ce5f67746ec)

<br/>
<br/>

### 모든 교수의 소속 학과 이름을 찾아라 라는 질의

```sql
select dept_name
from instructor;
```

<br/>

위 질의의 **결과**

- 한 학과에 한 명 이상의 교수가 소속될 수 있으므로 학과 이름은 instructor 릴레이션에서  한 번 이상 나타날 수 있다.
- 관계형 모델의 정의상 릴레이션은 집합이므로 중복된 튜플은 릴레이션에 나타날 수 없다.
  - 그러나 SQL은 **중복을 허용**한다.
- 따라서 위 질의는 instructor 테이블에 저장된 **모든 교수**에 대해 **학과 이름**(dept_name)을 가져오며, 이 과정에서 중복된 학과 이름도 포함된다.

![그림 3 3 select dept_name from instructor의 결과](https://github.com/user-attachments/assets/3adf4d4b-7120-48e2-aab1-b021bb9fd32d)

<br/>

<br/>

#### **중복된 튜플(학과 이름)을 제거하려면?**

`select` 뒤에 `distinct`라는 키워드를 삽입하면 된다.

```sql
select distinct dept_name
from instructor;
```

위 질의에 관한 결과에서 각 학과 이름은 한 번만 포함된다.

<br/>

<br/>

<br/>

#### 중복을 명시적으로 작성하는법

SQL에서 중복을 포함하고 싶을때 명시적으로 `all`이라는 키워드를 사용할 수 있다.

```sql
select all dept_name
from instructor;
```

기본적으로 SQL이 중복이 허용되기 때문에 중복을 포함하고 싶을 때 별도로 `all`을 사용할 필요는 없다.

<br/>

<br/>

<br/>

<br/>

`select` 절에 **상수나 튜플의 속성**에 적용되는 +,-,*,/ 연산자를 포함하는 **산술 연산**을 사용할 수 있다.

예를 들어, 교수 급여에 10% 인상된 값을 확인하는 질의는 다음과 같다.

```sql
select ID,name,dept_name,salary*1.1
from instrucotr
```

이 질의는 급여가 10% 인상된 값을 출력하지만, 실제로 instructor 테이블에 저장된 데이터는 **변화가 없다.**

> DB에 저장된 데이터가 변경되지 않으며 단지 출력만 한다는 뜻.

<br/>

<br/>

<br/>

<br/>

`where` 절은 `from` 절의 릴레이션에 있는 튜플 중 `where` 절에 명시된 술어를 만족하는 행만 반환하게 한다.

예를 들어, "Computer Science과에서 급여가 $70,000이 넘는 모든 교수의 이름을 구하라"와 같은 질의를 고려해 보자.

```sql
select name
from instructor
where dept_name = 'Computer Science' and salary>70000;
```

<br/>

이 질의의 **결과**

![그림 3 4 컴퓨터 과학과에서 급여가 $70,000이 넘는 모든 교수의 이름을 구하라는 질의의 결과](https://github.com/user-attachments/assets/e1bcbbcb-14b5-492e-af3a-099806e7fc6c)

<br/>

<br/>

<br/>

**논리 접속사 및 비교 연산자**

SQL에서 `where` 절에 `and`, `or`, `not`과 같은 논리 접속사를 사용할 수 있다.

비교 연산자 `<`, `<=`, `>`, `>=`, `=`, `<>`를 포함하는 표현식은 논리 접속사의 피연산자가 될 수 있다.

<br/>

SQL에서 **날짜 타입**과 같은 특수 타입뿐만 아니라 **문자열이나 산술 표현의 비교를 위한 비교 연산자**도 사용할 수 있다.

> where 절에 있는 술어에 대한 다른 특성에 관해서는 이 장 뒷부분에서 다룬다.

<br/>

<br/>

## 3.3.2 복수의 릴레이션에 관한 질의

![instructor와 department 릴레이션](https://github.com/user-attachments/assets/066520db-ef7f-4267-a0f7-e010aa2c6cb8)

### 모든 교수의 이름과 함께, 그들의 학과 이름과 건물 이름을 구하라라는 질의

dept_name 속성은 instructor와 department 두 릴레이션에 모두 나타나고, 참조하고자 하는 속성이 어느 릴레이션에 속해 있는지 명시적으로 나타나도록 릴레이션 이름이 앞에 붙는다.(instructor.dept_name 및 department.dept_name과 같이)

<br/>

> 이 명명법을 사용하려면 from 절에 존재하는 릴레이션이 **고유한 이름**을 가져야한다.
>
> **같은 릴레이션의 다른 두 튜플의 정보**가 결합되어야 하는 경우 문제가 생길 수 있는데 이를 해결하려면 rename연산을 쓰면된다.

```sql
select name, instructor.dept_name, building
from instructor, departemnt
where instructor.dept_name = department.dept_name;
```

<br/>

위 질의의 **결과**는 아래와 같다.

![그림 3 5 모든 교수의 이름과 함께, 그들의 학과 이름과 건물 이름을 구하라의 결과](https://github.com/user-attachments/assets/e7f3c849-ee8f-4209-9669-7eb4d0d8a7fc)

<br/>

### 복수의 릴레이션에 대한 일반적인 SQL 질의

SQL질의는 `select` 절, `from`절, `where`절의 세 가지 유형의 절로 구성된다. 각 절의 역할은 다음과 같다.

- `select` 절 :  질의의 결과에 나타나야 할 속성을 나열하는데 사용된다.
- `from` 절 : 질의를 수행하기 위해 접근해야 하는 릴레이션을 나열한다.
- `where` 절 : `from` 절에 있는 릴레이션의 속성과 관련된 술어로 구성된다.

<br/>

일반적인 SQL 질의의 형식

- **Ai** : 속성
- **ri** : 릴레이션
- **P** : 술어

>  where절이 없다면 술어는 true다.

```sql
select A1,A2, ...,An
from r1, r2, ..., rm
where P;
```

<br/>

<br/>

`FROM` 절 자체는 나열된 릴레이션의 **카티션 곱**을 정의한다.

공식적으로 카티션 곱은 관계 대수에 따라 정의되지만, `FROM` 절의 결과 릴레이션에 대한 튜플을 생성하는 반복적인 과정으로 이해할 수 있다.

```sql
for each tuple t1 in relation r1  
	for each tuple t2 in relation r2
		...
		for each tuple tm in relation rm
			Concatenate t1, t2,..., tm into a single tupe t
			Add t into the result relation
```

결과 릴레이션은 `from` 절의 **모든 릴레이션에 있는 모든 속성**을 다 가진다.

동일한 속성 이름이 여러 릴레이션에 나타날 수 있으므로, **릴레이션 이름을 접두어**로 붙여 속성을 구분한다.

<br/>
<br/>

예를들어, instructor와 teaches 릴레이션의 카티션 곱의 릴레이션 스키마는 다음과 같다

```
(instructor.ID, instructor.name, instructor.dept_name, instructor.salary,
 teaches.ID, teaches.course_id, teaches.sec_id, teaches.semester, teaches.year)
```

이 스키마에서 instructor.ID와 teaches.ID는 서로 구분된다. 

<br/>

<br/>

두 스키마 중에서 한 스키마에만 나타나는 속성에 대해서는 릴레이션 이름이 생략되는데, 이는 그와 같이 간단히 표기해도 어떤 릴레이션의 속성인지 바로 알 수 있기 때문이다. 

결과 릴레이션의 스키마는 다음과 같다.

```
(instructor.ID,name,dept_name,salary,teaches.ID,course_id,sec_id,semester,year)
```

<br/>

<br/>

다음 두 릴레이션을 고려해보자

![instructor와 teaches릴레이션](https://github.com/user-attachments/assets/b19de4ba-f2cd-43c4-a24a-8ec0f0579b8b)

<br/>

두 릴레이션의 카티션곱은 아래와 같다.

![그림 3 6 instructor 릴레이션과 teaches 릴레이션의 카티션 곱](https://github.com/user-attachments/assets/07e07809-c077-4044-8be6-e2562d939dab)

<br/>

**카티션 곱**은 서로 관계가 없는 릴레이션의 튜플을 모두 결합한다.

위의 경우 `instructor` 릴레이션의 각 튜플을 `teaches`의 모든 튜플과 결합한다. 

이 과정에서 **모든 조합**이 생성되기 때문에, **매우 큰 릴레이션**이 만들어질 수 있다.

하지만 이런 식으로 만들어진 **모든 튜플 조합**이 의미가 있지는 않다. 

카티션 곱은 관계 없는 튜플들을 결합할 수 있기 때문에, **잘못된 결과**를 초래할 가능성이 크다.

<br/>

<br/>

<br/>

<br/>

카티션 곱으로 생성된 불필요한 조합을 필터링하기 위해 **`where` 절의 술어**를 사용한다. 

`instructor`와 `teaches`에 관련된 질의는 일반적으로  같은 ID값을 가진 `teaches`튜플과 `instructor` 튜플이 일치하기를 기대한다.

<br/>

다음 SQL 질의는 `instructor` 릴레이션과 `teaches` 릴레이션의 `ID` 속성을 기준으로 **일치하는 튜플**을 결합하고, **교수 이름과 과목 식별자**(course ID)를 출력한다.

```sql
select name, course_id
from instructor, teaches
where instructor.ID=teaches.ID; 
```

다만 이 질의는 일부 과목을 가르쳤던 교수만을 출력한다. 다시 말하면, 어떤 과목도 가르치지 않았던 교수는 출력되지 않는다.

> 만약 그러한 튜플을 보고 싶다면 외부조인(outer join)이라고 불리는 연산을 사용해야한다. <- 이 내용은 4.1.3절에서 설명

<br/>

위 질의의 수행**결과**

- 어떠한 과목도 가르치지 않았던, 교수 Gold, Califieri, Singh는 결과에 나타나지 않음

![그림 3 7 대학교 내에서 일부 과목을 가르친 적이 있는 모든 교수에 대해, 그들의 이름과 그들이 가르쳤던 과목의 과목 아이디를 찾아라의 결과](https://github.com/user-attachments/assets/d10158f7-8140-47f7-aaf2-89ddee2d81c8)

<br/>

<br/>

대학교 내에서 일부 과목을 가르친 적이 있는 모든 교수가 아닌 **컴퓨터 과학 교수**에 대해, 그들의 **이름**과 그들이 가르쳤던 과목의 **과목 ID**를 찾아라의 질의

```sql
select name, course_id
from instructor, teaches
where instructor.ID = teaches.ID and dept_name = 'Comp.Sci';
```

<br/>

<br/>

SQL 질의의 의미는 일반적으로 다음과 같다.

1. `from` 절에 나열된 릴레이션의 카티션 곱을 생성하라
2. 1번의 결과에 대해 `where` 절에 명시된 술어을 적용하라
3. 2번의 결과의 모든 튜플에 대해 `select`절에 명시된 속성(또는 표현의 결과)을 출력하라.

<br/>

<br/>

<br/>

## 3.4 부가적인 기본 연산

SQL에서 지원하는 몇 가지 부가적인 기본 연산이 있다.

<br/>

## 3.4.1 재명명 연산

앞서 사용했던 질의를 다시 보자

```sql
select name, course_id
from instuctor, teaches
where instructor.ID = teaches.ID;
```

<br/>

이 질의의 결과는 다음의 속성을 갖는 릴레이션이다.

```
name, course_id
```

위의 결과에서 속성 이름은 `from`절에 있는 릴레이션의 속성 이름으로부터 파생되었다.

> **원래 테이블의 속성 이름을 그대로 따르는 것**

<br/>

<br/>

그러나 다음과 같은 몇 가지 이유 때문에 항상 이와 같은 방법으로 이름을 파생시킬 수 없다.

1. `from`절에 있는 **두 개의 릴레이션이 같은 이름을 갖는 경우**, 그 속성의 이름이 결과내에서 중복될 수 있다.
2. **`select`절에서 산술 식을 사용했을 경우**, 그 결과의 속성은 이름을 가지지 않는다.
3. 앞선 예제처럼 속성 이름을 원래의 릴레이션으로부터 얻는다고 하더라도 결과에서 **그 속성의 이름을 바꾸고 싶은 경우**가 있다.

<br/>

따라서 SQL은 결과 릴레이션에서 속성 이름을 다시 지정하기 위해 `as` 절을 제공한다.

`as` 절은 `select`절과 `from` 절 둘다에서 나타날 수 있다.

```sql
old-name as new-name
```

<br/>

<br/>

예를들어, **name의 속성 이름을 instructor_name으로 바꾸고** 싶다면, 앞의 질의를 다음과 같이 다시 작성할 수 있다.(3번째 이유)

```sql
select name as instructor_name, course_id
from instructor, teaches
where instructor.ID = teaches.ID;
```

<br/>

<br/>

`as`절은 릴레이션의 이름을 다시 지을 때 특히 유용하다.

릴레이션의 이름을 다시 짓는 한 가지 이유는 질의의 어디에서나 더 편리하게 사용하기 위해 **길이가 긴 릴레이션 이름을 길이가 짧은 것으로 바꾸기 위함**이다.

<br/>

예를들어, "**대학교 내에서 일부 과목을 가르친 적이 있는 모든 교수에 대해, 그들의 이름과 그들이 가르쳤던 과목 아이디를 찾아라**" 라는 질의가 있을때 이를 다음과 같이 재작성할 수있다.

```sql
select T.name, S.course_id
from instructor as T, teaches as S
where T.ID = S.ID
```

<br/>

<br/>

<br/>

릴레이션의 이름을 다시 짓는 또 다른 이유는 **같은 릴레이션에서 튜플을 비교**할 때다.

릴레이션을 **자기 자신과 카티션 곱을 수행해야 하는 경우**를 생각해보자

만약 릴레이션의 이름을 다시 지을 수 없다면, 카티션 곱에 참여하는 릴레이션의 튜플을 구분할 수 없다.

<br/>

예를들어, "**적어도 생물학과 한 교수보다 급여가 많은 모든 교수의 이름을 구하라**"는 질의 를 작성한다면 SQL로 다음과 같이 나타낼 수 있다.

```sql
select distinct T.name
from instructor as T, instructor as S
where T.salary > S.salary and S.dept_name = 'Biology';
```

<br/>

instructor.salary는 어떤 instructor를 가리키는지 명확하지 않으므로 사용할 수 없다.

이 질의에서 T와 S는 instructor 릴레이션의 복사본으로 생각될 수 있지만, 조금 더 정확하게는 **별칭**으로, instructor 릴레이션에 대해 **대체되는 이름으로 선언**되었다.

T,S와 같은 식별자는 릴레이션의 이름을 다시 짓기 위해 사용되며 SQL 표준에서는 **상관 이름**(correlation name)이라고 불리기도 하나, 흔히 **테이블 별칭**(table alias), **상관변수**(correlation variable), **튜플 변수**(tuple variable)라고도 부른다.

<br/>

<br/>

<br/>

## 3.4.2 문자열 연산

SQL은 **문자열을 작은따옴표**(' ')로 표기한다. 예를 들어, **'Computer'**와 같은 방식이다.

문자열 안에 작은따옴표를 포함시키려면 **작은따옴표 두 개**를 사용해야한다. 

<br/>

예를 들어, 문자열 "It's right"를 다음과 같이 작성할 수 있다.

```sql
'It''s right'
```

<br/>

SQL 표준은 문자열 비교 시 **대소문자를 구별**한다. 

<br/>

예를 들어, 아래의 연산의 결과는 **거짓**이 된다.

```sql
'comp.sci' = 'Comp.Sci'
```

> 일부 DBMS(MySQL, SQL Server 등)는 기본적으로 대소문자를 구별하지 않는다.
>
> 대소문자를 구분하려면 설정을 변경할 수 있다.

<br/>

<br/>

SQL은 문자열에 대한 다양한 함수를 제공한다.

- **문자열 합병**: `||` 사용 
- **부분 문자열 추출** 
- **문자열의 길이 찾기** 
- **대소문자 변환**: `upper(s)` 혹은 `lower(s)` (s는 문자열) 
- **문자열 끝의 공백 제거**: `trim(s)`

> 서로 다른 데이터베이스 시스템에서 제공되는 문자열 함수는 다양하다.

<br/>

<br/>

### like 연산자

문자열에서 `like` 연산자를 사용하여 패턴 일치를 수행할 수 있다.

패턴은 다음과 같은 특수 문자를 사용한다.

- `%` : 어떠한 부분 문자열과도 일치
- `_` : 어떠한 문자와도 일치 

패턴은 대소 문자를 구분한다. 

> 일부 데이터베이스는 대소 문자를 구별하지 않는 like 연산의 변종을 제공한다.

#### 예제

- `'Intro%'` :  "Intro"로 시작하는 문자열과 일치
- `'%Comp%'` : "Comp"를 포함하는 문자열과 일치
  - 예 : 'Intro. to Computer Science', 'Computational Biology'
- `'_ _ _'` : 정확히 세 개의 문자로 이루어진 문자열과 일치
- `'_ _ _%'` : 세 문자 이상으로 이루어진 문자열과 일치

<br/>

<br/>

SQL은 패턴을 `like` 비교 연산자를 사용하여 표현한다.

<br/>

예를들어, "건물 이름에 'Waston'이라는 부분 문자열을 포함하는 모든 학과 이름을 구하라."라는 질의

```sql
select dept_name
from department
where building like '%Waston%';
```

<br/>

<br/>

#### like 비교에서 escape 키워드

패턴에서 (`%`와 `_`와 같은) **특수 패턴 문자**를 포함할 수 있도록 SQL은 escape문자에 관한 명시를 하고 있다.

escape 문자는 **특수 패턴 문자가 일반 문자처럼 사용되기를 원할 때 특수 패턴 문자 바로 앞에 사용**한다.

like 비교에서 escape 키워드를 사용하여 escape문자열을 정의한다.

<br/>

`역슬래시(\)` 문자를 이스케이프 문자로 사용하는 패턴 예시

- `like 'ab\%cd%' escape '\'`는  "ab%cd"로 시작하는 모든 문자열과 일치한다.
- `like 'ab\\cd%' escape '\'`는 "ab\\cd"로 시작하는 모든 문자열과 일치한다.

<br/>

<br/>

### not like 비교 연산자

SQL은 `not like` 비교 연산자를 사용하여 일치하지 않는 문자열에 대해 검색을 할 수도 있다.

<br/>

예를들어, 'Waston'이라는 문자열을 포함하지 않는 모든 학과 이름을 검색하는 질의

```sql
select dept_name
from department
where building not like '%Waston%'
```

<br/>

<br/>

## 3.4.3 Select 절의 속성 지정

별표 `*`는 `select` 절에서 "**모든 속성**"을 가리키기 위해 사용된다.

```sql
select instructor.* 			
from instructor, teaches
where instructor.ID = teaches.ID;
```

<br/>

`instructor.*` 는 **instructor의 모든 속성**을 선택하도록 가리킨다.

`select *` 의 형식을 갖는 `select` 절은 **`from` 절에서 선택된 결과 릴레이션의 모든 속성**을 가리킨다.

<br/>

<br/>

## 3.4.4 튜플 출력의 순서

SQL은 릴레이션에 있는 **튜플의 출력될 순서를 사용자가 제어**할 수 있도록 한다.

`order by` 절은 질의의 결과 **튜플이 정렬된 순서**로 나타나도록 한다.

<br/>

물리학과의 모든 교수를 알파벳 순서로 나열하려면 다음과 같이 질의를 작성한다.

```sql
select name
from instructor
where dpet_name = 'Physics'
order by name;
```

기본적으로, `order by` 절은 **오름차순**으로 항목을 나열한다.

정렬 순서를 명시하기 위해서는

- 내림차순은 `desc`를 
- 오름차순은 `asc`를 명시할 수 있다. 

<br/>

또한 **정렬은 복수의 속성**에 대해 이루어질 수 있다.

<br/>

예를들어, instructor 릴레이션 전체를 **salary에 대해 내림차순**으로 정렬하기를 원한다고 하자 

만약 **여러 교수가 같은 급여를 받는다면(급여가 같은 경우), 교수 이름에 대한 오름차순**으로 이들을 정렬할 수 있다.

이 질의는 SQL로 다음과 같이 표현할 수있다.

```sql
select *
from instructor
order by salay desc, name asc;
```

<br/>

<br/>

## 3.4.5 Where 절의 술어

### between 비교 연산자

SQL은 `where` 절에서 어떤 값보다는 작거나 같고 어떤 값보다는 크거나 같은 값을 간단히 명시하기 위해 `between` 비교 연산자를 제공한다.

급여가 $90,000과 $100,000 사이에 있는 교수들의 이름을 찾기 위해

```sql
select name
from instructor
where salary <= 100000 and salary >= 90000;
```

위와 같은 질의문을 사용하는 대신에 between 비교를 사용해서 아래와 같은 SQL 질의를 작성할 수 있다.

<br/>

```sql
select name
from instructor
where salary between 90000 and 100000;
```

<br/>

<br/>

### 행 생성자

SQL에서는 `n개의 값 v1, v2, ..., vn`을 가지는 **n차 튜플**을 나타내기 위해 `(v1, v2, ..., vn)`와 같은 표현이 가능하다.

이를 **행 생성자**(row constructor)라고 한다.

>  예를 들어 id, name, age라는 세 개의 열이 있는 경우에서 한 행이 1, 'Bob', 30 인경우 3차 튜플 (1,'Bob',30)으로 나타낼 수 있다.

<br/>

비교 연산자는 **튜플**에서 사용될 수 있고, **순서는 사전 순서**대로 정의된다.

예를 들어, `a1<b1 and a2 <=b2`이면 `(a1,a2) <= (b1,b2)`는 참이다. 

<br/>

유사하게, 두 튜플의 **속성**이 같으면 두 튜플은 같다. 

그래서 앞의 질의는

```sql
select name, course_id
from instructor, teaches
where instructor.ID = teaches.ID and dept_name = 'Biology';
```

다음과 같이 다시 작성될 수 있다.

```sql
select name, course_id
from isntructor, teaches
where (instructor.ID,dept_name) = (teaches.ID, 'Biology');
```

<br/>

<br/>

<br/>

## 3.5 집합 연산

![그림 2 6 section 릴레이션](https://github.com/user-attachments/assets/f7ccbe13-d02c-42f8-bd48-5a81e8c750a4)

SQL 연산 `union`, `intersect`, `except`는 릴레이션에 대해 각각 관계 대수 연산 `⋃`, `⋂`, `-`와 같은 연산을 수행한다.

두 집합에 대해 `union`, `intersect`, `except` 연산을 수행하는 질의를 만들어 보겠다.

<br/>

#### 2017년 가을에 가르쳤던 모든 과목의 집합

```sql
select course_id
from section
where semester = 'Fall' and year = 2017;
```

위 질의 결과 **c1**

![그림 3 8 2017년 가을에 가르쳤던 과목을 나열한 c1 릴레이션](https://github.com/user-attachments/assets/8b682214-3905-4b41-9a2f-8da4cf72c2f6)



<br/>

#### 2018년 봄에 가르쳤던 모든 과목의 집합

```sql
select course_id
from section
where semester = 'Spring' and year = 2018;
```

위 질의의 결과 **c2**

- 2018년 봄에 CS-319 과목이 두 분반으로 나누어 개설되었으므로, c2는 course_id가 CS-319인 두 개의 튜플을 가지게 된다.

![그림 3 9 2018년 봄에 가르쳤던 과목을 나열한 c2 릴레이션](https://github.com/user-attachments/assets/909e0b62-da9d-4995-b8d3-fd5b96fe046a)

<br/>
<br/>

## 3.5.1 합집합 연산(Union Operation)

**2017년 가을과 2018년 봄 학기 중 최소 하나의 학기에 개설된 모든 과목을 찾는 질의**

```sql
(select course_id
 from section
 where semester = 'Fall' and year = 2017)
 union
(select course_id
 from section
 where semester = 'Spring' and year = 2018;)
```

<br/>

위의 각 `select-from-where`문 주위에 포함된 괄호는 선택 사항이나 쉽게 이해하는데 도움이 된다.

> 일부 DB는 괄호 사용을 허용하지 않는데 이 경우 괄호는 없어도 된다.

<br/>

`union` 연산은 `select` 절과는 다르게 자동으로 중복을 제거한다.

- CS-101과 CS-319는 결과에 한 번만 나타난다.

따라서 **c1 union c2**의 결과 릴레이션은 다음과 같다.

![그림 3 10 c1 union c2의 결과 릴레이션](https://github.com/user-attachments/assets/b55ca783-05e6-4f03-86a7-99278d9fb300)

<br/>

만약 모든 중복을 보유하고 싶다면 `union` 대신에 `union all`을 써야 한다.

```sql
(select course_id
 from section
 where semester = 'Fall' and year = 2017)
 union all
(select course_id
 from section
 where semester = 'Spring' and year = 2018;)
```

`union all`을 사용했기에 위 질의의 경우, CS-319와 CS-191은 각각 두 번씩 나타난다.

<br/>

<br/>

## 3.5.2 교집합 연산(Intersect Operation)

**2017년 가을과 2018년 봄 학기 모두 개설된 수업을 찾는 질의**

```sql
(select course_id
 from section
 where semester = 'Fall' and year = 2017)
 intersect
(select course_id
 from section
 where semester = 'Spring' and year = 2018;)
```

<br/>

`intersect` 연산은 자동으로 중복을 제거한다.

위 질의에 대한 결과인 **c1 intersect c2**의 결과 릴레이션은 다음과 같다.

![그림 3 11 c1 intersect c2의 결과 릴레이션](https://github.com/user-attachments/assets/4d0e7c76-7873-4be2-826a-4c4886a4034f)

<br/>

만약 모든 중복을 보유하고 싶으면 `intersect` 대신에 `intersect all`이라고 써야 한다.

```sql
(select course_id
 from section
 where semester = 'Fall' and year = 2017)
 intersect all
(select course_id
 from section
 where semester = 'Spring' and year = 2018;)
```

위 질의 결과의 중복 튜플의 개수는 **c1과 c2 둘 다에서 나타나는 중복 튜플 개수의 최솟값**과 같다.

**예시**

- ECE-101의 **네 개의 분반**이 2017년 가을 학기에 개설되고

  ECE-101의 **두 개의 분반**이 2018년 봄 학기에 개설되면

  결과에는 ECE-101의 **두 개의 튜플**이 나타난다.

<br/>

> MySQL은 intersect 연산을 구현하지 않는다. 대신 한 가지 해결책은 3.8.1절에 논의될 하위 질의를 사용하는 것이다.

<br/>

<br/>

## 3.5.3 차집합 연산

**2017년 가을학기에만 개설되고 2018년 봄 학기에는 개설되지 않은 수업을 찾는 질의**

```sql
(select course_id
 from section
 where semester = 'Fall' and year = 2017)
 except
(select course_id
 from section
 where semester = 'Spring' and year = 2018;)
```

<br/>

`except` 연산은 첫 번째 입력 릴레이션의 모든 튜플 중에 두 번째 입력 릴레이션에 없는 모든 튜플을 출력한다.(즉, 차집합 연산)

`except`연산을 수행하기 전에 자동으로 입력의 중복이 제거된다.

**예시**

- ECE-101의 **네 개의 분반**이 2017년 가을학기에 개설되었고

   ECE-101의 **두 개의 분반이** 2018년 봄 학기에 개설되었다면

   `except` 연산의 결과는(**입력 중복이 제거**되므로) ECE-101의 **어떤 복사본도 갖지 않게 된다**.

<br/>

위 질의에 대한 결과인 **c1 except c2**의 결과 릴레이션은 다음과 같다.

![그림 3 12 except c2의 결과 릴레이션](https://github.com/user-attachments/assets/b550399f-feda-44fc-9703-d9a7d7163ac8)

<br/>

만약 모든 중복을 보유하고 싶다면 `except` 대신 `except all`이라고 써야 한다.

```sql
(select course_id
 from section
 where semester = 'Fall' and year = 2017)
 except all
(select course_id
 from section
 where semester = 'Spring' and year = 2018;)
```

위 질의 결과의 중복 튜플 개수는 **c1에서 중복된 개수에서 c2에서 중복된 개수를 뺀 것에서 그 차가 양수 일때와 같다**.

**예시**

- 2017년 가을학기에 ECE-101의 **네 개의 분반**을 가르쳤고

   2018년 봄 학기에 ECE-101 **두 개의 분반**을 가르쳤다면 

  결과에는 ECE-101의 **두 개의 튜플**이 존재하게 된다.

<br/>

- 그러나 ECE-101의 **두 개 혹은 더 적은 분반**이 2017년 가을에 있었고 

  ECE-101의 **두 개의 분반**이 2018년 봄 학기에 있었다면

  결과에는 ECE-101의 **아무 튜플이 존재하지 않게** 된다.

<br/>

> 일부 DBMS, 특히 Oracle은 except 대신 키워드 minus를 사용하는 반면,
>
> Oracle 12c는 except all 대신 multiset except를 사용한다.
>
> MySQL은 이를 전혀 구현하지 않는다.
>
> 한 가지 해결 방법은 역시 3.8.1절에 논의될 하의 질의를 사용하는 것이다.

<br/>

<br/>

<br/>

## 3.6 널 값

산술, 비교, 집합 연산을 포함한 관계 연산을 수행할 때 **널 값**(null value)이 있으면 특별한 처리가 필요하다.

<br/>

### 산술 연산에서 null 값

산술 연산식(`+`, `-`, `*`, `/`)의 결과는 입력 값이 널이면 널이다.

<br/>

**예시**

- r.A가 특정 튜플에 대해 널일 때 그 튜플에 대한 연산식의 결과는 반드시 널이어야 한다.

```sql
select r.A + 5
from r;
```

<br/>

<br/>

### 비교 연산에서 null값 

널을 포함하는 비교는 좀 더 복잡하다.

예를 들어, 

- `1<null` : 널 값이 무엇을 나타내는지 모르기 때문에 이 비교 값의 결과를 알 수 없으니 **unknown**으로 처리된다. 
-  `not(1<null)` : 이 역시 **unknown**으로 처리

<br/>

SQL은 **비교 연산에서 null 값을 포함할 경우** 그 결과를 true 또는 false가 아닌 **unknown**으로 **처리**한다. 

**unknown**은 제3의 논리 값이다.

> null : 알려지지 않거나 존재하지 않은 값을 나타내는 특수한 값
>
> unknown : 비교 연산의 결과가 불확실함을 나타내는 상태

<br/>

<br/>

### Boolean 연산에서 null 값

where 절의 술어는 비교의 결과에 대한 `and`, `or` , `not`과 같은 Boolean 연산을 포함하기 때문에, 

다음과 같이 Boolean연산의 정의는 **unknown** 값을 다룰 수 있도록 확장되어야 한다.

- **and 연산**
  - `true and unknown`의 결과는 **unknown**
  - `false and unknown`의 결과는 **false**
  - `unknown and unknown`의 결과는 **unknown**
- **or 연산**
  - `true or unknown`의 결과는 **true**
  - `false or unknown`의 결과는 **unknown**
  - `unknown or unknown`의 결과는 **unknown**
- **not 연산**
  - `not unknown`의 결과는 **unknown**이다.


<br/>

where 절의 술어가 어떤 튜플에 대해 **false나 unknown으로 판명**되면 그 튜플은 **결과에 포함되지 않는다**.

<br/>

<br/>

### null값 여부 확인

SQL에서는 null 값인지 아닌지 확인하기 위해 `is null`과 `is not null`을 사용한다. 

일반적인 비교 연산자(`=` 또는 `!=`)는 null 값 비교에 사용할 수 없다.

> 왜냐하면 `null`은 어떤 값과도 비교할 수 없고, 비교의 결과는 **unknown**이 되기 때문이다.

<br/>

`is null` 술어는 적용된 값이 **널일 때 참**이다.

아래 질의는 instructor 릴레이션에서 salary가 null 값인 모든 교수를 찾는다.

```sql
select name
from instructor
where salay is null;
```

<br/>

`is not null` 술어는 적용된 값이 **널이 아닐 때 참**이다.

아래 질의는 instructor 릴레이션에서 salary가 null이 아닌 모든 교수를 찾는다.

```sql
select name
from instructor
where salay is not null;
```

<br/>

<br/>

### 비교 결과의 unknown 여부 확인

SQL은 비교의 결과가(true나 false라기보다) **unknown**인지를 테스트 하기위해 `is unknown`이나 `is not unknown`절을 사용할 수 있다.

> Oracle, MySQL 등 일부 데이터베이스에서는 지원되지 않는다.

**예시**

```sql
select name
from instructor
where salay > 10000 is unknown -- salay > 10000이라는 비교연산 수행 후 그 결과가 unknown 인지를 확인하는 술어
```

<br/>

<br/>

### `select distinct`에서 null 값

질의가 `select distinct` 절을 사용할 때, 중복된 튜플은 반드시 제거되어야한다.

이러한 목적으로, 두 튜플의 **상응하는 속성**값을 비교할 때 값이 둘 다 널이 아닌 같은 값을 가지거나 둘 다 널일 때 **값이 같은 것으로 간주**한다.

- 예를 들어, 두 튜플이 `{ ('A', NULL), ('A', NULL) }`일 경우, **두 튜플은 동일한 것으로 간주**되어 하나의 튜플만 결과 `{ ('A', NULL) }`로 남는다.

<br/>

즉, `distinct`는 `null = null` 비교를 **true**로 간주하여 중복을 제거한다. 

이는 SQL에서 일반적으로 `null = null`의 결과가 **unknown**인 것과 다른 처리 방식이다.

<br/>

<br/>

### 집합 연산에서 null 값

**일부 값이 널이라도 모든 속성값이 같으면 튜플을 같은 것**으로 보는 방법은 합집합(union), 교집합(intersect), 차집합(except) 연산에서도 사용된다.

<br/>

<br/>

<br/>

## 3.7 집계 함수

집계 함수(aggregate function)는 **입력으로 값의 모음**(collection)(즉 집합 혹은 다중 집합)을 가지며, **결과 값으로 단일 값을 반환**하는 함수다.

SQL은 다섯 개의 표준 내장 집계 함수를 제공한다.

> 대부분의 DBMS는 부가적으로 많은 집계 함수를 제공한다.

- 평균 : **avg**
- 최솟값 : **min**
- 최댓값 : **max**
- 총합 : **sum**
- 개수 : **count**

**sum**과 **avg**의 입력은 **숫자의 모음**이어야 하지만, 다른 연산자는 문자열과 같이 숫자가 아닌 데이터 타입 모음에서도 동작한다.

<br/>

<br/>

## 3.7.1 기본 집계(Basic aggregation)

"컴퓨터 과학과 교수들의 평균 급여를 구하라."는 질의

```sql
select avg(salary)
from instructor
where dept_name = 'Comp.Sci.';
```

- 질의의 결과 : 컴퓨터 과학과 교수들의 **평균 급여를 나타내는 숫자 값에 해당하는 단일 속성**의 **단일 튜플**(하나의 행)을 갖는 릴레이션

- 집계된 결과 속성의 이름은 DB 시스템에서 자동 부여.

<br/>

**속성에 의미 있는 이름을 부여**

```sql
select avg (salary) as avg_salary
from instructor
where dept_name = 'Comp. Sci.';
```

- `AS`절을 사용해 속성에 의미 있는 이름 부여 가능하다

<br/>

**중복을 포함 해야하는 경우**

급여: $75,000, $65,000, $92,000 → 평균: $77,333.33

$75,000 교수 추가 시:

- **중복 제거 시**: $76,750 (정확한 값)
- **중복 제거 안할 시**: $58,000 (잘못된 값)

<br/>

**집계 함수를 구하기 전에 중복을 제거해야 하는 경우**

- 키워드 `distinct`를 집계 함수 표현에 사용하여 중복 제거 

예 : "2018년 봄 학기에 어떤 과목을 가르치는 교수의 수를 구하라."는 질의 

```sql
select count(distinct ID)
from teaches
where semester = 'Spring' and year = 2018;
```

- `distinct`로 교수가 여러 과목을 가르치더라도 **한 번만** 세도록 처리됨

<br/>

**튜플 개수 세기**

`count(*)` 사용하여 릴레이션 내 튜플 수를 셀 수 있다.

예 : "course 릴레이션에서 튜플의 개수를 구하라" 질의

```sql
select count(*)
from course;
```

<br/>

SQL은 `count(*)`에서 distinct를 사용하는 것을 금하고 있다. 

`max`와 `min`에서는 `distinct`를 사용하는 것이 가능하지만, 그 결과는 변하지 않는다. 

중복 보유를 명시하기 위해 distinct 대신 키워드 all을 사용할 수 있지만, all은 기본값이기 때문에 그렇게까지 할 필요는 없다.

<br/>

<br/>

## 3.7.2 그룹단위 집계

**복수의 튜플 집합**에 대해 집계 함수를 적용하고 싶을 때 `group by` 절을 사용한다.

- `group by` 절에 주어진 속성(들)은 **그룹을 형성하기 위해 사용**된다. 
- `group by` 절에서 **모든 속성이 같은 값을 가지는 튜플들은 하나의 그룹**으로 묶인다.

<br/>

"각 학과의 평균 급여를 구하라."는 질의를 고려해 보자

```sql
select dept_name, avg(salary) as avg_salary
from instructor
group by dept_name;
```

아래 그림은 질의 결과를 계산하는 첫 번째 단계로, **dept_name 속성으로 그룹화한 instructor 릴레이션의 튜플**을 보여 준다. 

![그림 3 13 dept_name 속성으로 그룹화된 instructor 릴레이션의 튜플](https://github.com/user-attachments/assets/26c93985-a25e-441b-8362-79a57f456bae)

이같이 만들어진 **각 그룹에 대해 집계 연산**을 수행하여 얻은 아래와 같다.

![그림 3 14 각 학과의 평균 급여를 구하라는 질의에 관한 결과 릴레이션](https://github.com/user-attachments/assets/85589828-fe6d-4305-8f64-cebbae44d130)

<br/>

대조적으로, "모든 교수의 평균 급여를 구하라."는 질의를 고려해 보자.

```sql
select avg (salary)
from instructor;
```

이 경우에 `group by` 절은 제외되므로 **전체 릴레이션을 하나의 그룹으로 간주**한다.

<br/>

<br/>

#### 조인과 그룹화를 활용한 집계

"2018년 봄 학기에 각 학과에서 어떤 과목을 가르친 교수의 수를 구하라."는 질의를 고려해 보자. 

```sql
/*어떤 교수가 어떤 과목 분반을 어느 학기에 했는지에 대한 정보는 teaches 릴레이션에 있다. 
  그러나 이 정보는 각 교수가 소속된 학과명을 알아내기 위해 instructor 릴레이션과 조인되어야 한다. */

select dept_name, count (distinct ID) as instr_count
from instructor, teaches
where instructor.ID= teaches.ID and semester = 'Spring' and year = 2018
group by dept_name;
```

질의 결과는 그림 3.15와 같다.

![그림 3 15 2018년 봄 학기에 각 학과에서 어떤 과목을 가르친 교수의 수를 구하라는 질의의 결과 릴레이션](https://github.com/user-attachments/assets/02d84882-7bb6-4cca-a218-108d2d0b98b5)

<br/>

#### select 절에 나타날 수 있는 속성 규칙

`select` 절에 있는 속성은 `group by` 절에 있거나 집계 함수에 속해야 한다.

예시 : ID가 `group by` 절에 나타나지 않고, `select`절에서 집계되지 않은 상태로 나타났기에 오류 발생

```sql
/* erroneous query */
select dept_name, ID, avg (salary)
from instructor
group by dept_name;
```

- 위 질의에서 (dept_name에 따라 정의된) 특정 그룹의 각 교수는 각기 다른 ID를 가질 수 있고,  각 그룹에 대해 하나의 튜플만 출력되기 때문에 출력해야 할 ID 값을 선택하는 특별한 방법은 없다.
- 결과적으로 이러한 경우는 SQL에서 허용되지 않는다.

<br/>

#### 주석(comment)

SQL에서 주석을 달 때는 `/* */` 또는 `--`를 사용 한다.

```sql
/* erroneous query */
-- erroneous query 
```

<br/>

<br/>

## 3.7.3 Having 절

`having` 절

- 튜플보다 **그룹에 대한 조건**을 적용하기위해 사용한다.
- **집계 함수**와 함께 사용되어 **그룹 단위**의 필터링이 가능하다.
- `select` 절에서의 경우와 같이 `having` 절에 **집계 함수**와 함께 사용되지 않은 속성들은 반드시 `group by` 절에 나타나야 한다.

<br/>

교수들의 **평균 급여**가 **$42,000**을 넘는 학과를 찾고 싶다면, `group by` 절이 만든 각 **그룹(학과별 그룹)에 대해 조건**을 적용해야 한다.

```sql
select dept_name, avg(salary) as avg
from instructor
group by dept_name
having avg(salary) > 42000;
```

<br/>

이 결과는 아래 그림이다.

![그림 3 16 평균 급여가 42000보다 많은 학과의 교수들의 평균 급여를 구하라의 결과](https://github.com/user-attachments/assets/689dab25-c1a2-41e7-b0e6-593efa97af23)



<br/>

<br/>

#### 집계 함수, group by, having 절을 포함하는 질의의 의미는 다음과 같은 연산의 순서로 정의된다.

1. `from` 절은 릴레이션을 얻기 위해 먼저 수행된다.

<br/>

2. `where` 절이 존재하면, `where` 절의 술어는 `from` 절의 결과 릴레이션에 적용된다.

<br/>

3. `where` 절의 술어를 만족하는 튜플은 `group by` 절이 존재하면 `group by` 절에 의해 그룹화된다.
   `group by` 절이 없다면, `where` 절의 술어를 만족하는 **전체 튜플 집합은 하나의 그룹으로 간주**된다.

<br/>

4. `having` 절이 존재한다면 각각의 그룹에 적용된다. `having` 절의 술어를 만족하지 못하는 그룹은 제거된다.

<br/>

5. `select` 절은 남아 있는 그룹을 사용하여 질의의 결과 튜플을 생성하는데, 이때 **그룹별로 집계 연산을 적용**하여 단일 결과 항을 만든다.

<br/>

<br/>

#### having 절과 where 절을 함께 사용하는 SQL 질의 

"2017년에 개설한 각 과목 분반에 대해서, 그 분반에 적어도 두 명의 학생이 있으면 해당 분반에 등록한 학생들이 이수한 총 학점의 평균(tot_cred)을 구하라."는 질의를 고려해 보자.

`takes(ID, course_id, sec_id, semester, year, grade)`

`student(ID, name, dept_name, tot_cred)`

<br/>

```sql
select course_id, semester, year, sec_id, avg (tot_cred)
from student, takes
where student.ID= takes.ID and year = 2017
group by course_id, semester, year, sec_id
having count (ID) >=2;
```

<br/>

<br/>

## 3.7.4 널 값과 불리언 값의 집계

널 값이 존재할 때 널 값은 집계 함수의 처리를 복잡하게 한다. 

예를 들면, instructor 릴레이션의 몇몇 튜플에 salary에 대해 널 값을 가질 때 모든 급여의 총합을 구하는 다음 질의를 고려해 보자.

```sql
select sum(salary)
from instructor;
```

위 질의에서 합해지는 값은 일부 튜플이 salary에 대해 널 값을 가지고 있으므로 널 값을 포함하다.

전체 합이 null이라고 말하기보다는, SQL 표준은 `sum` 연산은 입력의 **null 값을 무시**한다고 말한다.

<br/>

#### 집계 함수에서 널 값 처리 규칙

`count(*)`를 제외한 모든 집계 함수는 입력 값에서 널 값을 무시한다. 

널 값을 무시한 결과로, 값의 모음은 비어 있을 수도 있다. 

- 빈 모음 처리
  - `count`는 0으로 정의되고
  - 다른 모든 집계 연산은 빈 모음이 적용되었을 때 **널 값을 반환**한다. 

<br/>

SQL:1999에서 도입된 Boolean 데이터 타입은 `true`, `false`, `unknown` 값을 가질 수 있다.

`SOME(ANY)`과 `EVERY(ALL)`와 같은 집계 함수는 불리언 값의 모음에 적용될 수 있다

- some : 이접 (Disjunction): `or` 연산
- every : 연접 (Conjunction): `and` 연산

<br/>

<br/>

<br/>

## 3.8 중첩 서브 쿼리(Nested Subqueries)

SQL은 하위 질의를 중첩시키는 방법을 제공한다. 하위 질의는 다른 질의 안에 중첩된 `select-from-where` 표현이다.

<br/>

하위 질의의 일반적인 용도

- `where` 절에서 하위 질의를 중첩함으로써 집합의 멤버십을 테스트

  > membership : 특정 값이 주어진 집합에 포함되어 있는지를 나타내는 개념

- 집합 비교

- 집합의 특정 조건을 만족하는 원소 개수를 결정

<br/>

<br/>

## 3.8.1 집합의 멤버십

SQL에서 `in`과 `not in` 접속사는 집합 멤버십을 테스트하는데 사용된다.

이는 `select` 절에 의해 생성된 값들의 집합에 대한 멤버십을 확인하거나 부재를 테스트하는 방식이다.

<br/>

### 단일 속성 멤버십 테스트

#### in 절을 사용한 질의

예 : "2017년 가을 학기와 2018년 봄 학기에 둘 다 있는 과목을 구하라."는 질의

```sql
select distinct course_id
from section
where semester = 'Fall' and year = 2017 and
course_id in (select course_id 
              from section
              where semester = 'Spring' and year = 2018)
```

- `in` 절을 사용한 방법은 두 개의 집합의 **교집합**(intersect)을 구하는 방식과 같은 결과를 낸다.
- `intersect` 연산은 기본적으로 중복을 제거하기 때문에 여기서 `distinct`를 사용해야 한다.

<br/>

#### not in 절을 사용한 질의

반대로, **not in**을 사용하면 "2017년 가을 학기에는 있지만 2018년 봄 학기에는 없는 과목"을 구할 수 있다.

```sql
select distinct course_id
from section
where semester = 'Fall' and year= 2017 and
course_id not in (select course_id
                  from section
                  where semester = 'Spring' and year= 2018);
```

- `not in`절을 사용한 방법은 두 개의 집합의 **차집합**(except)을 구하는 방식과 같은 결과를 낸다.

<br/>

#### 열거형 집합에서 in/not in 사용

`in`과 `not in` 연산자는 **열거형 집합**(enumerated set)에서 사용할 수 있다. 

다음은 이름이 "Mozart"도 "Einstein"도 아닌 교수를 선택하는 질의다.

```sql
select distinct name
from instructor
where name not in ('Mozart', 'Einstein');
```

<br/>

### 여러 속성 멤버십 테스트

<br/>

SQL에서 임의의 릴레이션에서 멤버십을 테스트하는 것도 가능하다. (여러 속성을 가진 릴레이션에서도 사용 가능)

예를 들어, "ID 110011의 교수가 가르치는 과목 분반을 수강하는 학생 수"를 구하는 질의는 다음과 같이 작성할 수 있다.

```sql
select count (distinct ID)
from takes
where (course_id, sec_id, semester, year) in (select course_id. sec_id, semester, year
                                              from teaches
                                              where teaches.ID = '10101'):
```

> 일부 SQL 구현은 위에서 사용된 행 구성 구문인 "(course_id. sec_id, semester, year)"를 지원하지 않는다.
>
> > Oracle 완벽하게 지원, MySQL 최신 버전(5.7이상)에서는 잘 지원
>
> 3.8.3절에서 이 질의를 작성하는 다른 방법을 살펴볼 것이다.

<br/>

<br/>

## 3.8.2 집합 비교

| SOME    | 설명                                                 |
| ------- | ---------------------------------------------------- |
| = SOME  | 하나라도 만족하는 값이 있으면 결과를 리턴(IN과 동일) |
| > SOME  | 값들 중 최소값 보다 크면 결과를 리턴                 |
| >= SOME | 값들 중 최소값 보다 크거나 같으면 결과를 리턴        |
| < SOME  | 값들 중 최대값 보다 작으면 결과를 리턴               |
| <= SOME | 값들 중 최대값 보다 작거나 같으면 결과를 리턴        |
| <> SOME | 적어도 하나의 값과 다르면 결과를 리턴                |

<br/>

<br/>

| ALL    | 설명                                          |
| ------ | --------------------------------------------- |
| = ALL  | 모든 값들이 같으면 결과를 리턴                |
| > ALL  | 값들 중 최댓값보다 크면 결과를 리턴           |
| >= ALL | 값들 중 최대값 보다 크거나 같으면 결과를 리턴 |
| < ALL  | 값들 중 최소값 보다 작으면 결과를 리턴        |
| <= ALL | 값들 중 최소값 보다 작거나 같으면 결과를 리턴 |
| <> ALL | 모든 값들과 다르면 결과를 리턴(NOT IN과 같음) |

<br/>

<br/>

### 예제

### 1. 생물학과의 적어도 한 교수보다 급여가 많은 모든 교수의 이름을 구하라는 질의

```sql
select distinct T.name
from instructor as T, instructor as S
where T.salary > S.salary and S.dept_name = 'Biology';
```

<br/>

위와 같이 작성해도 되지만 아래와 같이 작성할 수 있다.

```sql
select name
from instructor
where salary > some(select salary
                    from instructor
                    where dept_name = 'Biology');
```

- "**하나 이상보다 큰**"은 SQL에서 `> some`으로 표현될 수 있다. 
- `> some` 비교는 생물학과에 있는 교수들의 모든 급여 값의 집합에서 적어도 한 원소의 값보다는 큰 salary 값을 갖는 튜플에 대해서 참이다.

> 키워드 any는 SQL에서 some과 동의어다.

<br/>

<br/>

### 2. 생물학과의 모든 교수보다 급여가 많은 교수의 이름을 찾는 질의

```sql
select name
from instrucotr
where salary > all(select salary
                   from instructor
                   where dept_name = 'Biology');
```

- `> all`은 **"모든 것보다 큰"**을 의미하며, 생물학과의 모든 교수 급여보다 큰 교수의 이름을 반환한다. 

<br/>

<br/>

### 3. 가장 높은 평균 급여를 받는 학과를 구하라는 질의를 보자. 

먼저 학과별 평균 급여를 구하는 질의를 작성하고

다른 모든 평균 급여보다 크거나 같은 평균 급여를 가지는 학과를 찾는 하위 질의를 중첩한다.

```sql
select dept_name
from instructor
group by dept_name
having avg(salary) >= all(select avg(salary)
                          from instructor
                          group by dept_name);
```

<br/>

<br/>

## 3.8.3 빈 릴레이션에 대한 테스트

SQL은 **하위 질의가 그것의 결과로 튜플을 가지는지 아닌지**를 테스트하는 특성이 있다. 

<br/>

### `exists` 구문

`exists` 구문은 인자의 하위 질의가 비어 있지 않을 때 `true`를 반환한다. (즉, 행이 존재하는지 여부를 확인)

 exists 구문을 사용해서 "2017년 가을 학기와 2018년 봄 학기 둘 다 있는 과목을 구하라."는 질의 역시 다른 방법으로 작성할 수 있다.

```sql
-- section(course_id, sec_id, semster, year, building, room_number, time_slot_id)

select course_id
from section as S
where semester = 'Fall' and year= 2017 and
	exists (select *
            from section as T
            where semester = 'Spring' and year= 2018 and
            S.course_id = T.course_id);
            
/* 실행 순서
	1. 먼저 바깥 쿼리가 section 테이블에서 2017년 가을 학기에 해당하는 course_id들을 가져온다.
	2. 각 course_id에 대해, 하위 질의가 2018년 봄 학기에 동일한 과목이 있는지 확인한다.
	3. 하위 질의가 결과를 반환하면 EXISTS는 TRUE를 반환하고, 해당 과목이 최종 결과에 포함된다.
	4. 이 과정을 반복해서 최종적으로 2017년 가을 학기와 2018년 봄 학기 모두에 개설된 과목을 찾는다.
*/
```

위 질의는 `where` 절의 하위 질의에서 바깥 질의에 있는 상관 이름을 사용할 수 있는 SQL의 특성을 보여 준다. 

- **상관 하위 질의**(correlated subquery) : 바깥 질의의 상관 이름을 사용하는 하위 질의
- **범위 지정 규칙**(scoping rule) : 하위 질의는 하위 질의 또는 포함하는 질의의 상관 이름만 사용할 수 있으며, 지역적으로 정의된 이름이 우선 적용된다.

<br/>

### `not exists` 구문

`not exists`는 하위 질의의 결과가 없음을 검사할 수 있다. 

`not exists` 구문으로 릴레이션 A는 릴레이션 B를 포함한다.를 `not exists (B except A)`와 같이 작성할 수 있다

<br/>

예를 들어, "생물학과에서 제공하는 모든 과목을 수강하는 모든 학생을 구하라."는 질의를 다음과 같이 작성할 수 있다.

```sql
-- student(ID, name, dept_name, tot_cred)
-- course(course_id, title, dept_name, credits)
-- takes(ID, course_id, sec_id, semester, year, grade)

select S.ID, S.name
from student as S
where not exists ((select course_id
                   from course
                   where dept_name = 'Biology')
                  except
                  (select T.course_id
                   from takes as T
                   where S.ID = T.ID));
```

<br/>

- 첫 번째 하위 질의는 생물학과의 모든 과목을 찾는다.

  ```sql
  (select course_id from course where dept_name = 'Biology');
  ```

- 두 번째 하위 질의는 학생 S.ID가 수강하는 모든 과목을 찾는다.

  ```sql
  (select T.course_id from takes as T where S.ID = T.ID);
  ```

따라서 외부 질의의 select는 각각의 학생을 찾아 해당 학생이 수강하는 모든 과목의 집합이 생물학과에서 개설한 모든 과목의 집합을 포함하는지 테스트한다.

<br/>

### `Exists` 대안 구문

3.8.1절에서 "ID 110011인 교수가 가르친 과목 분반을 수강한 (고유) 학생의 총수를 찾아라."라는 SQL 질의를 보았다. 

```sql
select count (distinct ID)
from takes
where (course_id, sec_id, semester, year) in (select course_id. sec_id, semester, year
                                              from teaches
                                              where teaches.ID = '10101'):
```

<br/>

`exists` 구문을 사용한 다른 대안으로 다음과 같은 질의가 있다.

- `exists` 구문은 해당 학생이 특정 교수가 가르친 과목을 수강했는지 여부를 확인합니다.

```sql
select count (distinct ID)
from takes
where exists (select course_id, sec_id, semester, year
              from teaches
              where teaches.ID = '10101'
              		and takes.course_id = teaches.course_id
              		and takes.sec_id = teaches.sec_id
              		and takes.semester = teaches.semester
              		and takes.year = teaches.year);
```

<br/>

<br/>

## 3.8.5 From 절의 서브 쿼리

SQL은 `from` 절에서 하위 질의 표현을 허용한다.

여기서 적용되는 핵심 개념은 `select-from-where`표현은 릴레이션을 결과로 반환한다는 것이고, 이 개념은 `select-from-where` 표현에서 릴레이션이 나타나는 어디에서나 적용될 수 있다.

<br/>

"평균 급여가 $42,000 이상인 학과의 교수들의 평균 급여를 구하라."는 질의를 고려해 보자.

**이전 방법 : `having` 절 사용**

```sql
select avg(salary) as avg_salary
from instructor
group by dept_name
having salary >= 42000;
```

<br/>

**새로운 방법 : `from` 절의 하위 질의를 사용**

```sql
select dept_name, avg_salary
from (select dept_name, avg(salary) as avg_salary
      from instructor
      group by dept_name)
where avg_salary > 42000;
```

- 하위 질의는 모든 학과 이름과 그들 각각의 평균 급여로 구성된 릴레이션을 생성한다.

- 위 예제에서 볼 수 있듯이, 이 **하위 질의 결과의 속성은 바깥 질의에서 사용**된다.

- `from` 절의 하위 질의가 평균 급여를 계산하고, 이전의 `having` 절에 있었던 술어가 이제는 바깥 질의의 `where`절에 있으므로,

   `having` 절을 사용할 필요가 없어졌다.

<br/>

`as`절을 사용하여 **하위 질의의 결과 릴레이션에 이름**을 붙일 수 있고, 속성을 재명명할 수 있다.

```sql
select dept_name, avg_salary
from(select dept_name, avg(salary)
     from instructor
     group by dept_name)
     as dept_avg(dept_name, avg_salary)
where avg_salary > 42000;
```

- 하위 질의 결과 릴레이션은 속성 dept_name과 avg_salary를 가지는 dept_avg로 이름 지어졌다.

<br/>

`from` 절의 중첩 하위 질의는 모두는 아니지만, 대부분의 SQL 엔진에서 지원된다.

#### SQL 엔진별 특징

● **MySQL, PostgreSQL**

`from` 절에 각 하위 질의 릴레이션 이름을 절대 참조하지 않는다고 하더라도 반드시 해당 릴레이션의 이름을 지정해야 한다.

<br/>

● **Oracle**

하위 질의 결과 릴레이션에 이름(키워드 **`as` 생략**)을 부여할 수 있다.

**릴레이션 속성의 이름 변경은 허용하지 않는다**.

<br/>

하위 질의 `select` 절에서 속성 이름을 바꾸는 것으로 해결 가능 

- 위 질의에서 하위 질의의 select절은 다음으로 대체될 수 있다.

```sql
select dept_name, avg(salary) as avg_salary
```

- 위 질의에서

  ```sql
  as dept_avg(dept_name, avg_salary)
  ```

- 는 아래의 절로 대체될 수 있다.

  ```sql
  as dept_avg
  ```

<br/>

또 다른 예제로, 각 학과의 총급여 최댓값을 찾아보자.

`having` 절로는 이 질의를 처리하기가 쉽지 않지만, `from`절에 하위 질의를 사용함으로써 이 질의를 다음과 같이 쉽게 작성할 수 있다.

```sql
-- 최댓값 구하기 위해서는 dept_name별로 total_salary의 릴레이션을 만든 후 max(tot_salary)

select max(tot_salary)
from(select dept_name, sum(salary) as tot_salary
     from instructor
     group by dept_name);
```

<br/>

`from` 절의 중첩된 하위 질의는 외부 쿼리의 변수를 참조할 수 없다.

그러나 SQL:2003에서는 `lateral` 키워드를 사용하여 외부 쿼리의 변수에 접근할 수 있다.

예를 들면 각 교수의 이름을 그들의 급여와 그들이 소속된 학과의 평균 급여와 함께 출력하고자 하자

```sql
select name, salary, avg_salary
from instructor I1, 
lateral (select avg(salary) as avg_salary
         from instructor I2
         where I2.dept_name = I1.dept_name);
```

- `lateral`을 사용하여 내부 쿼리에서 외부 쿼리의 `I1.dept_name`에 접근

> 보다 최근에 개발된 SQL 엔진만이 lateral 절을 지원한다.

<br/>

<br/>

## 3.8.6 With 절

`with` 절은 **임시 릴레이션**(뷰)을 정의하여 해당 질의 내에서만 사용될 수 있도록 한다. 

이를 통해 **복잡한 서브 쿼리**를 **더 명확하고 논리적으로** 작성할 수 있다. 

이 질의는 **가장 많은 예산을 가진 학과**를 찾는다.

1. 서브 쿼리 방식

```sql
select budget
from department
where budget = (select max(budget)
                from department)
```

2. **with 절**

```sql
with max_budget (value) as 
	(select max(budget)
     from department)
select budget
from department, max_budget
where department.budget = max_budget.value;
```

- 장점
  - **논리적 명확성**: 서브 쿼리 대신 **임시 릴레이션**을 정의해 질의를 구조적으로 구분 가능.
  - **재사용성**: **동일 질의 내**에서 여러 번 사용되는 복잡한 계산을 **한 번 정의**하여 재사용 가능.

<br/>

**학과의 총급여가 평균보다 큰 학과를 구한다**고 가정해보자.

이 질의를 `with` 절을 이용하여 다음과 같이 작성할 수 있다.

```sql
with dept_total (dept_name, value) as
	(select dept_name, sum(salary)
     from instructor
     group by dept_name),
dept_total_avg(value) as
	(select avg(value)
     from dept_total)
select dept_name
from dept_total, dept_total_avg
where dept_total.value > dept_total_avg.value;
```

`with` 절 없이도 작성 가능하지만, 더 **복잡**하고 **이해하기 어려움**.

<br/>

<br/>

## 3.8.7 스칼라 서브 쿼리

**스칼라 하위 질의**(scalar subquery)는 **단일 속성을 가진 하나의 값**을 반환하는 하위 질의를 의미한다.

SQL에서 **값이 필요한 연산식**에 삽입될 수 있으며, `select`, `where`, `having` 절 등에서 사용 가능하다.

<br/>

예시 , 모든 학과와 그 학과의 교수의 수를 함께 나열하는 스칼라 하위 질의

```sql
select dept_name, (select count(*)
                   from instructor
                   where department.dept_name = instructor.dept_name)
                  as num_instructors
from department;
```

- **하위 질의**: 이 예제의 하위 질의는 `count(*)` 집계를 사용하여 결과를 반환하는데, `group by`가 없으므로 오직 하나의 값만 반환하는 것이 보장된다.
- **상관 변수**: 예제는 외부 질의의 `FROM` 절에 있는 릴레이션의 속성을 참조하는 상관 변수 사용법을 보여줍니다. 여기서는 `department.dept_name`이 그 예이다.

<br/>

스칼라 하위 질의는 또한 집게 함수 없이 정의될 수 있다.

컴파일 시간에 하위 질의가 결과로 하나 이상의 튜플을 반환할 가능성을 알 수 없는 경우가 있다. 

만약 하위 질의가 실행될 때 결과가 하나 이상의 튜플을 가진다면, 실행 시간에 오류가 발생

<br/>

#### 스칼라 하위 질의의 반환 타입

스칼라 하위 질의가 하나의 튜플을 포함하고 있더라도, 스칼라 하위 질의 결과의 타입은 여전히 **릴레이션**

하지만, 하나의 값이 필요한 표현식에 스칼라 하위 질의가 사용되면, SQL은 암묵적으로 릴레이션 내의 하나의 튜플에서 하나의 속성 값을 추출하여 반환한다.

<br/>

<br/>

## 3.8.8 From 절 없는 스칼라

특정 질의는 계산이 필요하지만, 릴레이션에 대한 참조는 필요하지 않다.

또한, 일부 질의에서는 최상위 질의에 `from` 절이 필요하지 않지만, 하위 질의에 `from` 절이 포함될 수 있다.

<br/>

예를 들어, 여러 교수가 가르친 분반을 교수당 한 번 계산하여, (연도 또는 학기와 관계없이) 교수당 가르친 평균 분반 수를 찾고 싶다고 가정해 보겠다.

1. **가르친 분반의 총 개수**를 찾으려면 `teaches`의 튜플 수를 계산하고,
2. **교수의 수**를 찾기 위해 `instructor`의 튜플 수를 계산해야 한다.

```sql
select count (*) from teaches / (select count(*) from instructor);
```

<br/>

일부 시스템에서 이것이 적법하나 다른 시스템에서는 from 절이 없으므로 오류를 보고할 수 있다.

> 이 구문은 Microsoft SQL Server에는 적합하지만 Oracle에서는 적합하지 않다.

<br/>

#### Oracle의 대안 : dual 릴레이션 사용

Oracle에서 위 질의를 실행하려면 단일 튜플을 포함하는 `dual`이라는 특수한 더미 릴레이션을 사용할 수 있다.

> 이 릴레이션에는 사용 목적과 상관없는 단일 속성이 있다

이를 통해 이전 질의를 다음과 같이 작성할 수 있다

```sql
select (select count(*) from teaches) / (select count(*) from instructor)
from dual
```

<br/>

#### 정밀도 손실 문제

위 질의는 하나의 정수를 다른 정수로 나누기 때문에 대부분 Db에서 결과는 정수과 되어 정밀도가 손실된다.

결과를 부동 소수점 수로 얻으려면 나누기 연산을 수행하기 전에 두 하위 질의 결과 중 하나에 **1.0**을 곱하여 부동 소수점 수로 변환할 수 있다.

<br/>

<br/>

<br/>

## 3.9 데이터베이스의 변경

SQL을 이용하여 정보를 어떻게 삽입,삭제, 변경할 것인지 보겠다.

<br/>

## 3.9.1 삭제

`delete` 요청은 전체 튜플만 삭제할 수 있으며, 특정 속성의 값만 삭제할 수는 없다.

```sql
delete from r
where P; -- where 절은 생략될 수 있다.
```

- r : 삭제할 릴레이션 이름
- P : 술어

`delete` 명령은 한 릴레이션에 대해서만 동작한다. 

여러 개의 릴레이션에서 튜플을 삭제하고 싶다면, 각각의 릴레이션에 대해 하나의 `delete` 명령을 사용해야한다.

<br/>

### SQL 삭제문 예시

#### instructor 릴레이션에서 모든 튜플을 삭제하라

이 명령 수행후 릴레이션 자체는 존재하지만, 데이터는 비어있는 상태

```sql
delete from instructor
```

<br/>

### 재무과의 교수와 관련된 instructor 릴레이션의 모든 튜플을 삭제하라

```sql
delete from instructor
where dept_name = 'Finance';
```

<br/>

### 급여가 $13,000에서 $15,000 사이인 모든 교수를 삭제하라

```sql
delete from instructor
where salary between 13000 and 15000;
```

<br/>

### Waston 건물에 있는 학과와 관련된 instructor 릴레이션의 모든 교수를 삭제하라

`department(dept_name, building, budget)`

```sql
delete from instructor
where dept_name in (select dept_name
                    from department
                    where building = 'Waston');
```

<br/>

### 대학교의 평균보다 낮은 급여를 가진 모든 교수의 레코드를 삭제하라

```sql
delete from instructor
where salary < (select avg(salary)
                from instructor);
```

- instructor 릴레이션의 모든 튜플에 대해 평균 급여보다 낮은 급여를 가진 교수인지를 테스트하고, 그 조건을 만족하는 튜플들을 삭제한다.
- 삭제가 수행되기 전에 모든 튜플에 대해 테스트가 이루어져야 한다. 만일 일부 튜플이 미리 삭제되면 평균 급여가 변경될 수 있어 최종 결과가 달라질 수 있기 때문이다.

<br/>

<br/>

## 3.9.2 삽입

릴레이션에 데이터를 삽입할 때는 **삽입할 튜플을 명시**하거나 **질의를 통해 튜플 집합을 생성**할 수 있다.

<br/>

### 단일 튜플 삽입

컴퓨터 과학과에 "Database Systems"라는 제목의 CS-437 과목을 삽입하는 간단한 예

```sql
insert into course
values ('CS-437', 'Database Systems', 'Comp. Sci.', 4);
```

<br/>

속성 이름을 명시하여 삽입할 수도 있다

```sql
insert into course (course_id, title, dept_name, credits)
values ('CS-437', 'Database Systems', 'Comp. Sci.', 4);
```

<br/>

아래 예처럼 속성 순서는 상관없다

```sql
insert into course (title, course_id, credits, dept_name)
values ('Database Systems', 'CS-437', 4, 'Comp. Sci.');
```

> VALUES 절에 입력할 테이블의 모든 컬럼이 나열되어 있다면 INTO 절의 컬럼은 생략해도 된다.
>
> 하지만 생략 안하는걸 추천

<br/>

<br/>

### 질의를 통한 삽입

질의 결과로 생성된 튜플을 릴레이션에 삽입할 수 있다. 

예시로, 음악학과에 144학점이 넘는 학생을 음악학과의 교수가 되게 하면서 급여가 $18,000이 되도록 한다고 하면 질의는 다음과 같다.

```sql
insert into instructor
	select ID, name, dept_name, 18000
	from student
	where dept_name = 'Music' and tot_cred > 144;
```

SQL은 `select` 문을 먼저 실행하고 나서, 주어진 튜플에 대해 instructor 릴레이션에 삽입한다.

<br/>

#### 무한 삽입 문제

SQL에서 데이터를 삽입할 때, `select` 문이 먼저 **완전히 실행**되고 나서 결과를 다른 테이블에 삽입한다. 

**`select` 문을 모두 수행한 후에 삽입**을 해야지, 그렇지 않으면 예상치 못한 문제가 발생할 수 있다. 

<br/>

 `select` 문이 수행되는 도중에 삽입을 동시에 하게 되면, 무한히 데이터가 중복 삽입될 수 있다. 

<br/>

예를 들어, 아래와 같은 구문이 있을 때

```sql
insert into student
	select *
	from student;
```

student에 대해 주 키 제약이 없다면 위와 같은 요청은 튜플에 대해서 무한히 삽입 작업을 수행하게 될 것이다. 

> 이 요청은 다시 첫 번째 튜플을 student에 삽입하고 튜플의 두번째 사본을 만든다.  이 두 번째 사본은 이제는 student의 일부가 되었으므로 select 문은 이를 다시 발견하게 되고, 세 번째 사본이 student에 삽입될 것이다. 
>
> select 문은 세 번째 사본을 발견하고 네 번째 사본을 삽입하고, 영원히 이를 반복하게 된다. 

<br/>

**해결 방법**

- **주 키 제약 조건**을 설정하여 중복 데이터를 방지하거나, 
- 삽입 전에 `select` 문이 **완전히 실행**되도록 보장하면 이런 문제를 피할 수 있다.

<br/>

### 일부 속성만 삽입

모든 속성 값을 제공하지 않고, 일부 속성만 삽입할 경우 나머지 속성은 `null`로 채워진다:

`student(ID, name, dept_name, tot_cred)`

```sql
insert into student
values ('3003', 'Green', 'Finance', null);
```

이 경우, `tot_cred` 속성은 `null`로 삽입된다.

<br/>

<br/>

### 대량 적제기(bulk loader)

대부분의 관계형 데이터베이스 제품은 릴레이션에 **많은 수의 튜플을 삽입**하기 위해 **대량 적제기**(bulk loader)라는 특별한 유틸리티를 가지고 있다. 

이 유틸리티는 데이터를 정형화된 텍스트 파일로부터 읽어 들이는 것을 허용하며 매번 같은 **일반적인 삽입 명령문의 반복보다 훨씬 빠르게 실행**한다.

<br/>

<br/>

## 3.9.3 갱신

특정 조건에 따라 튜플의 일부 값만 갱신하고 싶을 때 `update` 문을 사용한다.

다양한 예제들을 소개하겠다.

<br/>

### 모든 교수들의 급여를 5% 인상하는 갱신 문

```sql
update instructor
set salary = salary * 1.05;
```

- 이 갱신문은 instructor 릴레이션의 각 튜플에 대해 한 번만 적용된다.

<br/>

### 급여가 $70,000 미만인 교수들만 5% 인상하는 갱신 문

```sql
update instructor
set salary = salary * 1.05
where salary < 70000;
```

<br/>

### "평균보다 적은 급여를 받는 교수들의 급여를 5% 인상하라 갱신 문

```sql
update instructor
set salary = salary * 1.05
where salary < (select avg(salary)
                from instructor);
```

<br/>

### $100,000 이상의 급여를 받는 교수들에게 급여를 3% 인상해 주고 다른 교수들은 5%의 인상 해주는  갱신 문 

다음과 같은 두 개의 `update` 문을 작성할 수 있다.

```sql
update instructor
set salary = salary * 1.03
where salary > 100000;
```

```sql
update instructor
set salary = salary * 1.05
where salary <= 100000;
```

`update` 문의 순서가 중요한데 두 문장의 순서가 바뀐다면, $100,000보다 약간 적은 급여를 받는 교수는 8%의 인상을 받을 수도 있다.

<br/>

SQL의 `case` 구문은 갱신의 순서와 관련된 문제를 없앨 수 있게 단일 `update` 문에서 두 갱신을 모두 수행할 수 있도록 해 준다.

```sql
update instructor
set salary = case
				when salary <= 100000 then salary * 1.05
				else salary * 1.03
			 end
```

<br/>

`case` 문의 일반적인 형태는 다음과 같다.

- 값이 올 수있는 어떤 곳에서도 사용 가능

```sql
/*	이 연산은 pred1, pred2, ... , predn, 중에서 처음으로 만족하는 i에 대해 resulti를 반환한다. 
	만일 어떠한 술어도 만족하지 않는다면 연산은 result0를 반환한다. 
*/

case
	when pred1 then result1
	when pred2 then result2
	...
	when predn then resultn,
	else result0
end
```

<br/>

### 각 student 튜플의 tot_cred 속성을 학생이 이수한 과목의 학점의 합으로 설정하는 갱신 문 

학생의 등급이 'F'나 널 값이 아니면 과목을 이수한 것으로 가정한다. 

이 갱신을 명세하기 위해 아래에서 보는 바와 같이 set 절에 있는 스칼라 하위 질의를 사용해야 한다.

```sql
update student
set tot_cred = (
    select sum(credits)
    from takes, course
    where student.ID = takes.ID and
    		takes.course_id = course.course_id and
    		takes.grade <> 'F' and
    		takes.grade is not null);
```

학생이 어떠한 과목도 성공적으로 이수하지 못했을 경우 위 갱신문은 tot_cred 속성의 값을 널 값으로 설정한다. 

> 이는 `SUM(credits)` 함수가 이수한 과목이 없을 때 널 값을 반환하기 때문이다.

<br/>

대신 값을 0으로 설정하기 위해, 널 값을 0으로 바꾸도록 다른 update 문을 사용할 수 있다. 

```sql
update student
set tot_cred = 0
where tot_cred is null;
```

<br/>

#### 널 값을 처리하는 더 나은 두 가지 방법은 다음과 같다.

1. `case` 표현식

   ```sql
   select case
   		when sum(credits) is not null then sum(credits)
   		else 0
   end
   ```

   - 전체 코드

     ```sql
     update student
     set tot_cred = (
         select case
             when sum(credits) is not null then sum(credits)
     		else 0
         end
         from takes, course
         where student.ID = takes.ID
           and takes.course_id = course.course_id
           and takes.grade <> 'F'
           and takes.grade is not null
     );
     ```

<br/>

2. `coalesce` 함수

   - 널 값을 다른 값으로 대체할 수 있는 더 간결한 방법.

     ```sql
     select coalesce(sum(credits), 0)  -- sum(credits)가 널이 아니면 그 값을 반환하고, 널 값일 경우 0을 반환한다.
     ```

   - 전체 코드

     ```sql
     update student
     set tot_cred = (
         select coalesce(sum(credits),0)
         from takes, course
         where student.ID = takes.ID
           and takes.course_id = course.course_id
           and takes.grade <> 'F'
           and takes.grade is not null
     );
     ```

     

