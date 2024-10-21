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

- **중복 포함 시**: $76,750 (정확한 값)
- **중복 제거 시**: $58,000 (잘못된 값)

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
