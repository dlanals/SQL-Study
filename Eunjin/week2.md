## week2 - SUM,MAX,MIN/ IS NULL/ JOIN
------------------------------------------------
## SUM,MAX,MIN

### 가격이 제일 비싼 식품의 정보 출력하기

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/131115

#### 코드

``` sql
SELECT PRODUCT_ID, PRODUCT_NAME, PRODUCT_CD, CATEGORY, PRICE
FROM FOOD_PRODUCT
ORDER BY PRICE DESC
LIMIT 1
```
----------------------------------------------------

### 가장 비싼 상품 구하기

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/131697

#### 코드

``` sql
SELECT PRICE AS MAX_PRICE
FROM PRODUCT
ORDER BY PRICE DESC LIMIT 1
```
----------------------------------------------------
### 최댓값 구하기

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/59415

#### 코드
SELECT DATETIME
FROM ANIMAL_INS
ORDER BY DATETIME DESC
LIMIT 1
``` sql

```
----------------------------------------------------
### 최솟값 구하기

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/59038

#### 코드

``` sql
SELECT DATETIME
FROM ANIMAL_INS
ORDER BY DATETIME
LIMIT 1
```
----------------------------------------------------
### 동물 수 구하기

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/59406

#### 코드

``` sql
SELECT COUNT(ANIMAL_ID)
FROM ANIMAL_INS
```
----------------------------------------------------
### 중복 제거하기

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/59408

#### 코드

``` sql
SELECT COUNT(DISTINCT NAME) 
FROM ANIMAL_INS
```
----------------------------------------------------
## IS NULL

### 경기도에 위치한 식품창고 목록 출력하기

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/131114

#### 코드

``` sql
SELECT WAREHOUSE_ID, WAREHOUSE_NAME, ADDRESS, IFNULL(FREEZER_YN, 'N') AS FREEZER_YN
FROM FOOD_WAREHOUSE
WHERE ADDRESS LIKE '경기%'
ORDER BY WAREHOUSE_ID
```
----------------------------------------------------
### 이름이 없는 동물의 아이디

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/59039

#### 코드

``` sql
SELECT ANIMAL_ID
FROM ANIMAL_INS
WHERE NAME IS NULL
ORDER BY ANIMAL_ID
```
----------------------------------------------------
### 이름이 있는 동물의 아이디

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/59407

#### 코드

``` sql
SELECT ANIMAL_ID
FROM ANIMAL_INS
WHERE NAME IS NOT NULL
ORDER BY ANIMAL_ID
```
----------------------------------------------------
### NULL 처리하기

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/59410

#### 코드

``` sql
SELECT ANIMAL_TYPE, IFNULL(NAME, 'No name') AS NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```
----------------------------------------------------
### 나이 정보가 없는 회원 수 구하기

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/131528

#### 코드

``` sql
SELECT COUNT(USER_ID) AS USERS
FROM USER_INFO
WHERE AGE IS NULL
```
----------------------------------------------------
## JOIN

### 그룹별 조건에 맞는 식당 목록 출력하기

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/131124

#### 코드

``` sql
SELECT MP.MEMBER_NAME, RR.REVIEW_TEXT, DATE_FORMAT(RR.REVIEW_DATE, '%Y-%m-%d')as REVIEW_DATE
FROM MEMBER_PROFILE MP JOIN REST_REVIEW RR ON MP.MEMBER_ID = RR.MEMBER_ID
WHERE RR.MEMBER_ID = (SELECT MEMBER_ID FROM REST_REVIEW
                     GROUP BY MEMBER_ID
                     ORDER BY COUNT(MEMBER_ID) DESC
                     LIMIT 1)
ORDER BY RR.REVIEW_DATE, RR.REVIEW_TEXT
```
----------------------------------------------------
### 주문량이 많은 아이스크림들 조회하기

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/133027

#### 코드

``` sql
SELECT FH.FLAVOR
FROM FIRST_HALF FH, JULY J
WHERE FH.FLAVOR = J.FLAVOR
GROUP BY FH.FLAVOR 
ORDER BY SUM(FH.TOTAL_ORDER + J.TOTAL_ORDER) DESC LIMIT 3
```
----------------------------------------------------
### 5월 식품들의 총매출 조회하기

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/131117

#### 코드

``` sql
SELECT P.PRODUCT_ID, P.PRODUCT_NAME, P.PRICE * SUM(O.AMOUNT) AS TOTAL_SALES
FROM FOOD_PRODUCT P, FOOD_ORDER O
WHERE P.PRODUCT_ID = O.PRODUCT_ID
AND O.PRODUCE_DATE LIKE '2022-05%'
GROUP BY P.PRODUCT_ID
ORDER BY TOTAL_SALES DESC, P.PRODUCT_ID ASC
```
----------------------------------------------------
### 조건에 맞는 도서와 저자 리스트 출력하기

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/144854

#### 코드

