### 常用sql

```sql
--查看所有的数据库
show databases;

--使用数据库
use database01;

--查看所有表
show tables;

--查看表的详情
desc user;

--查询mysql运行时参数
show variables;
show variables like '%join_buffer%';

--查看表的所有索引
show index from user;

--给表添加索引
alter table user add index index_1(name);

--添加前缀索引 (添加city字段的前7个字节为前缀索引)
alter table user add key(city(7));


--查看sql执行时间
set profiling=1;
...sql...
show profiles;

--查询最后一条sql执行查询了多少个数据页 (进行了多少此IO)
select status like 'last_query_cost';

--查询mysql默认的排序选择参数
show variables like 'max_length_for_sort_data';
```



