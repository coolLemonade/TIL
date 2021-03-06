# 부속질의 Subquery
> 부속질의는 하나의 SQL 문 안에 다른 SQL 문이 중첩된*nested* 질의를 말한다.   
  다른 테이블에서 가져온 데이터로 현재 테이블에 있는 정보를 찾거나 가공할 때 사용한다.   
  두 테이블의 관계를 기초로 데이터를 확인하려면 조인*join* 혹은 부속질의를 사용하면 된다.

두 테이블을 연관시킬 때 조인을 선택할지 부속질의를 선택할지 여부는 **데이터의 형태와 양에 따라** 달라진다.   
일반적으로 **데이터가 대량일 경우 데이터를 모두 합쳐서 연산하는 조인보다 필요한 데이터만 찾아서 공급해주는 부속질의의 성능이 더 좋다.**

부속질의는 **위치와 역할**에 따라 구분된다.
- **부속질의의 종류**
  - **스칼라 부속질의** : **SELECT**절에 위치하며, **단일 값을 반환**한다.
  - **인라인 뷰** : **FROM**절에 위치하며, 결과를 **뷰*view* 형태로 반환**한다.
  - **중첩질의** : **WHERE**절에 위치하며, **결과를 한정**시키기 위해 사용된다. 상관 혹은 비상관 형태다.

**더 알아보기 - 부속질의의 다른 분류**   
부속질의는 동작하는 방식과 반환하는 결과의 형태에 따라 다음과 같이 분류할 수도 있다. 실제 분류 방법 및 용어는 각 DBMS마다 다르다.
- **동작 방식**   
부속질의가 주질의의 값을 참조하는가에 따라 상관 부속질의와 비상관 부속질의로 분류할 수 있다.
  - **상관 부속질의** : 주질의의 특정 열 값을 부속질의가 상속받아 부속질의의 질의에 사용하는 형태
  - **비상관 부속질의(일반 부속질의)** : 독립된 질의를 수행해서 결과 값을 가져오는 형태
- **반환하는 결과의 형태**   
부속질의가 단일 행을 반환하느냐 혹은 다중 행을 반환하느냐에 따라 분류할 수 있다.
  - **단일행 부속질의** : 부속질의의 결과 하나의 행을 반환하여 주질의에 전달한다. 비교 연산자의 수행이나 스칼라 부속질의 등에서 나타난다.
  - **다중행 부속질의** : 부속질의의 결과 여러 개의 행을 반환한다. IN 연산자를 사용하여 여러 행을 처리한다.

## 1. 스칼라 부속질의 - SELECT 부속질의
스칼라 부속질의(*scalar subquery*)는 SELECT 절에서 사용되는 부속질의로, 부속질의의 결과 값을 단일 행, **단일 열의 스칼라 값으로 반환**한다.   
만약 결과 값이 다중 행이거나 다중 열이라면 DBMS는 그 중 어떠한 행, 어떠한 열을 출력해야 하는지 알 수 없어 에러를 출력한다.   
또 **결과가 없는 경우에는 NULL값을 출력**한다.
일반적으로 **SELECT 문과 UPDATE SET 절에 사용**된다.   

```sql
select custid, (select name from customer cs where cs.custid = od.custid) 'name', sum(saleprice) 'total'
from orders od
group by od.custid;
```

```sql
update orders
set bname = (select bookname from book where book.bookid = orders.bookid);
```   
   
## 2. 인라인 뷰 - FROM 부속질의
인라인 뷰(*inline view*)는 FROM 절에서 사용되는 부속질의를 말한다.   
뷰(*view*)는 기존 테이블로부터 **일시적으로 만들어진 가상의 테이블**을 말한다.   
   
SQL 문의 FROM 절에는 테이블의 이름이 위치하는데, 여기에 **테이블 이름 대신 인라인 뷰 부속질의를 사용하면 보통의 테이블과 같은 형태로 사용할 수 있다.**   
   
```sql
select name, sum(saleprice) -- query cost : 2.41
from (select custid, name from customer where custid <= 2) cs, orders od
where cs.custid = od.custid
group by cs.name;
```   
실행 과정을 보면 먼저 cs 테이블을 계산해서 가상의 테이블(뷰)를 만들고 od 테이블과 조인을 한다.   
나머지는 일반적인 SQL 문의 처리 순서와 같다.   
인라인 뷰를 사용하면 **조인에 참여하기 직전에 필요한 데이터만 뽑아 조인시킬 수 있으므로 처리 성능을 높일 수 있다.**   
   
## 3. 중첩질의 - WHERE 부속질의
중첩질의(*nested query*)는 WHERE 절에서 사용되는 부속질의를 말한다.   
WHERE 절은 보통 데이터를 선택하는 조건 혹은 술어(*predicate*)와 같이 사용된다.   
   
  - **비교 연산자**   
  비교 연산자는 부속질의가 반드시 단일 행, 단일 열을 반환해야 하며, 아닐 경우 질의를 처리할 수 없다.   
  - **IN, NOT IN**   
  IN 연산자는 주질의의 속성 값이 부속질의에서 제공한 결과 집합에 있는지 확인*check*하는 역할을 한다.   
  IN 연산자에서 사용 가능한 부속질의는 결과로 다중 행, 다중 열을 반환할 수 있다.   
  주질의는 WHERE 절에 사용되는 속성 값을 부속질의의 결과 집합과 비교해 하나라도 있으면 참이 된다.   
  NOT IN은 이와 반대로 값이 존재하지 않으면 참이 된다.   
  ```sql
  -- 대한민국에 거주하는 고객에게 판매한 도서의 총 판매액 구하기
  query cost : 1.50
  select sum(saleprice)
  from orders
  where custid in (select custid from customer where address like '%대한민국%');
  ```   
   
  - **ALL, SOME(ANY)**   
    ALL, SOME(ANY) 연산자는 비교 연산자와 함께 사용된다.
    ALL은 모든, SOME은 어떠한(최소한 하나라도)이라는 의미를 가진다.
    ANY는 SOME과 동일한 기능을 한다.   
   
    ALL의 경우 부속질의의 결과 집합 전체를 대상으로 하므로 결과 집합의 최댓값(*MAX*)과 같다고 볼 수 있다.   
    SOME은 부속질의 결과 집합 중 어떠한 값을 의미하므로 최솟값(*MIN*)과 같다고 볼 수 있다.   
   
  - **EXISTS, NOT EXISTS**   
    EXISTS와 NOT EXISTS는 데이터의 존재 유무를 확인하는 연산자이다.   
    주질의에서 부속질의로 제공된 속성의 값을 가지고 부속질의에 조건을 만족하여 값이 존재하면 참이 되고, 주질의는 해당 행의 데이터를 출력한다.
    NOT EXISTS의 경우 이와 반대로 동작한다.   
  
    EXISTS 연산자는 다른 연산자와 달리 왼쪽에 스칼라 값이나 열을 명시하지 않는다.
    때문에 부속질의에 주질의의 열 이름이 제공되어야 한다.
    부속질의는 필요한 값이(조건을 만족하는 행) 발견되면 참 값을 반환한다.
  ```sql
  -- 대한민국에 거주하는 고객에게 판매한 도서의 총 판매액 구하기
  query cost : 1.25
  select sum(saleprice) 'total'
  from orders od
  where exists (select * from customer cs where address like '%대한민국%' and cs.custid = od.custid);
  ```
**출처** [MySQL로 배우는 데이터베이스 개론과 실습](http://www.yes24.com/Product/Goods/77724190)를 정리한 문서이다.

