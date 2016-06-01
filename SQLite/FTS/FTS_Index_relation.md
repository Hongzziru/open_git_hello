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
> content 테이블 스키마
> CREATE VIRTUAL TABLE abc USING fts4(a int, b text, c);
> CREATE TABLE abc_content(docid INTEGER PRIMARY KEY, c0a, c1b, c2c);


>asd
2. aa
  
