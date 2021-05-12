### Redis

1. 概念 : 一款高性能的NOSQL系列的非关系型数据库 .      www.redis.net.cn  -->教程

2. 非关系型数据库的特点 : 

   1. 数据之间没有关联关系
   2. 数据存储在内存中 , 无需和硬盘做IO操作 , 速度快

3. NOSQL的优缺点 : 

   1. 优点 : 
      1. 成本 :  简易部署 , 开源 , 不需要花钱购买
      2. 速度 :  数据存储于内存中 , 不像关系型数据库存储于硬盘中 , 查询速度快
      3. 存储格式 :  存储格式是key-value形式 , 文档形式, 图片形式 ...可以存储各种类型的数据 , 关系型数据库只能存储基础类型
      4. 扩展性  :  存储数据没有关联关系 , 所以便于扩展
   2. 缺点 : 
      1. 技术较新 , 维护工具和资料有限
      2. 不像关系型数据库有统一的标准SQL 

4. 缓存思想 : 

   1. 客户端向服务器请求资源 -->
      1. 在缓存中查找资源-->
         1. 有 : 直接返回
         2. 没有 : 
            1. 去数据库查找资源
            2. 写入缓存
            3. 返回资源

5. 为什么选择用Redis 而不选择使用Map做缓存 ? 

   1. Map集合只能在一台机器上做缓存 , Redis可以做分布式的缓存
   2. Map集合使用的是JVM的内存空间做缓存 , 比较小  , Redis可以使用单独的机器 , 将全部的内存空间都用来做缓存 , 缓存的内容更多

6. Redis的应用场景 : 

   1. 缓存 
   2. 任务队列 (秒杀 , 抢购)
   3. 应用排行榜
   4. 网站访问统计
   5. 数据过期处理

7. Redis目录解析 : 

   1. redis.windows.conf      Redis配置文件
   2. redis-cli.exe                   Redis客户端
   3. redis-server.exe           Redis服务器端

8. __Redis数据结构__

   - redis存储的是 : key : value格式的数据结构 , 其中 key 都是字符串 , value有5中不同的数据结构
     1. 字符串类型 string  :                                            name --zhangsan
     2. 哈希类型 hash : map格式                                 name --   name-lisi  age-23
     3. 列表类型 list : linkedList格式                            name --   zhangsan  lisi  lisi
     4. 集合类型 set : hashSet格式                              name --  zhangsan  lisi 
     5. 有序集合类型 sortedSet :                                 name -- zhangsan  lisi

9. 命令操作 : 

   1. 字符串类型 : 

      1. 存储  set  key  value
      2. 获取  get  key
      3. 删除  del  key

   2. 哈希类型 : 

      1. 存储 : hset  key field value
      2. 获取 : hget  key field         /      hgetall   key
      3. 删除 : hdel  key field

   3. 列表类型 :  

      - 按照插入顺序进行排序 , 可以添加元素在列表的头部或者尾部 , 允许重复元素

      1. 添加 :  
         - lpush  key   value    将元素添加到列表左边
         - rpush  key   value    将元素添加到列表右边
      2. 获取 : 
         - lrange key  start  end       范围获取    end为-1 : 获取所有
      3. 删除
         - lpop  key        删除列表最左边的元素 , 并将其返回
         - rpop  key        删除列表最右边的元素 , 并将其返回

   4. 集合类型 :

      -  无序 , 不允许重复元素

      1. 存储 : 
         - sadd  key  value
      2. 获取 : 
         - smembers key     查询所有元素
      3. 删除 : 
         - srem  key  value  

   5. 有序集合类型 :

      - 元素有序 , 不允许重复元素
      - 添加时 附带score  , 按照 score从小到大排序

      1. 添加 :  
         - zadd  key  score  value
      2. 获取 : 
         - zrange  key  start  end 
         - zrange  key  start  end  withscores         附带score
      3. 删除 : 
         - zrem  key  value

   6. 通用命令 : 

      1. 获取所有key  : 
         - keys  * 
      2. 获取key的类型 : 
         - type  key
      3. 删除指定的key-value
         - del  key

