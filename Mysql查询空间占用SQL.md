## 查询所有数据库占用磁盘空间大小的 SQL 语句
```sql
select TABLE_SCHEMA, concat(truncate(sum(data_length)/1024/1024,2),' MB') as data_size,
  concat(truncate(sum(index_length)/1024/1024,2),'MB') as index_size
  from information_schema.tables
    group by TABLE_SCHEMA
    order by data_length desc;
```

## 查询单个库中所有表磁盘占用大小的SQL语句
```sql
select TABLE_NAME, concat(truncate(data_length/1024/1024,2),' MB') as data_size,
  concat(truncate(index_length/1024/1024,2),' MB') as index_size
  from information_schema.tables where TABLE_SCHEMA = 'TestDB'
    group by TABLE_NAME
    order by data_length desc;
```
