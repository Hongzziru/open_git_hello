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

* AND OR NOT 은 항상 대문자로 작성해야 한다.

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


####Syntax - OR, NOT
  * NEW DATA INSERT
  ```SQL
  insert into docs values('python good software');
  insert into docs values('java is a nice');
  ```
  
  * basic OR, NOT
  ```SQL
  select * from docs where docs match 'good OR system';
  --3 rows returned:
  --"a database is a software system"
  --"sqlite is a software system"
  --"python good software"
  
  select * from docs where docs match 'good' OR content like '%nice%';
  --unable to use function MATCH in the requested context:
  
  
  select * from docs where docs match 'database NOT sqlite';
  --0 rows returned:
  select * from docs where docs match 'database -sqlite';
  --1 rows returned:
  --"a database is a software system"
  
  select * from docs where docs match 'software -sqlite -database';
  --1 rows returned:
  --"python good software"
  
  ```
  
  * UNION
  ```SQL
  select * from docs where docs match 'sqlite OR java';
  ```
  This query is equivalent to the above.
  ```SQL
  select * from docs where docs match 'sqlite'
  UNION
  select * from docs where docs match 'java';
  ```

###TEST
  
  * 괄호, ""에 의한 return 
  ```SQL
  SELECT * FROM docs WHERE docs MATCH 'sqlite OR database library';
  --0 rows returned:
  
  SELECT * FROM docs WHERE docs MATCH 'sqlite OR database system';
  --a database is a software system
  --sqlite is a software system
  
  SELECT * FROM docs WHERE docs MATCH 'java OR database system';
  SELECT * FROM docs WHERE docs MATCH 'java OR (database system)';
  --"a database is a software system"
  
  SELECT * FROM docs WHERE docs MATCH 'java OR "database system"';
  SELECT * FROM docs WHERE docs MATCH 'java OR ("database system")';
  --"java is a nice"
  
  SELECT * FROM docs WHERE docs MATCH 'java OR "database"';
  --a database is a software system
  --sqlite is a database
  --java is a nice
  ```
  
  
