## MySQL 查询所有数据库占用磁盘空间大小的 SQL 语句
```sql
select TABLE_SCHEMA, concat(truncate(sum(data_length)/1024/1024,2),' MB') as data_size,
  concat(truncate(sum(index_length)/1024/1024,2),'MB') as index_size
  from information_schema.tables
    group by TABLE_SCHEMA
    order by data_length desc;
```

## MySQL 查询单个库中所有表磁盘占用大小的SQL语句
```sql
select TABLE_NAME, concat(truncate(data_length/1024/1024,2),' MB') as data_size,
  concat(truncate(index_length/1024/1024,2),' MB') as index_size
  from information_schema.tables where TABLE_SCHEMA = 'TestDB'
    group by TABLE_NAME
    order by data_length desc;
```

## Oracle 查询某一用户下所有的表占用表空间大小
```sql
select SEGMENT_NAME, TABLESPACE_NAME, sum(BYTES / 1024 / 1024) || 'M'
  from USER_extents
 where SEGMENT_TYPE = 'TABLE'
 group by SEGMENT_NAME, TABLESPACE_NAME
```

## Oracle 查询所有用户占用表空间大小
```sql
select SEGMENT_NAME, TABLESPACE_NAME, sum(BYTES / 1024 / 1024) || 'M'
  from dba_extents
 where SEGMENT_TYPE = 'TABLE'
 group by SEGMENT_NAME, TABLESPACE_NAME
```

## Oracle 查询表空间的使用情况
```sql
SELECT UPPER(F.TABLESPACE_NAME) "表空间名",
       
       　　D.TOT_GROOTTE_MB "表空间大小(M)",
       
       　　D.TOT_GROOTTE_MB - F.TOTAL_BYTES "已使用空间(M)",
       
       　　TO_CHAR(ROUND((D.TOT_GROOTTE_MB - F.TOTAL_BYTES) /
                       D.TOT_GROOTTE_MB * 100,
                       2),
                 '990.99') || '%' "使用比",
       
       　　F.TOTAL_BYTES "空闲空间(M)",
       
       　　F.MAX_BYTES "最大块(M)"
       
       　　FROM (SELECT TABLESPACE_NAME,
                      
                      　　ROUND(SUM(BYTES) / (1024 * 1024), 2) TOTAL_BYTES,
                      
                      　　ROUND(MAX(BYTES) / (1024 * 1024), 2) MAX_BYTES
                      
                      　　FROM SYS.DBA_FREE_SPACE
                      
                      　　GROUP BY TABLESPACE_NAME) F,
       
       　　 (SELECT DD.TABLESPACE_NAME,
                  
                  　　 ROUND(SUM(DD.BYTES) / (1024 * 1024), 2) TOT_GROOTTE_MB
                  
                  　　FROM SYS.DBA_DATA_FILES DD
                  
                  　　GROUP BY DD.TABLESPACE_NAME) D
       
       　　WHERE D.TABLESPACE_NAME = F.TABLESPACE_NAME
       
       　　ORDER BY 1;
```
