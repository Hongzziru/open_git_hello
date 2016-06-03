###게시판 페이징 Basic SQL

```SQL
SELECT empno, ename, hiredate
FROM (	SELECT ROWNUM rn, empno, ename, hiredate
		FROM (
				SELECT empno, ename, hiredate
    			FROM EMP
    			ORDER BY hiredate	
    	)
	)
WHERE rn BETWEEN 6 AND 10;
```
