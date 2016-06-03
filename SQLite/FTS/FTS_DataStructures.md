#FTS Data structures

###Test Data

  * ```SQL
  create virtual table t using fts4(a, b);
  insert into t values('a', 'apple is taste');
  insert into t values('b', 'banana good');
  insert into t values('c', 'car is fast');
  insert into t values('d', 'dog cute');
  ```
  |a|b|
  |----|----|
  |a|apple is taste|
  |b|banana good|
  |c|car is fast|
  |d|dog cute|
  
#tt

###Shadow Tables

  * %_content table
    >The %_content table contains the unadulterated data inserted by the user into the FTS virtual table by the user. If the user does not explicitly supply a "docid" value when inserting records, one is selected automatically by the system.
    
    ```SQL
    -- Virtual table declaration
    CREATE VIRTUAL TABLE abc USING fts4(a NUMBER, b TEXT, c);
    
    -- Corresponding %_content table declaration
    CREATE TABLE abc_content(docid INTEGER PRIMARY KEY, c0a, c1b, c2c);
    ```
  
