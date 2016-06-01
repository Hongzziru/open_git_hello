#FTS commom table index relation

* content option을 사용해 만든 FTS 테이블과 원본 테이블의 인덱스 유무에 따른 비교

* data
  
  |a col|b col|
  |-----|-----|
  |a    |a    |
  |b    |b    |

* 인덱스 생성 전 (before create index)
  ```SQL
  -- 테이블 생성
  create table f(a text, b text);
  insert into f('a', 'a');
  insert into f('b', 'b');
  create virtual table ftsf using fts4(content="f", a, b);
  
  explain query plan select * from f where a = 'a';
  --0|0|0|SEARCH TABLE f USING INDEX f1 (a=?)
  explain query plan select * from ftsf where a= 'a';
  --0|0|0|SCAN TABLE ftsf VIRTUAL TABLE INDEX 0:
  explain query plan select * from ftsf where ftsf match 'a';
  --0|0|0|SCAN TABLE ftsf VIRTUAL TABLE INDEX 4:
  ```
* 인덱스 생성 후 (after create index)
  ```SQL
  -- create index
  create index f1 on f(a);
  create index ftsf1 on ftsf(a);
  --Error: virtual tables may not be indexed
  
  explain query plan select * from f where a = 'a';
  --0|0|0|SEARCH TABLE f USING INDEX f1 (a=?)
  explain query plan select * from ftsf where a = 'a';
  --0|0|0|SCAN TABLE ftsf VIRTUAL TABLE INDEX 0:
  explain query plan select * from ftsf where ftsf match 'a';
  --0|0|0|SCAN TABLE ftsf VIRTUAL TABLE INDEX 4:
  ```
* **결론**
  content 옵션에 따른 FTS4 테이블은 인덱스에 전혀 영향을 받지 않는다
