# 常见的sql语句


```mysql
-- 按照条件更新数据
UPDATE table 
SET col2 = ( 
	CASE 
		WHEN col1 = 1 THEN 'A' 
		WHEN col1 = 2 THEN 'B'
	END 
) where col3 = '2.0' and col4 != 99;

```