10. 持久化 : 

    1. redis是一个内存数据库 , 当redis重启或者蹦了 , 数据会丢失 , 可以将redis内存中的数据持久化保存到硬盘的文件中
    2. 持久化机制 : 
       1. RDB : 默认方式 
          - 在一定间隔时间内 , 检测key的变化情况 , 然后持久化数据
          1. 修改redis.windows.conf配置文件
             - save  900   1              900s有1个key发生改变 , 持久化一次
             - save  300   10            300s有10个key发生改变 , 持久化一次
             - save   60     10000     60s有10000个key发生改变 , 持久化一次
          2. 重新启动服务器并指定配置文件名称
       2. AOF : 日志记录的方式

          - 可以记录每一条命令的操作 , 可以每一次命令操作后 , 持久化数据
          1.  修改redis.windows.conf配置文件
             - appendonly  no ---->   appendonly   yes
             - #appendfsync always  : 每一次操作都进行持久化
             - appendfsync  everysec : 每秒持久化一次
             - #appendfsync  no    :  不进行持久化
          2. 重新启动服务器并指定配置文件名称

11. Java客户端 : Jedis

    1. 概念 : 一款java操作redis数据库的工具

    2. 使用步骤 : 

       1. 导入jar包   jedis-2.7.0.jar

       2. 获取连接

          - ```java
            //如果不指定连接地址和端口号 , 默认为localhost , 6379
            Jedis jedis = new Jedis("localhost",6379);
            ```

       3. 调用方法操作数据

          - ```java
            //可以再存储数据时设置数据的超时时间 , 单位 : 秒
            jedis.setex("age",10,"23");
            ```

       4. 关闭连接

          - ```java
            jedis.close();
            ```

    3. Jedis的连接池 : JedisPool

       1. 创建连接池对象

          - ```java
            JedisPool jedisPool = new JedisPool();
            ```
            
          - ```java
            //创建时可以指定连接池的配置参数对象
            JedisPoolConfig jedisPoolConfig = new JedisPoolConfig();
            JedisPool jedisPool = new JedisPool(jedisPoolConfig,"localhost",6379);
            ```

       2. 调用方法 , 获取连接

          - ```java
            Jedis jedis = jedisPool.getResource();
            ```

       3. 使用连接 , 操作数据

          - ```java
            jedis.set("gender","true");
            ```

       4. 归还连接

          - ```java
            jedis.close();
            ```

    4. JedisPool参数配置 :

       - 参考 : jedis详细配置.properties
       
    5. JedisPool的工具类 :

       1. 加载连接池配置文件  --静态代码块
       2. 获取连接   --静态方法
       - ```java
         /**
          * JedisPool的工具类
          */
         public class JedisPoolUtil {
         
             private static JedisPool jedisPool;
         
             /**
              * 静态代码块 -- 初始化连接池配置
              */
             static {
                 //读取配置文件
                 InputStream stream = JedisPoolUtil.class.getClassLoader().getResourceAsStream("jedis.properties");
                 Properties properties = new Properties();
                 try {
                     //加载配置
                     properties.load(stream);
                 } catch (IOException e) {
                     e.printStackTrace();
                 }
                 //创建连接池配置对象
                 JedisPoolConfig config = new JedisPoolConfig();
                 //获取配置文件中的配置项
                 String host = properties.getProperty("host");
                 Integer port = Integer.parseInt(properties.getProperty("port"));
                 Integer maxTotal = Integer.parseInt(properties.getProperty("maxTotal"));
                 Integer maxIdle = Integer.parseInt(properties.getProperty("maxIdle"));
                 //将配置项设置给配置对象
                 config.setMaxIdle(maxIdle);
                 config.setMaxTotal(maxTotal);
                 //使用配置对象创建连接池对象
                 jedisPool = new JedisPool(config, host, port);
             }
         
             /**
              * 获取连接
              */
             public static Jedis getJedis(){
                 return jedisPool.getResource();
             }
         }
         ```
    
12. redis具体使用方法 : 

    1. 请求到达 
    2. 去缓存中查数据
       1. 有数据  -->直接返回数据
       2. 没数据 : 
          1. 查数据库
          2. 写入缓存
          3. 返回数据

