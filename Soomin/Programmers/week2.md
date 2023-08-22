# Programmers 외
## With
### With문이란
- 이름을 가진 SubQuery를 정의한 후 사용하는 구문.
- Query의 전체적인 가독성을 높이고, 재사용할 수 있는 장점이 있음.
- 대부분의 DBMS에서 지원함.
- 계층형쿼리를 구현할 수 있음. 
- 오라클에서는 한번만 사용되면 Inline View, 두번이상 사용되면 Materialize View로 처리함.
  - 찬호샘께서 intermediate views 설명때 말씀하신 Materialize View
 
### 기본 구조
```sql
WITH [alias1] [(column1 [, column2])] as (
  SUB QUERY
)[, alias2 AS ...]
MAIN QUERY
```
- WITH [ alias ] AS ( SUB QUERY )
- 컬럼명은 생략할 수 있음.
- 쉼표(,)로 구분하여 여러개를 정의할 수 있음.
- 먼저 생성된 SubQuery는 나중에 생성하는 SubQuery에서 사용할 수 있음.
  - ex) [ alias2 ]에서 [ alias1 ]을 사용할 수 있음.
- With절 안 With 사용 가능
  - ex)
    ```sql
    WITH NEW_AND_REMOVED_WITHOUT_PROCESSING AS(
    	WITH MONTHLY_CUSTOMERS AS(
    	    SELECT ...
    	    FROM ...
    	)
    	SELECT ...
    	FROM (
    	    (
    	        SELECT ...
    	        FROM ...
    	    )
    	    UNION ALL (
    	        SELECT ...
    	        FROM ...
    	    )
    	 )NR
    	LEFT JOIN ...
    	ON ...
    )
    
    select ...
    from ...
    ```
- 단 Main Query가 없으면 실행시 에러
  

# IS NULL
## ISNULL 함수
- Microsoft SQL SQL Server (MSSQL) 의 내장 함수
- 사용법
  - ISNULL(column, null일 경우 대체할 값)
  - ISNULL(컬럼명, 대체할 값) = 비교 대상
    - 해당 컬럼의 NULL 값을 특정 값으로 대체해서 결과값을 만든 후, 그 결과로 나온 행들을 우항의 비교 대상과 비교해서 같은 것들만 출력

## IFNULL
- MySQL에서는 ISNULL이 없고 IFNULL 함수가 존재

## IS NULL
- Where 절에서 Null인 값 필터링 하고싶을 때 IS NULL 사용
- ex) WHERE name IS NULL
- 반대의 경우 IS NOT NULL


            ELSE TRUNCATE(PRICE, -4)
        END) AS PRICE_GROUP,
        COUNT(PRODUCT_ID) AS PRODUCTS
FROM PRODUCT
GROUP BY PRICE_GROUP
ORDER BY PRICE_GROUP
```
