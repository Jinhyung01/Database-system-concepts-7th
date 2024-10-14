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

     

