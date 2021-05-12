## MySQL多表查询

```mysql
CREATE TABLE dept(
	id INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(20)
);

INSERT INTO dept (NAME) VALUES ('开发部'),('市场部'),('财务部');

CREATE TABLE emp(
	id INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(10),
	gender CHAR(1),
	salary DOUBLE,
	join_date DATE,
	dept_id INT,
	FOREIGN KEY (dept_id) REFERENCES dept (id)
);

INSERT INTO emp (NAME,gender,salary,join_date,dept_id) VALUES ('孙悟空','男',7200,'2013-02-24',1);
INSERT INTO emp (NAME,gender,salary,join_date,dept_id) VALUES ('猪八戒','男',3600,'2010-12-02',2);
INSERT INTO emp (NAME,gender,salary,join_date,dept_id) VALUES ('唐僧','男',9000,'2008-08-08',2);
INSERT INTO emp (NAME,gender,salary,join_date,dept_id) VALUES ('白骨精','女',5000,'2015-10-07',3);
INSERT INTO emp (NAME,gender,salary,join_date,dept_id) VALUES ('蜘蛛精','女',4500,'2011-03-14',1);
```

### 内连接

1. 隐式内连接

   ```mysql
   SELECT e.`id`,e.`name`,d.`name` FROM dept d ,emp e WHERE d.`id` = e.`dept_id`;
   ```

2. 显式外连接(INNER可以删除)

   ```mysql
   SELECT e.id,e.name,e.gender,d.name FROM dept d INNER JOIN emp e ON d.id = e.dept_id;
   ```
### 外连接
1. 左外

   ```mysql
   INSERT INTO emp (NAME,gender,salary,join_date,dept_id) VALUES ('白龙马','男',3000,2020-0813,NULL);
   
   SELECT e.*,d.`id` FROM emp e LEFT JOIN dept d ON d.`id` = e.`dept_id`;
   ```

2. 右外

   ```mysql
   SELECT e.*,d.`id` FROM emp e RIGHT JOIN dept d ON d.`id` = e.`dept_id`;
   ```

### 对比

1. 内连接和外连接的区别:

​		内连接是查询出多表之间的交集

​		左外连接是查出左表的全部数据及两张表的交集

​		右外连接是查出右表的全部数据及两张表的交集

2. 内连接和外连接的相同点:
   1. 去除多表查询的笛卡尔积

### 子查询

- 查询中嵌套查询

	```mysql
--查询工资最高的人  								(查询结构为单行单列,使用运算符< > = ...)
SELECT * FROM emp e WHERE e.`salary` = (
	SELECT MAX(salary) FROM emp 
)
	
	--查询财务部和市场部的人员信息						(查询结果为多行单列,使用运算符IN)
	SELECT * FROM emp WHERE emp.`dept_id` IN (
		SELECT id FROM dept WHERE dept.`name` = '财务部' OR dept.`name` = '市场部'
	)
	
	--查询入职时间晚于2011-11-11的员工信息及其部门信息		 (查询结果为多行多列,可以将其作为一张虚拟表进行查询)
	SELECT a.* , dept.`name` FROM (
		SELECT * FROM emp WHERE emp.`join_date` > '2011-11-11'
	) a  , dept 
	WHERE a.dept_id = dept.`id`
	
	--用普通的内连接也可以查询
	SELECT e.* ,d.name FROM emp e INNER JOIN dept d ON e.`dept_id` = d.id AND e.`join_date`>'2011-11-11'
	
	```

### 自关联

 - 查询同一张表两次,起别名

   ```mysql
   
   ```




## 事务

1. mysql默认自动提交事务,y一条DML语句就会提交一次事务

2. oracle数据库默认手动提交,每条修改语句后都要手动commit

3. 如果需要手动提交,需要先手动开启事务

4. ```mysql
   --开启事务
   START TRANSACTION;
   
   --出现问题，回滚事务
   ROLLBACK;
   
   --执行成功，提交事务
   COMMIT;
   ```

4. 查看事务提交方式

   ```mysql
   SELECT @@autocommit;
   
   =1 :自动提交
   =0 :手动提价
   ```

5. 设置事务提交方式

   ```mysql
   SET @@autocommit = 0;
   ```

### 事务的四大特征

1. 原子性 : 不可分割的最小操作单位,要么同时成功,要么同时失败
2. 持久性: 事务提交或回滚后,数据会持久化保存到数据库
3. 隔离性 : 多个事务之间相互独立
4. 一致性 : 事务操作前后,数据总量不会改变

### 事务的隔离级别

- 概念 : 多个事物之间相互隔离,相互独立,但是如果多个事务同时操作同一数据,就会引发一些问题,设置不同的事务隔离级别可以解决此问题.
- 问题 : 
   	1. 脏读
   	2. 不可重复读
   	3. 幻读
- 隔离级别 : 
  1. read uncommited : 读未提交  
     - 问题:脏读/不可重复读/幻读
  2. read commited : 读已提交   (oracle默认隔离级别)
     - 问题 : 不可重复读/幻读
  3. repeatable read: 可重复读    (mysql默认隔离级别)
     - 问题 : 幻读
  4. serializable : 串行化
     - 问题 : 无

- 设置隔离级别:

  ```mysql
  --查询隔离级别
  SELECT @@tx_isolation;
  --设置隔离级别   (设置完需要断开连接才会生效)
  SET GLOBAL TRANSACTION ISOLATION LEVEL 级别字符串;
  ```

  