``` sql
SELECT B.BOOK_ID, A.AUTHOR_NAME, DATE_FORMAT(B.PUBLISHED_DATE, '%Y-%m-%d') AS PUBLISHED_DATE
FROM BOOK B, AUTHOR A
WHERE B.AUTHOR_ID = A.AUTHOR_ID AND B.CATEGORY = '경제'
ORDER BY B.PUBLISHED_DATE
```
----------------------------------------------------
### 특정 기간동안 대여 가능한 자동차들의 대여비용 구하기

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/157339

#### 코드

``` sql
SELECT C.CAR_ID, C.CAR_TYPE, 
    ROUND(C.DAILY_FEE*30*(1-(D.DISCOUNT_RATE/100)))AS FEE
FROM CAR_RENTAL_COMPANY_CAR C
JOIN CAR_RENTAL_COMPANY_DISCOUNT_PLAN D ON C.CAR_TYPE = D.CAR_TYPE
WHERE C.CAR_TYPE IN ('세단', 'SUV')
    AND D.DURATION_TYPE = '30일 이상'
    AND C.CAR_ID NOT IN (
    SELECT H.CAR_ID
    FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY H
        WHERE (H.END_DATE BETWEEN '2022-11-01' AND '2022-11-30') 
            OR (H.START_DATE BETWEEN '2022-11-01' AND '2022-11-30') 
            OR (H.START_DATE <'2022-11-01' AND H.END_DATE > '2022-11-30'))
    AND C.DAILY_FEE*30*(1-(D.DISCOUNT_RATE/100)) >= 500000 
    AND C.DAILY_FEE*30*(1-(D.DISCOUNT_RATE/100)) < 2000000
ORDER BY FEE DESC, C.CAR_TYPE ASC, C.CAR_ID DESC

```
----------------------------------------------------
### 없어진 기록 찾기

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/59042

#### 코드

``` sql
SELECT O.ANIMAL_ID, O.NAME
FROM ANIMAL_INS I
RIGHT JOIN ANIMAL_OUTS O ON I.ANIMAL_ID = O.ANIMAL_ID
WHERE I.DATETIME IS NULL
ORDER BY O.ANIMAL_ID, O.NAME
```
----------------------------------------------------
### 있었는데요 없었습니다

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/59043

#### 코드

``` sql
SELECT I.ANIMAL_ID, I.NAME
FROM ANIMAL_INS I, ANIMAL_OUTS O 
WHERE I.ANIMAL_ID = O.ANIMAL_ID AND I.DATETIME > O.DATETIME
ORDER BY I.DATETIME
```
----------------------------------------------------
###오랜 기간 보호한 동물(1)

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/59044

#### 코드

``` sql
SELECT I.NAME, I.DATETIME
FROM ANIMAL_INS I
LEFT JOIN ANIMAL_OUTS O ON I.ANIMAL_ID = O.ANIMAL_ID
WHERE O.DATETIME IS NULL
ORDER BY I.DATETIME LIMIT 3
```
----------------------------------------------------
### 보호소에서 중성화한 동물

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/59045

#### 코드

``` sql
SELECT I.ANIMAL_ID, I.ANIMAL_TYPE, I.NAME
FROM ANIMAL_INS I, ANIMAL_OUTS O
WHERE I.ANIMAL_ID = O.ANIMAL_ID
    AND I.SEX_UPON_INTAKE LIKE 'Intact%' 
    and O.SEX_UPON_OUTCOME REGEXP 'Spayed|Neutered'
ORDER BY I.ANIMAL_ID
```
----------------------------------------------------
### 상품 별 오프라인 매출 구하기

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/131533

#### 코드

``` sql
SELECT P.PRODUCT_CODE, P.PRICE * SUM(OS.SALES_AMOUNT) AS SALES
FROM PRODUCT P, OFFLINE_SALE OS
WHERE P.PRODUCT_ID = OS.PRODUCT_ID
GROUP BY P.PRODUCT_CODE
ORDER BY SALES DESC, PRODUCT_CODE ASC
```
----------------------------------------------------
### 상품을 구매한 회원 비율 구하기

#### 문제
https://school.programmers.co.kr/learn/courses/30/lessons/131534

#### 코드

``` sql
SELECT YEAR(O.SALES_DATE) AS YEAR, MONTH(O.SALES_DATE) AS MONTH, 
COUNT(DISTINCT O.USER_ID) AS PUCHASED_USERS, 
ROUND(COUNT(DISTINCT O.USER_ID)/
      (SELECT COUNT(U.USER_ID) 
       FROM USER_INFO U
       WHERE U.JOINED LIKE '2021%'),
       1) AS 'PURCHASED_RATIO'
FROM USER_INFO U
JOIN ONLINE_SALE O ON U.USER_ID = O.USER_ID
WHERE YEAR(U.JOINED) = 2021
GROUP BY YEAR, MONTH
ORDER BY YEAR, MONTH
```
----------------------------------------------------
