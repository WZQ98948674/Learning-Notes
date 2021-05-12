## JDBC/数据库连接池/JDBCTemplate
### 1.JDBC
1. 概念 

	- JDBC概念: Java DataBase Connectivity   java数据库连接   用java语言操作数据库
	- JDBC定义了操作所有关系型数据库的规则(实际上是sun公司定义了一整套接口,每个不同的数据库厂商定义数据库驱动去实现接口,用于操作自己的数据库)

2. 使用方式

   ```java
   //1.导入驱动jar包   mysql-connect-java.jar
   //2.注册驱动  注意:mysql5之后的驱动jar包,可以不用手动注册驱动,因为jar包中写了配置文件,会自动注册驱动
   //Class.forName("com.mysql.jdbc.Driver");
   //3.获取数据库连接对象
   Connection connection = DriverManager.getConnection("jabc:mysql://localhost:3306/test", "root", "root");
   //4.创建执行sql的平台对象
   Statement statement = connection.createStatement();
   //5.定义sql
   String sql = "select * from stu";
   //6.执行sql
   ResultSet resultSet = statement.executeQuery(sql);
   //7.处理结果集
   System.out.println(resultSet);
   //8.关闭资源
   resultSet.close();
   connection.close();
   statement.close();
   ```

3. JDBC中的常用类解析
   1. DriverManager : 
      - 作用 : 1. 注册驱动(静态代码块)   	 2.获取数据库连接
   2. connection:
      - 作用 : 1. 创建执行sql的平台   	  2. 管理事务(开启,提交,回滚)
   3. statement :  (sql注入问题)
      - 作用 : 1. 执行静态sql语句并返回结果
   4. preparedStatement : 
      - 作用 : 1. 执行预编译sql语句,参数使用?占位符
   5. resultSet : 
      - 作用 : 1. 查询的结果集对象

4. JDBC控制事务
   - 使用connection对象
     - 开启事务 : setAutoCommit(false)
     - 提交事务 : commit();
     - 回滚事务 : rollback();

### 2.数据库连接池

1. 概念 : 一个存放数据库连接的容器,当系统初始化后,容器被创建,申请一些连接对象,当用户访问数据库时,从容器中获取连接对象,访问完毕后,归还给连接池.

2. 好处 : 

   1. 节约资源 
   2. 访问高效

3. 实现

   - 标准接口 : java.sql.DataSource              
     - 获取连接 :  getConnection();
     - 归还连接 : close();       从连接池中获取的对象,调用close方法,不会关闭连接,而是归还连接

   - Sun公司提供 , 由数据库厂商实现.
     - C3P0    
     - Druid    --阿里

4. C3P0 连接池使用

   1. 导包  (C3P0包+数据库驱动)
   2. 编辑配置文件
      - c3p0.properties 或者 c3p0-config.xml , 放在classpath下 , 可自动读取
      - 可在配置文件中配置多组配置,创建ComboPooledDataSource时传参进行切换
   3. 创建连接池对象  ComboPooledDataSource
   4. 获取连接 getConnection();

5. Druid 连接池使用

   1. 导包

   2. 编辑配置文件

      - properties 格式

      - 任意名称,放在任意位置,需手动读取

   3. 手动读取配置文件

   4. 使用Druid工厂获取连接池对象  DruidDataSourceFactory.createDataSource(properties);

   5. 获取连接 getConnection();

### Spring JDBC

 	1. 概念
     - Spring 框架对JDBC的简单封装 , 提供了JDBCTemplate对象 , 简化对JDBC的开发.
 	2. 使用
     1. 导包
     2. 创建JDBCTemplate对象 , 依赖于数据源DataSource
        - JdbcTemplate template = new JdbcTemplate (ds); 
     3. 调用JdbcTemplate 的方法完成CRUD操作
        - update() : 执行DML语句 , 增删改
        - queryForMap() ; 查询结果集封装为Map集合  
          * 列名作为key , 值为value , 只能查询一条记录
        - queryForList() ; 查询结果集封装为List集合 
          - List 装了很多Map集合 , 一个Map对应一条记录
        - queryForObject() ; 查询结果集封装为对象
          - 一般用于聚合函数的查询
        - query(); 查询结果集封装为JavaBean对象
          - 参数需要RowMapper , 一般使用BeanPropertyRowMapper实现类 , 完成数据到JavaBean的自动封装
          - new BeanPropertyRowMapper<类型>(类型.class);
	3. 优点
    - 简化代码 , 无需获取连接 , 无需释放资源