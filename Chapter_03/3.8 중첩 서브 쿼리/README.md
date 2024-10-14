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