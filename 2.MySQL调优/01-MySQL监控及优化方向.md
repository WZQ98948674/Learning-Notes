MySQL--01

#### 1.MySQL层次:



#### 2.性能监控(简单,即将废弃)

```sql
--设置性能监控
set profiling=1;
--查询所有sql的执行时间
show profiles;
--查询最近一条sql的详细执行时间
show profile;
--查询指定query_id的sql语句的详细执行时间
show profile for query 2;
--可指定参数 type :all, cup , io ,lock....
```

#### 3.性能监控(复杂)

```sql
--设置性能监控, 默认开启, 如果关闭需要去MySQL配置文件中修改, 基本没必要关闭
show variables like 'performance_schema';

```

```java
--修改performance_schema的方法
    1.找到配置文件位置
   	 	Linux下:etc/my.cnf
    	Windows下:安装目录/my.ini
    2.修改
    	找到performance_schema=OFF改为performance_schema=ON
        如果没有,则在配置文件末尾新增performance_schema=ON
    3.重启mysql服务
        Linux下:
		Windows下:管理员打开cmd    net stop mysql   net start mysql   
```

#### 4.连接监控

```sql
--使用比较少,因为基本都用连接池,连接不会出问题
show processList;
```

#### 5.优化方向

##### 1.数据类型优化

原则:

​	1.更小的通常更好 : 选择合适的最小类型

​	2.简单就好 : 选择最合适的数据类型(日期-date ip-整型) 

```sql
--ip转整数
select INET_ATON('192.168.61.54');
--整数转ip
select INET_NTOA('323365458');
```

​	3.进行避免使用null : 数据库中null不等于null ,比较,优化都很麻烦

​	4.数据类型间的区别:

​		(1).char和**varchar**的区别

​		(2).dateTime和**timeStamp**和date

​		(3).数据库枚举代替字符串

##### 2.范式与反范式

范式: 

1. 优点: 

   - 更新速度通常比反范式快

   - 数据不冗余

   - 数据可以放在内存中,操作比较快

2. 缺点

   - 需要进行关联查询

反范式:

1. 优点:
   - 所有数据都设计在一张表中,无需进行关联
   - 可以设计有效的索引
2. 缺点
   - 数据存在冗余
   - 冗余数据同步更新,否则容易出现数据不一致

##### 3.主键的选择

1. 代理主键 : 与业务无关,没有实际意义的数字       如:id

2. 自然主键 : 事务属性中的自然唯一标识 , 即业务主键    如:身份证号,订单号

   **推荐使用代理主键**

##### 4.存储引擎的选择

​	**补充图片**

1. MyISAM
2. InnoDB

##### 5.适当的数据冗余

​	被频繁的使用并且只能通过join两张或以上的大表才能得到的独立小字段,该做数据冗余.

原因: 由于每次join只为了取得某个小字段的值,join的记录又大,会造成大量不必要的IO,完全可以通过空间换时间的方式,不过冗余的数据需要同步更新,防止数据一致性被破坏.

##### 6.适当的拆分

 表中存在类似TEXT或者很大的VARCHAR或者很多不常用的字段, 如果我们大部分访问都不需要这些字段 , 就该拆分其到独立的表中 , 以减少常用数据所占用的存储空间 . 

好处: 明显减少每个数据块(页) 可以存储的数据条数大大增加 , 既减少物理IO次数 , 也能大大提高内存中的缓存命中率 .

