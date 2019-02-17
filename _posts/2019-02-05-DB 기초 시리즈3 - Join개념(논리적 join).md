---
layout: post
title:  DB 기초 시리즈3 - Join개념(논리적 join) 
date:   2019-02-17
description: DB 기초 시리즈3로 DB 논리적조인 join에 대한 설명입니다.
tags: [DB, Databases, 기초, DBMS, join, 면접, 조인]
category: [DB, 면접]
---


저번 post DB 기초 시리즈2 - Join개념(물리적 join)에서도 말했지만 DB JOIN은 물리적 조인과 논리적 조인 둘로 나뉩니다. 이는 조인의 처리 방식, 조인의 종류로 나뉜다는 뜻입니다. join의 처리 방식은 join을 물리적으로 행하는 방식입니다. 그리고 그 종류로는 대표적으로 **1. NL(Nested Loof) join, 2. Sort Merge join, 3. Hash join**이 있습니다. 

<br/>

이번 포스트에서는 논리적 조인(조인의 종류)에 대해서 얘기를 하려고 합니다. 이번 포스트도 대부분의 내용을 www.dbguide.net에서 도움을 받았습니다. db를 공부하신다면 꼭 한 번 사이트에 들어가서 공부하시길 권장드립니다. 

<br/>

<br/>

### 논리적 조인

<br/>

논리적 조인(조인의 종류)에 대해서 다양한 블로그에서 다양한 분류로 설명을 해주시고 계십니다. 이번 포스트는 이러한 다양한 분류들의 정보는 어디에서 파생되었고 정확한 분류는 어떻게 해야하는지에 초점을 맞춰서 진행하려고 합니다. 

<br/>

현재 우리가 자주 사용하고 있는 DB의 논리적 조인을 이해하기 위해서는 표준 SQL을 살펴봐야 합니다.

<br/>

**1. Standard SQL**

<br/>

기존의 관계형 DBMS의 탄생을 보면 다음과 같습니다. 

1970년: Dr. E.F.Codd 관계형 DBMS(Relational DB) 논문 발표 

1974년: IBM SQL 개발

1979년: Oracle 상용 DBMS 발표

1980년: Sybase SQL Server 발표 (이후 Sybase ASE로 개명)

1983년: IBM DB2 발표

1986년: ANSI/ISO SQL 표준 최초 제정 (SQL-86, SQL1)

1992년: ANSI/ISO SQL 표준 개정 (SQL-92, SQL2)

1993년: MS SQL Server 발표 (Windows OS, Sybase Code 활용)

1999년: ANSI/ISO SQL 표준 개정 (SQL-99, SQL3)

2003년: ANSI/ISO SQL 표준 개정 (SQL-2003) 2008년: ANSI/ISO SQL 표준 개정 (SQL-2008)

<br/>

여기서 중요한 것은 ANSI/ISO SQL 표준입니다. 다양한 벤처사에서 다양한 DBMS가 만들어졌습니다. 그리고 사용하는 방법 또한 다양했습니다. 당연히 각 DBMS마다 통일성이 없었고 상호호환성이 부족했습니다. 이에 표준을 마련하기로합니다. 대표적인 기능은 다음과 같습니다. 

- STANDARD JOIN 기능 추가 (CROSS, OUTER JOIN 등 새로운 FROM 절 JOIN 기능들)

- SCALAR SUBQUERY, TOP-N QUERY 등의 새로운 SUBQUERY 기능들
- ROLLUP, CUBE, GROUPING SETS 등의 새로운 리포팅 기능
- WINDOW FUNCTION 같은 새로운 개념의 분석 기능들



여기서 우리가 눈여겨봐야 할 것은 당연히 **STANDARD JOIN** 입니다. 

<br/>

<br/>

### STANDARD JOIN(새로운 형태의 논리적 join)

<br/>

STANDARD라는 말이 붙었지만 맨 처음 만들 때야 새로운 것이었지 지금은 우리에게 익숙한 JOIN들입니다. 종류로는 inner join, natural join, cross join, outer join 등이 있습니다. 한 번 살펴보도록 하겠습니다. 

