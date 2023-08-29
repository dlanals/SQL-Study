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
