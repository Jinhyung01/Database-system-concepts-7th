## 2.3 키

주어진 릴레이션 안에서 튜플을 구별하기 위해서 튜플의 속성값은 그 튜플을 **유일하게 식별**할 수 있어야한다.

<br/>

![KEY](https://github.com/user-attachments/assets/b179971f-685b-4061-a73d-06e759900613)

<br/>

### 수퍼 키(superkey)

수퍼 키란 한 릴레이션에서 **튜플을 유일하게 식별할 수 있도록 해주는(유일성)** 속성 또는 속성들의 집합

<br/>

##### `instructor(ID, name, dept_name, salary)` 릴레이션에서의 예

- `ID`,   `ID + 다른 속성들` : ID가 수퍼키 이므로 ID를 포함한 어떠한 집합도 수퍼키이다.
- ID 속성을 제외한 속성들은 중복될 수 있기 때문에 **단독으로는 수퍼 키가 될 수 없다.**

<br/>

<br/>

### 후보 키(candidate key)

후보 키란 **유일성**과 **최소성**을 만족하는 속성 또는 속성들의 집합

즉, 한 릴레이션에서 **튜플을 유일하게 식별하기 위해 꼭 필요한 최소한의 속성들로만 이루어진다.**

수퍼키 중에서 최소성을 만족하는 것이 후보키가 된다

<br/>

##### `instructor(ID, name, dept_name, salary)` 릴레이션에서의 예

- `ID` : 유일성 + 최소성을 만족하므로 후보키이다.

- 다른 여러 속성들의 집합이 하나의 후보 키 역할을 할 수도 있다.
  - `name`과 `dept_name` 각각 속성이 단독으로 사용되었을때 튜플을 유일하게 식별할 수 없는 속성이고
  - `name`과 `dept_name`의 조합이 instructor릴레이션에서 튜플을 구분하는 데 충분하다면 {name,dept_name}은 후보키이다.
- `ID + 다른 속성들` :  instructor 튜플은 구분가능 하지만 ID하나만으로도 후보 키를 이루기에 후보 키가 아니다.(유일성만 만족한다.)

<br/>

<br/>

### 주 키,기본키(Primary key)

주 키란 릴레이션 안에서 튜플을 구분하기 위한 수단으로 DB 설계자에 의해 **선택된 후보키**이다.

<br/>

> 키의 지정은 모델링하는 실세계의 제약 조건을 나타낸다. 그래서  주 키는 주 키 제약 조건(primary key constraint)을 갖는다.

**주 키 제약 조건(primary key constraint)**

- 주 키(Primary Key)로 지정된 속성에 대해 어떠한 튜플도 동시에 **중복 값**과 **NULL 값**을 허용하지 않도록 강제하는 규칙
- 이 제약조건은 테이블에서 **각 행을 고유하게 식별**할 수 있도록 보장한다.

<br/>

**주 키 관련 특성**

- 릴레이션 스키마의 다른 속성을 나열하기 전에 **주 키 속성을 먼저 나열**하는 것이 일반적이다.

- 주 키는 그 속성이 절대로 변하지 않거나 매우 드물게 변하도록 선택해야 한다.

  > 예를 들어, 개인의 주소 항목은 자주 바뀔 수 있기에 주 키의 일부분으로 사용하면 안 된다.

<br/>

<br/>

### 대체 키(Alternate key)

주 키로 선정되지 않고 남은 후보키들이다.

주 키가 없어지면 후보키는 주 키를 대체할 수 있다.

<br/>

<br/>

### 외래 키(Foreign key)

외래 키란 **다른 릴레이션의 주 키를 참조**하는 속성 또는 속성들의 집합

외래키로 지정되면 **참조 테이블의 기본키에 없는 값은 입력 불가**

<br/>

-------

<br/>

instructor 릴레이션에 나오는 어떤 튜플의 속성 dept_name이 department 릴레이션에 나오지 않는다면 매우 이상할 것이다. 

instructor 릴레이션의 어떤 튜플 ta가 있다면, ta의 dept_name 속성값이 반드시 department 릴레이션에 존재해야 한다.

즉, ta의 **dept_name 속성값이** dept_name을 주 키로 가지는 **department 릴레이션의 어떤 튜플 tb의 주 키로 나와야 한다**는 것이다.

<br/>

<br/>

### 외래키 제약 조건(foreign-key constraint)

릴레이션 스키마 r1의 **속성 A**와 다른 릴레이션 스키마 r2의 **주 키 B** 사이의 **외래 키 제약 조건**

- 모든 DB 인스턴스에 대해서 r1에 존재하는 각 튜플의 A값이 r2에 있는 어떤 튜플의 주 키 B값으로 반드시 나와야 한다는것을 의미한다.

<br/>

**속성 집합 A**를 r1으로부터 r2를 참조하는 **외래 키**라고 한다.

> instructor의 속성  dept_name은 instructor에서 department 릴레이션을  참조하는 **외래키다.**
>
> section 릴레이션의  building과 room_number는 classroom 릴레이션을 참조하는 **외래 키**를 구성한다.

<br/>

**릴레이션 r1은** 외래 키 종속을 가진 **참조하는 릴레이션**(referencing relation)이라고 부른다.

**릴레이션 r2는** **참조된 릴레이션**(referenced relation)이라고 한다.

<br/>

<br/>

### 참조 무결성 제약 조건(referential integrity constraint)

참조하는 릴레이션의 어떤 튜플의 **특정 속성에 출현한 값이** **참조되는 릴레이션에서** 적어도 하나의 튜플의 **특정 속성으로 출현**해야 한다는 것을 의미

외래 키 제약 조건은 참조 무결성 제약조건의 특별한 경우이다.

- 참조되는 속성이 참조된 릴레이션의 주 키를 구성하기 때문에 

<br/>

> 오늘날 DB 시스템은 외래 키 제약 조건을 지원한다. 그러나 참조되는 속성이 주 키가 아닌 참조 무결성 제약 조건은 지원하지 않는다.