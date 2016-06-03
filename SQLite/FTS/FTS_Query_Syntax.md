#Query Syntax - AND OR NOT

###Test Data
  ```SQL
  create virtual table docs using fts4();
  INSERT INTO docs(docid, content) VALUES(1, 'a database is a software system');
  INSERT INTO docs(docid, content) VALUES(2, 'sqlite is a software system');
  INSERT INTO docs(docid, content) VALUES(3, 'sqlite is a database');
  ```
|docid|content|
|----|----|
|1|a database is a software system|
|2|sqlite is a software system|
|3|sqlite is a database|

  
####Syntax - AND
  * 암묵적 명시
  
  ```SQL
  select * from docs where docs match 'sqlite AND database';
  --0 rows returned:
  select * from docs where docs match 'sqlite database';
  --1 rows returned:
  --sqlite is a database
  ```````
  
    >The AND operator may be implicitly specified.[link](https://www.sqlite.org/fts3.html#section_3_1)
 
  * match는 하나의 쿼리문 안에 한번만 사용
  
  ```SQL
  select * from docs where docs match 'sqlite' and docs match 'database';
  --unable to use function MATCH in the requested context:
  select * from docs where docs match 'sqlite' and content like '%database%';
  --1 rows returned:
  --sqlite is a database
  ```
  * match는 다른 where 조건과 사용 가능
  
  ```SQL
  select * from docs where docs match 'database' AND content like '%sqlite%';
  --1 rows returned:
  --sqlite is a database
  ```
  
  * INTERSECT

  ```SQL
  select * from docs where docs match 'database'
  intersect
  select * from docs where docs match 'sqlite'
  --1 rows returned:
  --sqlite is a database
  ```
    >The AND operator determines the intersection of two sets of documents.


####Syntax - OR
  * 
