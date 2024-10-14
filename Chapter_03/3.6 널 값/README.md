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
- `not(1<null)` : 이 역시 **unknown**으로 처리

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