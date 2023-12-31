## week1_select

### 강원도에 위치한 생산공장 목록 출력하기
#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/131112

#### 코드

``` sql
SELECT FACTORY_ID, FACTORY_NAME, ADDRESS
FROM FOOD_FACTORY
WHERE ADDRESS LIKE '강원도%'
ORDER BY FACTORY_ID ASC
```
----------------------------------------------------

### 서울에 위치한 식당 목록 출력하기
#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/131118

#### 코드

``` sql
SELECT i.rest_id, i.rest_name, i.food_type, i.favorites, i.address, 
    round(avg(r.review_score),2) as score
from rest_info i
join rest_review r on i.rest_id = r.rest_id
where i.address like '서울%'
group by i.rest_id
order by score desc, i.favorites desc
```

----------------------------------------------------

### 조건에 맞는 도서 리스트 출력하기
#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/144853

#### 코드

``` sql
SELECT BOOK_ID, DATE_FORMAT(PUBLISHED_DATE,'%Y-%m-%d')
FROM BOOK
WHERE YEAR(PUBLISHED_DATE) = '2021' AND CATEGORY = '인문'
ORDER BY PUBLISHED_DATE ASC
```

----------------------------------------------------

### 평균 일일 대여 요금 구하기
#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/151136

#### 코드

``` sql
SELECT  ROUND(AVG(DAILY_FEE)) AS 'AVGERAGE_FEE'
FROM CAR_RENTAL_COMPANY_CAR
WHERE CAR_TYPE = 'SUV'
```

----------------------------------------------------

### 조건에 부합하는 중고거래 댓글 조회하기
#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/131112

#### 코드

``` sql
SELECT B.TITLE, B.BOARD_ID, R.REPLY_ID, R.WRITER_ID, R.CONTENTS, DATE_FORMAT(R.CREATED_DATE,'%Y-%m-%d')
FROM USED_GOODS_BOARD B
JOIN USED_GOODS_REPLY R ON B.BOARD_ID = R.BOARD_ID
WHERE B.CREATED_DATE like '2022-10-%'
ORDER BY R.CREATED_DATE asc, B.title asc
```

----------------------------------------------------

### 인기있는 아이스크림
#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/133024

#### 코드

``` sql
SELECT FLAVOR
FROM FIRST_HALF
ORDER BY TOTAL_ORDER DESC, SHIPMENT_ID ASC
```

----------------------------------------------------

### 흉부외과 또는 일반외과 의사 목록 출력하기
#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/132203

#### 코드

``` sql
SELECT DR_NAME, DR_ID , MCDP_CD, DATE_FORMAT(HIRE_YMD, '%Y-%m-%d')
FROM DOCTOR
WHERE MCDP_CD IN ('CS', 'GS')
ORDER BY HIRE_YMD DESC, DR_NAME ASC
```

----------------------------------------------------

### 3월에 태어난 여성 회원 목록 출력하기
#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/131120

#### 코드

``` sql
SELECT MEMBER_ID, MEMBER_NAME, GENDER, DATE_FORMAT(DATE_OF_BIRTH, '%Y-%m-%d')
FROM MEMBER_PROFILE
WHERE MONTH(DATE_OF_BIRTH) = 3 AND GENDER = 'W' AND TLNO IS NOT NULL
ORDER BY MEMBER_ID
```
----------------------------------------------------

### 12세 이하인 여자 환자 목록 출력하기
#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/132201

#### 코드

``` sql
SELECT PT_NAME, PT_NO, GEND_CD, AGE, 
    CASE WHEN TLNO IS NULL THEN 'NONE' ELSE TLNO END AS 'TLNO'
FROM PATIENT
WHERE AGE <= 12 AND GEND_CD = 'W'
ORDER BY AGE DESC, PT_NAME ASC
----------------------------------------------------

### 모든 레코드 조회하기
#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/59034

#### 코드

``` sql
SELECT ANIMAL_ID, ANIMAL_TYPE, DATETIME, INTAKE_CONDITION, NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```
----------------------------------------------------

### 재구매가 일어난 상품과 회원 리스트 구하기
#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/131536

#### 코드

``` sql
SELECT USER_ID, PRODUCT_ID
FROM ONLINE_SALE
GROUP BY USER_ID, PRODUCT_ID
HAVING COUNT(PRODUCT_ID)>1
ORDER BY USER_ID ASC, PRODUCT_ID DESC
```
----------------------------------------------------

### 역순 정렬하기
#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/59035

#### 코드

``` sql
SELECT NAME, DATETIME
FROM ANIMAL_INS
ORDER BY ANIMAL_ID DESC
```
----------------------------------------------------

### 오프라인/온라인 판매 데이터 통합하기
#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/131537

#### 코드

``` sql
SELECT DATE_FORMAT(SALES_DATE,'%Y-%m-%d') AS SALES_DATE, PRODUCT_ID, USER_ID, SALES_AMOUNT
FROM ONLINE_SALE
WHERE YEAR(SALES_DATE) = 2022 AND MONTH(SALES_DATE) =3
UNION ALL
SELECT DATE_FORMAT(SALES_DATE,'%Y-%m-%d') AS SALES_DATE, PRODUCT_ID, NULL AS USER_ID, SALES_AMOUNT
FROM OFFLINE_SALE
WHERE YEAR(SALES_DATE) = 2022 AND MONTH(SALES_DATE) =3
ORDER BY SALES_DATE, PRODUCT_ID , USER_ID
```
----------------------------------------------------

### 아픈 동물 찾기
#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/59036

#### 코드

``` sql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE INTAKE_CONDITION = 'Sick'
```
----------------------------------------------------

### 어린 동물 찾기
#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/59037

#### 코드

``` sql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE INTAKE_CONDITION != 'Aged'
ORDER BY ANIMAL_ID
```
----------------------------------------------------

### 동물의 아이디와 이름
#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/59403
#### 코드

``` sql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```
----------------------------------------------------

### 여러 기준으로 정렬하기
#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/59404

#### 코드

``` sql
SELECT ANIMAL_ID, NAME, DATETIME
FROM ANIMAL_INS
ORDER BY NAME ASC, DATETIME DESC
```
----------------------------------------------------

### 상위 n개 레코드
#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/59405

#### 코드

``` sql
SELECT NAME
FROM ANIMAL_INS
ORDER BY DATETIME 
LIMIT 1
```
----------------------------------------------------

### 조건에 맞는 회원수 구하기
#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/131535

#### 코드

``` sql
SELECT COUNT(USER_ID)
FROM USER_INFO
WHERE AGE >= 20 AND AGE <= 29
    AND YEAR(JOINED) = 2021
```
