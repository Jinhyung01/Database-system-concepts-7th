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