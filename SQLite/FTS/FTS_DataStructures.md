#FTS Data structures

###Test Data
 * Data Schema
  
  ```SQL
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
 


###Shadow Tables
  * %_content table
   
  >사용자가 FTS테이블에 삽입한 데이터를 포함하고 있는 테이블이다. 사용자가 명시적으로 docid 값을 제공하지 않는 경우, 자동으로 docid가 지정된다.[link](https://www.sqlite.org/fts3.html#section_9_1)
   
    a. %_content table schema

    ```SQL
    -- Virtual table declaration
    CREATE VIRTUAL TABLE abc USING fts4(a NUMBER, b TEXT, c);
    -- Corresponding %_content table declaration
    CREATE TABLE abc_content(docid INTEGER PRIMARY KEY, c0a, c1b, c2c);
    ```  
    b. t_content
    
  |docid|a|b|
  |----|----|----|
  |1|a|apple is taste|
  |2|b|banana good|
  |3|c|car is fast|
  |4|d|dog cute|

  * %_stat, %_docsize table
  
  >사용자가 FTS3를 사용하는 경우 %_stat, %_docsize 테이블만이 생성된다.

    a. %_stat, %_docsize table schema
    
    ```SQL
    CREATE TABLE %_stat(
      id INTEGER PRIMARY KEY, 
      value BLOB
    );
    
    CREATE TABLE %_docsize(
      docid INTEGER PRIMARY KEY,
      size BLOB
    );
    ```
    b. docsize 테이블의 docid는 동일한 값이 대응되며 size는 사용자 정의 열의 수를 blob로 나타낸다. stat 테이블은 항상 id가 0인 단일 행이며, value는 사용자 정의 컬럼 수 +1의값이 blob로 나타낸다. value의 blob중 첫째 varint는 FTS 테이블의 총 row 수를 나타내며 두번째 varint는 테이블의 모든행에 저장된 토큰의 총 수를 나타낸다.
  
    t_docsize
    
  |docid|size|
  |----|----|
  |1||
  |2||
  |3||
  |4||
  
    t_stat
    
  |id|value|
  |----|----|
  |0| |
  
  
  
  * %_segments, %_segdir
  
  >이 두 테이블은 full-text index를 저장하기 위해 사용된다. 인덱스는 %_content 테이블의 데이터를 word 별로 docid값의 세트로 매핑되어 구성된다. FTS의 match는%_content테이블에서 검색하는 단어를 포함하는 docid 세트를 결정한다. FTS테이블이 생성될 때 두 테이블은 아래 스키마를 통해 항상 생성된다.
    
    a. %_segments, %_segdir schema

    ```SQL
    CREATE TABLE %_segments(
      blockid INTEGER PRIMARY KEY,       -- B-tree node id
      block blob                         -- B-tree node data
    );
    
    CREATE TABLE %_segdir(
      level INTEGER,
      idx INTEGER,
      start_block INTEGER,               -- Blockid of first node in %_segments
      leaves_end_block INTEGER,          -- Blockid of last leaf node in %_segments
      end_block INTEGER,                 -- Blockid of last node in %_segments
      root BLOB,                         -- B-tree root node
      PRIMARY KEY(level, idx)
    );
    ```
    
    b. 위 스키마는 FTS 인덱스를 직접 저장하지 않고, 하나 이상의 b-tree 구조를 사용한다. %_segdir 테이블은 각 행에 하나의 b-tree가 있고 root노드와 b-tree관련 메타 데이터를 가진다. %_segments 테이블은 루트가 아닌 다른 모든 b-tree의 노드를 가진다. 
