---
layout: post
title:  "[MariaDB] 페이징 쿼리 "
subtitle:   "페이징"
categories: dev
tags: note
---















### 준비사항

- 한번에 몇개를 보여줄 것인지 ex) 10개씩 
- 현재 몇번째 페이지를 보여줄 것인지 ex) 1페이지, 2페이지
- 전체 건수 ex) 조회조건등을 설정하여 반횐되는 total count



# 페이징쿼리

```sql
    select 
            sql_calc_found_rows
            user_id,
            user_name,
            user_age
    from    user
    where   1=1
    LIMIT ${count} OFFSET ${startRows}
``` 

- LIMIT ${count}: 한번에 몇개를 보여줄 것인지 ex) 10개씩 
- OFFSET ${startRows}: 어디서부터 가져올것인지((현재페이지-1)*보여줄 글 수) ex) 0 , 10 , 20
- sql_calc_found_rows : 조회된 전체 row수를 임시로 저장함 (아래서 다시 설명)





# 전체 데이터 건수 구하기

- 방법 1
```sql
SELECT FOUND_ROWS() AS totalCnt;
```
- FOUND_ROWS() : 직전 쿼리에서 검색된 row수를 반환한다.
  
- sql_calc_found_rows : 쿼리 전체의 row수를 임시로 저장합니다.
여기서 반환되는 값은 LIMIT로 반환되는 row수가 아니라 전체 row 수다.
LIMIT가 있어 10개의 row만 가져온다 하더라도 LIMIT가 없을때의 결과 row수를 가져온다.
LIMIT의 영향을 받지 않으므로 쿼리 성능을 떨어트릴수 있다.(FULL SCAN)


- 방법 2
```sql
select count(1) AS totalCnt from user
```
일반적인 get Total Count



### 주의

FOUND_ROWS() , SQL_CALC_FOUND_ROWS는가 모든 상황에 최적화 되어있고, 모둔 상황의 성능을 떨어트리는것은 아니다.


DB성능에 관련해서는 인덱스 등 고려해야할 사항이 워낙 많기 때문이다. 그래서 몇가지만 확인해 보겠다.


쿼리가 index를 잘 타게 되서 쿼리의 비용이 줄어드는 경우에 SQL_CALC_FOUND_ROWS을 포함시켜 풀스캔을 하게되면 성능이 떨어진다.


그러나 SQL_CALC_FOUND_ROWS가 아닌 Index에 의한 정렬(Order by)등 풀스캔을 하게 되는 경우가 있다면


어짜피 풀스캔을 하는거 SQL_CALC_FOUND_ROWS을 통해 풀 스캔을 한번으로 줄일 수 있다.


## 정리

쿼리에 성능은 그때 그때 다르기 때문에 SQL_CALC_FOUND_ROWS이 만능은 아니다.


그러므로 SQL_CALC_FOUND_ROWS는 전체 스캔(Full Scan)을 피할 수 없는 상황에서 


이왕 전체 스캔한거 count를 위해 다시 하지 않도록 활용하는 것이 좋을듯 싶다.


[참고 - blog](https://blog.asamaru.net/2015/09/11/using-sql-calc-found-rows-and-found-rows-with-mysql/)


[참고 - blog](https://livetodaykono.tistory.com/71)
