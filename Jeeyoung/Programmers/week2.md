## SUM, MAX, MIN / IS NULL / JOIN

- [NULL 처리하기](#NULL-처리하기)
- [조건에 맞는 사용자와 총 거래금액 조회하기](#조건에-맞는-사용자와-총-거래금액-조회하기)
- [자동차 대여 기록에서 대여중 / 대여 가능 여부 구분하기](#자동차-대여-기록에서-대여중-/-대여-가능-여부-구분하기)

----------------------------------------------------

## IS NULL
### NULL 처리하기

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/59410

#### 코드

``` sql
SELECT ANIMAL_TYPE, IFNULL(NAME, 'No name') AS NAME, SEX_UPON_INTAKE 
FROM ANIMAL_INS
```

##### IFNULL
> 칼럼 값이 NULL 일 때 다른 값으로 대체/치환해주는 함수

+ 다른 방법
1) IF문 사용
``` sql
SELECT ANIMAL_TYPE, IF(NAME IS NULL, 'No name', NAME) AS NAME, SEX_UPON_INTAKE 
FROM ANIMAL_INS
```
2) CASE WHEN
``` sql
SELECT ANIMAL_TYPE, CASE WHEN NAME IS NULL THEN 'No name'
                    ELSE NAME
                    END AS NAME,
SEX_UPON_INTAKE 
FROM ANIMAL_INS
```

----------------------------------------------------
## JOIN
### 조건에 맞는 사용자와 총 거래금액 조회하기

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/164668

#### 코드

``` sql
SELECT B.USER_ID, B.NICKNAME, SUM(PRICE) AS TOTAL_SALES
FROM USED_GOODS_BOARD AS A, USED_GOODS_USER AS B
WHERE A.WRITER_ID = B.USER_ID AND A.STATUS = 'DONE'
GROUP BY B.USER_ID
HAVING SUM(PRICE) >= 700000
ORDER BY 3 ASC
```
##### WHERE 절과 HAVING 절의 차이
- HAVING 절은 그룹 전체 즉, 그룹을 나타내는 결과 집합의 행에만 적용된다
- WHERE 절은 개별 행에 적용된다
<br/>

- HAVING 절은 SQL select문이 집계 값이 지정된 조건을 충족하는 행만 반환하도록 지정한다
- WHERE 절은 단일 테이블에서 데이터를 가져 오거나 여러 테이블과 결합하여 조건을 지정한다
<br/>

- 집계함수는 HAVING 절과 함께 사용할 수 있다
- WHERE 절을 HAVING 절에 포함된 하위 쿼리에 있지 않으면 집계함수와 함께 사용할 수 없다

(집계함수란, COUNT, MIN, MAX, SUM, AVG 등)


----------------------------------------------------


### 자동차 대여 기록에서 대여중 / 대여 가능 여부 구분하기

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/157340

#### 코드

``` sql
SELECT CAR_ID, CASE WHEN '2022-10-16' BETWEEN start_date AND end_date 
                THEN '대여중'
                ELSE '대여 가능' END AS AVAILABILITY
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
WHERE end_date >= '2022-10-16'
GROUP BY 1
ORDER BY 1 DESC
```

##### CASE 
1) 새로운 열을 생성하는 경우
``` sql
SELECT CAR_ID, CASE WHEN '2022-10-16' BETWEEN start_date AND end_date THEN '대여중'
                ELSE '대여 가능' END AS AVAILABILITY
```
2) 열을 집계하는 경우 (집계함수와 함께 사용): 집계 열에 집계함수를 적용
``` sql
SELECT 집계함수((DISTINCT) CASE WHEN 기존 열 = 조건 THEN 집계 열 (ELSE 값) END) AS 새로운 열
```

