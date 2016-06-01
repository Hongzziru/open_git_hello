#SQLite FTS3 vs FTS4 (3) index_relation

* content 옵션을 통한 FTS4 테이블 생성 시 오리지날 테이블과 FTS 테이블 간 인덱스 관련 정리

1. content 옵션을 통한 FTS4 생성
  * FTS4 option, [content=]를 통해 테이블을 참조하는 FTS4 테이블 생성
  ```SQL
  create table common(a, b);
  --각 테이블은 다른 .db 를 통해 생성
  --common.db
  create virtual table t using fts4(content="common");
  --common2.db
  create virtual table t using fts4(content="common", a, b);
  --common3.db
  create virtual table t using fts4(content="common", a);
  --FTS.db
  create virtual table fts using fts4(a, b);
  ```
  * .db 파일 사이즈
  ```SQL
  Common.db  : 28672
  Common2.db : 28672 
  Common3.db : 28672
  FTS.db     : 28672
  ```
  * .db 테이블 스키마
    * content 옵션 사용 .db
      * tablename_content 테이블 생성x
    * 일반 FTS 쿼리문 사용 .db
      * tablename_content 테이블 생성o
  * content 테이블 스키마
  ```SQL
   CREATE VIRTUAL TABLE abc USING fts4(a int, b text, c);
   CREATE TABLE abc_content(docid INTEGER PRIMARY KEY, c0a, c1b, c2c);
  ```
2. select 결과
 ```SQL
  create table common(a, b)
  create virtual table t using fts4(content="common")
  
  select * from common;
  select * from t;
  --5 rows returned in 0ms from:
 ```
3. match 결과
 ```SQL
 select * from t where t match 'a'
 --0 rows returned in 0ms from:
 select * from common where a like 'a'
 --1 rows returned in 0ms from:
 ```
 * t테이블(content 옵션으로 생성된 FTS테이블)은 t_docsize, t_stat, t_segdir의 데이터가 존재하지 않는다.
 * t테이블에 데이터 삽입 후 match에선 1rows가 검색된다.(이 때 검색 결과는 docid가 동일한 원본 테이블의 데이터가  검색된다.)
 ```SQL
 insert into t values('a', 'a');
 insert into t(a, b) select a, b from common;
 --constraint failed:
 --FTS table insert에선 docid를 반드시 지정해야 한다.
 
 insert into t(docid, a, b)select 1, a, b, from common where rowid = 1;
 insert into t(docid, a, b) values(1, 'a', 'a');
 --Query executed successfully:
 
 select * from t where t match 'a'
 --1 rows returned in 0ms from:
 ```
  * 데이터 삽입 후 t_docsize, t_stat, t_segdir 데이터
  ```SQL
  select * from t_docsize
  --docid size
  --  1     
  select * from t_segdir
  --level  idx  start_block  leaves_end_block  end_block root
  --0       0        0            0                0      10
  --1       0        0            0                0      10
  select * from t_stat
  --docid  size
  --  0
  ```
4. 
