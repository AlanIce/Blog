SQL基础知识
======

## GROUP BY分组取字段最大值
```sql
SELECT
	t.*
FROM
	tablename t
WHERE
	val = (
		SELECT
			max(val)
		FROM
			tablename
		WHERE
			val = t.val
	)
GROUP BY
	t.val;
```