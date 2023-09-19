# GROUP BY
## 가격대 별 상품 개수 구하기 (Level 2)
- 가격대 정보를 각 구간의 최소 금액으로 설정
  - CASE 문 사용해서 각 구간별 가격대 값 정하기
  - 10000원보다 작은 경우는 0, 그 외의 경우는 천원 단위 버림해주기 -> TRUNCATE(숫자, 버릴 자릿수)
- 가격대 별 상품 개수
  - GROUP BY, COUNT
```sql
SELECT (CASE
            WHEN PRICE < 10000 THEN 0
            ELSE TRUNCATE(PRICE, -4)
        END) AS PRICE_GROUP,
        COUNT(PRODUCT_ID) AS PRODUCTS
FROM PRODUCT
GROUP BY PRICE_GROUP
ORDER BY PRICE_GROUP
```

## 자동차 종류 별 특정 옵션이 포함된 자동차 수 구하기 (Level 2)
- 내 답안
```sql
SELECT CAR_TYPE, COUNT(*) AS CARS
FROM CAR_RENTAL_COMPANY_CAR
WHERE OPTIONS LIKE '%통풍시트%' OR OPTIONS LIKE '%열선시트%' OR OPTIONS LIKE '%가죽시트%'
GROUP BY CAR_TYPE
ORDER BY CAR_TYPE
```

- 다른 방법
```sql
SELECT  CAR_TYPE
      , COUNT(*) AS CARS
FROM  CAR_RENTAL_COMPANY_CAR
WHERE  OPTIONS REGEXP '통풍시트|열선시트|가죽시트'
GROUP
  BY  CAR_TYPE
ORDER
  BY CAR_TYPE ASC ;
```
  - 정규식 이용 (REGEXP)

## 진료과별 총 예약 횟수 출력하기 (Level 2)
- 오답 이유
  - ORDER BY 문에서 '5월예약건수', '진료과코드' 이런식으로 X
  - GROUP BY, ORDER BY, HAVING 절에 alias를 쓸 경우 그대로 써주거나 `` (Backticks)를 사용
  - 작은 따옴표를 사용할 경우 따옴표 안 문자로 order by를 해주게 됨
- MONTH, YEAR 함수 사용, 비교 시 연,월,일을 따옴표 안에 넣어주기
  - 실행해본 결과 큰 따옴표, 따옴표 없이도 잘 돌아감

```sql
SELECT MCDP_CD AS '진료과코드', COUNT(PT_NO) AS '5월예약건수'
FROM APPOINTMENT
WHERE MONTH(APNT_YMD) = '05' AND YEAR(APNT_YMD) = '2022'
GROUP BY MCDP_CD
ORDER BY 5월예약건수 ASC, 진료과코드 ASC
```

## 즐겨찾기가 가장 많은 식당 정보 출력하기 (Level 3)
- 오답
  ```sql
  SELECT FOOD_TYPE, REST_ID, REST_NAME, MAX(FAVORITES) AS FAVORITES
  FROM REST_INFO
  GROUP BY FOOD_TYPE
  ORDER BY FOOD_TYPE DESC
  ```
  - MYSQL에서는 GROUP BY로 묶으면 가장 상단에 있는 데이터들을 임의로 가져오게 됨
  - 따라서 위 코드는 최대 즐겨찾기 수를 갖는 식당 정보를 가져오는게 아니라, 각 그룹에서 가장 상단에 있는 데이터를 가져온 것
 
- 정답
  ```sql
  SELECT FOOD_TYPE, REST_ID, REST_NAME, FAVORITES
  FROM REST_INFO
  WHERE (FOOD_TYPE, FAVORITES) 
  IN 
    (SELECT FOOD_TYPE, MAX(FAVORITES)
    FROM REST_INFO
    GROUP BY FOOD_TYPE )
  ORDER BY FOOD_TYPE DESC
  ```

## 자동차 대여 기록에서 대여중 / 대여 가능 여부 구분하기 (Level 3)
- 날짜 비교할 때 '' 사용할 것
- 처음 짠 코드 (오답)
  ```sql
  SELECT DISTINCT CAR_ID,
      CASE
          WHEN START_DATE <= '2022-10-16' AND END_DATE >= '2022-10-16' THEN '대여중'
          ELSE '대여 가능'
      END AS AVAILABILITY
  FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
  ORDER BY CAR_ID DESC
  ```
  - 이렇게 했더니 아래 결과와 같이 자전거가 여러번 대여되는 경우 중복 발생
    <img width="408" alt="image" src="https://github.com/dlanals/SQL-Study/assets/97150219/c048d73c-d77e-426b-958c-7bc0ea8afefb">
- 정답
  ```sql
  SELECT DISTINCT CAR_ID,
      CASE
          WHEN CAR_ID IN (SELECT CAR_ID
                          FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
                          WHERE '2022-10-16' BETWEEN START_DATE AND END_DATE) THEN '대여중'
          ELSE '대여 가능'
      END AS AVAILABILITY
  FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
  ORDER BY CAR_ID DESC
  ```
  - BETWEEN 사용
  - 서브쿼리 사용

## 입양 시각 구하기 2 (Level 4) - SET
- 단순히 Groupby 해버리면 입양이 발생하지 않은 시간대는 출력되지 않음
  ```sql
  SELECT HOUR(DATETIME) AS HOUR, COUNT(*) AS COUNT
  FROM ANIMAL_OUTS
  GROUP BY HOUR(DATETIME)
  ORDER BY HOUR
  ```
  
- 데이터가 없는 시간대를 위해 set 사용
  ```sql
  SET @HOUR = -1;
  SELECT (@HOUR := @HOUR +1) AS HOUR,
      (SELECT COUNT(HOUR(DATETIME)) 
      FROM ANIMAL_OUTS 
      WHERE HOUR(DATETIME)=@HOUR) AS COUNT 
      FROM ANIMAL_OUTS
  WHERE @HOUR < 23;
  ```
  - SET은 어떤 변수에 특정 값 할당할 떄 쓰는 명령어
  - SET @hour := -1 : 사용자 지정 변수 hour을 선언, 변수 값은 -1 (0시부터 나타내야함, 1씩 증가할거라)
  - @hour := @hour + 1 : ROW가 한번 지날 때마다 +1 (1씩 시간대가 증가)
  - WHERE @hour < 23 : 24시까지만 나타내고 종료
