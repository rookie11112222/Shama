**==批量删除表的方法==**
1. **筛选出需要删除的表名**
```
select 'drop table "'||tablename||'";'|'";'
from pg_tables 
where tablename like 'H2021%'
```
- 第一个||前面为"和'，第二个||后面为'和"
- A开头：Alarm
- CH开头：Control
- H开头：History
2. **将第一步筛选出来的结果粘贴进excel**
- 数据→分列→分隔符号→[文本识别符号]设为无
- 数据→分列→固定宽度→将最前和最后的"去除
3. **将加工好的excel数据粘贴到Postgre的Query窗口运行**


**==限制返回结果的行数==**
```
SELECT column
FROM table_name
LIMIT 3
```
- 返回前3行的数据


**==统计某个表的大小==**
```
SELECT table_schema, table_name,
pg_size_pretty(pg_total_relation_size(quote_ident(table_schema) || '.' || quote_ident(table_name))) as total_size
FROM information_schema.tables
WHERE table_name LIKE '%特定字符%'  -- 替换为你要查找的字符
AND table_schema NOT IN ('pg_catalog', 'information_schema')
ORDER BY pg_total_relation_size(quote_ident(table_schema) || '.' || quote_ident(table_name))) DESC;
```



