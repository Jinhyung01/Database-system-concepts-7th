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
