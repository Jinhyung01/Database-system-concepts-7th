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