<br/>

**1.Inner Join**

<br/>

Inner Join은 Outer Join과 대비되는 개념입니다. join 조건에서 동일한 값이 있는 행만 반환합니다. Inner Join의 경우 join에 사용된 컬럼들을 별개의 컬럼으로 표시합니다.(출력되는 경우 두개의 같은 컬럼이 나오게 됩니다. )

![image-20190216223641431](/assets/img//image-20190216223641431.png)

<br/>

예)

새로운 형태의 from 절 Join 형태 1

```sql
select * from tableA inner join tableB on tableA.name = tableB.name
```

새로운 형태의 from 절 Join 형태 2

(inner는 join의 default옵션임으로 아래와 같이 생략 가능 )

```sql
select * from tableA join tableB on tableA.name = tableB.name
```

기존 where 절 join 형태

```sql
select * from tableA, tableB where tableA.name = tableB.name
```





<br/>

<br/>

**2. Natural Join**

<br/>

Natural Join은 두 테이블 간의 동일한 이름을 갖는 모든 칼럼들에 대해 EQUI(=) JOIN을 수행합니다. Natural Join이 명시되면 , 추가로 on 조건절, where 절에서 join 조건을 정의할 수 없게됩니다.(Using 조건절은 사용할 수 있음)

Natural Join은 Join에 사용된 컬럼을 하나의 컬럼으로 표시합니다.(Inner Join은 별개의 컬럼으로 표시함)

**주의사항으로** JOIN에 사용될 칼럼들은 같은 데이터 유형이여야 하며, ALIAS나 테이블 명과 같은 접두사를 붙일 수 없습니다. 



예)

일반적인 natural join

```sql
select * from tableA natural join tableB
```

에러가 나는 sql문 (EMP.DEPTNO에서 접두어를 붙여서)

```sql
select EMP.DEPTNO, EMPNO from emp natural join dept; #ERROR: NATURAL JOIN에 사용된 열은 식별자를 가질 수 없음
```

<br/>

<br/>


**3. Cross Join**

<br/>

Cross Join은 일치하는 컬럼에 대한 조건이 없으며 모든 경우를 고려하는 조인이라고 생각하시면 됩니다. 즉 해당 테이블들의 모든 데이터 조합이 결과롤 리턴됩니다. 보통은 많이 사용되지 않지만, 리포트, 튜닝을 작성하기 위해 아니면 매달, 날짜별로 출력하기 위해서 사용할 수 있습니다. 

![image-20190217133939318](/assets/img//image-20190217133939318.png)

<br/>

예)

```sql
select * from tableA cross join tableB
```

<br/>

<br/>

**4.Outer Join**

<br/>

Outer Join은 Inner Join과 대비되는 개념입니다. Join 조건에서 동일한 값이 없는 행도 반환할 때도 사용할 수 있습니다. Left Outer Join, Right Outer Join, Full Outer Join 세가지로 나뉩니다. Outer Join을 명시할 경우 From절의 Join조건을 정의하겠다는 표시이기 때문에 Using 조건이나, On 조건절을 필수적으로 사용해야 합니다. Left/Right Outer Join의 경우 기준이 되는 테이블이 조인 수행시 무조건 드라이빙 테이블이 됩니다. 

