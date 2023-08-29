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

```sql
```
