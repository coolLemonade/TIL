# EXISTS vs IN

> EXISTS를 이해하는데 도움이 되는 설명들을 모아 놓은 글이다.

### EXISTS와 IN
SQL의 WHERE절에서 **조건에 따라 데이터를 걸러내어 결과를 조회**할 때 사용한다.

### EXISTS
- 서브 쿼리의 모든 레코드의 **존재를 테스트** 하는 데 사용한다.
- 서브 쿼리가 반환하는 결과 값이 있는지 조사한다.
- **반환된 ROW가 있는지 없는지만 보고 값이 있다면 참, 없으면 거짓을 반환**한다.
- 한 테이블이 다른 테이블과 외래키와 같은 관계에 있을 때 유용하다.
- 조건에 해당하는 ROW의 존재 유무를 확인하면 더이상 수행하지 않는다.
- 일반적으로 SELECT절까지 가지 않기 때문에 **IN에 비해서 속도나 성능면에서 더 좋다.**
- **메인 쿼리에서 EXISTS 쿼리 순으로 진행**한다.
- 메인 쿼리부터 처리하기 때문에 서브 쿼리를 처리할 때 모든 정보를 가지고 있다.
   
```sql
SELECT name, address
FROM customer
WHERE EXISTS (
  SELECT *
  FROM orders
  WHERE customer.custid = orders.custid
); -- Query cost : 0.75
```

### IN  
- **실제로 존재하는 데이터의 값을 비교**하기 때문에 **EXISTS보다 속도가 느린 편**이다.
- 조건에 해당하는 ROW의 COLUMN을 비교하며 확인한다.
- **SELECT절에서 조회한 COLUMN 값을 비교**하기 때문에 EXISTS에 비해 성능이 떨어진다.
- **IN 쿼리에서 메인 쿼리 순으로 진행**한다.
- 서브 쿼리를 처리할 때 입력된 메인 쿼리의 정보가 없다.
   
```sql
SELECT name, address
FROM customer
WHERE customer.custid IN (
  SELECT custid
  FROM orders
  WHERE customer.custid = orders.custid
); -- Query cost : 3.25
```
**참고**   
[MySQL EXISTS와 IN 설명과 차이점](https://wedul.site/450)      
[MySQL EXISTS와 IN 사용법 비교하기](https://codingspooning.tistory.com/entry/MySQL-EXISTS%EC%99%80-IN-%EC%82%AC%EC%9A%A9%EB%B2%95-%EB%B9%84%EA%B5%90%ED%95%98%EA%B8%B0-%EC%98%88%EC%A0%9C)   
[MySQL EXISTS, NOT EXISTS를 사용하여 데이터 확인하기](https://kkh0977.tistory.com/1141)   
