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