![image-20190217134305978](/assets/img//image-20190217134305978.png)

<br/>

- **Left Outer Join**

<br/>

  조인 수행시 먼저 표기된 좌측 테이블에 해당하는 데이터를 먼저 읽은 후, 나중 표기된 우측 테이블에서 Join 대상 데이터를 읽어 옵니다. 즉 Table A(기준 테이블), Table B이 있을 때 A와 B를 비교하여 B의 컬럼에서 Join컬럼과 같은 값이 있을 때 해당 데이터를 가져오고, 없을 때는 NULL 값으로 채웁니다. Left Outer Join은 Left Join으로 Outer를 생략할 수 있습니다. 

![image-20190217134552524](/assets/img//image-20190217134552524.png)

<br/>

- **Right Outer Join**

<br/>

  Left Outer Join과는 반대로 우측 테이블이 드라이빙 테이블이 되어 join을 주도합니다. 

![image-20190217134618217](/assets/img//image-20190217134618217.png)

<br/>

- **Full Outer Join**
<br/>

  조인 수행시 좌측, 우측 테이블의 모든 데이터를 읽어 Join합니다. 테이블 A, B모두 기준이 됩니다. 

![image-20190217134811414](/assets/img//image-20190217134811414.png)

<br/>

<br/>

**5. Using 조건절 vs On 조건절**

<br/>

Join의 조건으로는 Using과 On조건절을 사용할 수 있습니다. 두 조건절이 하는 역할은 비슷합니다. join을 할 수 있는 조건을 명시합니다. 그 차이는 미비하지만 중요할 수 있습니다. 

<br/>

- Using 조건절 
<br/>

  1. 두 테이블의 join 컬럼이 동일한 이름을 쓸 때 사용가능합니다. 

  2. equi join(=)을 합니다. 

  3. join컬럼의 결과를 하나로 통일합니다. 

     - ```sql
       +------+
       |   i  |
       +------+
       |    1 |
       +------+
       ```


  예)

  ```sql
  select * from tableA inner join tableB using(name) # 공통된 컬럼이름은 name으로 한다. 
  ```

<br/>

- On 조건절

<br/>

  1. 두 테이블의 Join 컬럼이 동일한 이름이 아니어도 상관 없습니다. 

  2. equi join(=), non equi join(>, <) 다 가능합니다.

  3. join컬럼의 결과가 통일 되지 않고 각각 나오게 됩니다. 

     - ```
       +------+------+
       |  i    | i   |
       +------+------+
       |    1 |    1 |
       +------+------+
       ```

  예)

  ```sql
  select * from tableA inner join tableB on(tableA.name = tableB.name)
  ```

<br/>

<br/>

**6. equi join vs non equi join**

<br/>

많은 블로그에서 equi join(=)은 inner join에 속한 조인으로 설명하는 경우가 많습니다. 이것은 잘 못된 설명으로 equi join은 inner join, outer join모두 될 수 있습니다. 그저 **equi join은 =(동등 비교)를 통해서 join을 결정하느냐 아니냐의 차이일 뿐입니다.** inner join의 하위개념이 아닙니다. **non equi join은 당연히 =(동등 비교)가 아닌 나머지를 뜻합니다.** 처음 join의 종류를 배울 때 너무 많은 join들의 상관관계가 헷갈려 이해하기 어려운 경우가 생깁니다. 표준 join의 개념을 익히고 그에 따른 분류를 따라가야 합니다. 

<br/>

<br/>

**요약**

- 연산자에 따른 분류
  - equi join
  - non equi join
- 표준 sql에 따라 생긴 새로운 논리적 join(from절에 따른 join조건)에 따른 분류
  - inner join
  - natural join
  - cross join
  - outer join
- 요약 표

![image-20190217145532317](/assets/img//image-20190217145532317.png)

<br/>

<br/>



**마치며**

이상으로 DB Join에 대한 설명을 마치겠습니다. 많은 내용들을 정리한 내용이며 나의 생각을 정리했습니다. 혹시라도 잘못된 점이 있다면 피드백 주시면 감사하겠습니다.

<br/>

<br/>

### 참고자료 

http://www.dbguide.net/db.db?cmd=view&boardUid=148198&boardConfigUid=9&categoryUid=216&boardIdx=135&boardStep=1

https://www.quora.com/What-is-the-difference-between-inner-join-and-equijoin-in-Oracle

https://searchoracle.techtarget.com/answer/What-s-the-difference-between-an-SQL-inner-join-and-equijoin

https://www.w3resource.com/sql/joins/perform-an-equi-join.php

https://stackoverflow.com/questions/17759687/cross-join-vs-inner-join-in-sql-server-2008

https://stackoverflow.com/questions/406294/left-join-vs-left-outer-join-in-sql-server

https://stackoverflow.com/questions/11366006/mysql-on-vs-using