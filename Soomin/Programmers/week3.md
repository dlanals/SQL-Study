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
