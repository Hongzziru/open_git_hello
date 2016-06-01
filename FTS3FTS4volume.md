#SQLite FTS3 vs FTS4 (1) volume

* SQLite의 일반테이블,  FTS3, FTS4 의 용량 차이 테스트
  각 테이블의 스키마를 동일하게 생성한 뒤 데이터 또한 동일하게 삽입. 각 테이블의 용량을 확인하기 위한 테스트

* sqlite3 version - 2.4.1

1. common, FTS3, FTS4 db 타입 없이 스키마 생성
```python

import sqlite3
​
# FTS3.db, FTS4.db 파일도 동일하게 생성.
con = sqlite3.connect('ordinary.db')
​
# FTS3,FTS4도 동일하게 생성.
con.execute('create table test(a, b, c, d)')
​
# 50000 row 의 데이터 insert
for N in range(50000):
    sql = 'insert into test values('aN', 'bN', 'cN', 'dN'))
    con.execute(sql)
​
con.commit()
```
2. common, FTS3, FTS4 db 타입 존재 스키마 생성
```python

import sqlite3
​
# 일반, FTS3, FTS4 - TYPE O/X .db 생성
con = sqlite3.connect('ordinaryTypeO.db')
​
# TYPE O/X table 생성
con.execute("create table test (a text, b integer, c text, d text)")
#con.execute("create table test (a, b, c, d)")
​
# 50000 row 데이터 insert
for N in range(50000):
    sql = 'insert into test values("aN", "bN", "cN", "dN")'
    con.execute(sql)
​
con.commit()
```
3. 결과
  용량 크기 순서
  FTS4 > FTS3 > 일반 테이블
  타입 유무에 따른 용량 변화 없음
```
ordinary Type O size :  1712128
ordinary Type X size :  1712128
FTS3 Type O size :  4161536
FTS3 Type X size :  4161536
FTS4 Type O size :  4808704
FTS4 Type X size :  4808704
```
  .db  스키마

