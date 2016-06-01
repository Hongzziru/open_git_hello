
# SQLite FTS3 vs FTS4 (2) content option volume

* FTS content option에 따른 테이블 용량 비교

* FTS4 content option
>  * The content option allows FTS4 to forego storing the text being indexed. The content option can be used in two ways:
>    1. The indexed documents are not stored within the SQLite database at all (a "contentless" FTS4 table), or
>    2. The indexed documents are stored in a database table created and managed by the user (an "external content" FTS4 table). [link](https://www.sqlite.org/fts3.html#section_6_2)

> * content 옵션은 index 생성을 하지 않을 수 있다.
>   1. 인덱스는 SQLite db에 저장하지 않을 수 있다.
>   2. 인덱스는 사용자에 의해 테이블에 생성되고 관리될 수 있다.


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
* 결과
```SQL

--데이터가 들어가 있는 FTS4 테이블의 DB 사이즈(contentles)
Common.db : 151552

--데이터가 들어가 있는 일반 테이블의 DB 사이즈(content 생성 전)
Common.db : 131072

--데이터가 있는 일반 테이블에서 content 옵션을 통해 FTS4테이블을 생성하고
--FTS4 테이블에 insert into t1(docid, a, b, c) values() 까지 하고 난 후의 상태
Common.db : 278528

--데이터가 있는 일반 테이블에서 content 옵션을 통해 FTS4테이블을 생성하고
--insert는 하지 않은 상태
Common.db : 151552

```

