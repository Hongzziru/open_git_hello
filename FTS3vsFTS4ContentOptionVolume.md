
# SQLite FTS3 vs FTS4 (2) content option volume

* FTS content option에 따른 테이블 용량 비교

* FTS4 content option
  * The content option allows FTS4 to forego storing the text being indexed. The content option can be used in two ways:
    1. The indexed documents are not stored within the SQLite database at all (a "contentless" FTS4 table), or
    2. The indexed documents are stored in a database table created and managed by the user (an "external content" FTS4 table).

  * content 옵션은 index 생성을 하지 않을 수 있다.
    1. 인덱스는 SQLite db에 저장하지 않을 수 있다.
    2. 인덱스는 사용자에 의해 테이블에 생성되고 관리될 수 있다.
    

* 테이블 생성 및 데이터 삽입 
```SQL

--테이블 생성
create table c(a text, b text, c text)
create virtual table t1 using fts4(content="", a, b, c)
create virtual table t2 using fts4(content="c", a, b, c)
--데이터 삽입
insert into c values('a', 'b', 'c')
insert into t1(docid, a, b, c) values('a', 'b', 'c')
insert into t2(docid, a, b, c) values('a', 'b', 'c')
```


