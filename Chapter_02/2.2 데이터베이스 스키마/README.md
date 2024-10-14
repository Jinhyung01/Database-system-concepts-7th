## 2.2 데이터베이스 스키마

**데이터베이스 스키마(database schema)** : DB의 논리적 설계

**데이터베이스 인스턴스(database instance)** : 어떤 한순간에 DB에 저장되어 있는 데이터의 스냅샷(snapshot)

<br/>

마찬가지로

**릴레이션 스키마(releation schema)** : 릴레이션에 어떤 정보가 담길지(헤더)를 정의하는 역할

- 프로그래밍 언어의 type definition에 해당된다.

- 속성과, 그 속성이 가지는 도메인의 명세로 구성된다.

  > 도메인의 명세 : 해당 속성이 가질 수 있는 값의 **범위** 또는 **타입**을 정의

<br/>

**릴레이션 인스턴스**: 릴레이션 스키마에 실제로 저장되는 데이터의 집합

- 프로그래밍 언어에서 변수의 값과 비슷하다.

- 릴레이션 인스턴스의 튜플도 릴레이션이 변경됨에 따라 변하게된다. 하지만 일반적으로 릴레이션의 스키마는 변하지 않음

<br/>

<br/>

department(학과) 릴레이션의 스키마 

- `department(dept_name, building, budget)`

아래 그림 2.5는 department 릴레이션의 인스턴스의 예이다. 



![그림 2 5 department 릴레이션](https://github.com/user-attachments/assets/a42d666e-1372-4e0e-bc11-6e2bfef8b745)

dept_name속성은 instructor 스키마와 department 스키마에 모두 나타나는 것을 알 수 있다.

**릴레이션 스키마에서 공통적인 속성을 사용하는 것은, 서로 다른 릴레이션에 있는 튜플을 연관시키는 방법 중 하나이다.**

> 예를 들어, Waston building에서 일하는 교수에 대한 모든 정보를 찾고 싶다고 하자. 
>
> 이를 위해서는 department 릴레이션에서 building이 Waston인 모든 dept_name을 찾아야한다. 
>
> 그리고 instructor 릴레이션에서 해당 dept_name을 가지고 있는 모든 교수의 정보를 찾아야한다.

<br/>

<br/>

대학교에서 각 수업은 학기(semester)마다 한번, 혹은 한 학기 안에서도 여러 분반(section)이 열릴 수 있다.

각 수업의 관계를 표현할 수 있는 스키마를 제공해야한다.

- `section(course_id, sec_id, semester, year, building, room_number, time_slot_id)`

<br/>

아래 그림 2.6은 section relation의 인스턴스의 예이다.

![그림 2 6 section 릴레이션](https://github.com/user-attachments/assets/f7ccbe13-d02c-42f8-bd48-5a81e8c750a4)

<br/>

<br/>

이러한 상황에서 교수와 수업 사이의 관계를 표현할 수 있는 릴레이션이 필요하다.

이러한 관계를 표현할 수 있는 릴레이션의 스키마는 다음과 같다.

- `teaches(ID, course_id, sec_id, semester, year)`

<br/>

아래 그림 2.7은 teaches 릴레이션의 인스턴스의 한 예다.

![그림 2 7 teaches 릴레이션](https://github.com/user-attachments/assets/27174188-2bcb-4a16-a243-0a0c1a04ed83)