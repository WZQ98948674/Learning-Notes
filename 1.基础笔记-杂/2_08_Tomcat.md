### Tomcat

1. 服务器 :  安装了服务器软件的计算机 . 
2. 服务器软件 : 接收请求 , 处理请求 , 做出响应 . 
3. web服务器软件/web容器 : 
   - 让用户通过浏览器访问 , 接收请求 , 处理请求 , 做出响应 . 
   - 可以部署web项目 , 让用户访问
4. 常见的服务器软件 : 
   - weblogic : oracle   大型J2E服务器 , 收费 . 
   - webSphere : IBM 大型J2E服务器 , 收费 . 
   - JBoss : JBoss公司  大型J2E服务器 , 收费 . 
   - Tomcat : apache组织 中小型J2E服务器 ,  仅支持少量的J2E规范 ,  开源
5. Tomcat使用
   1. 下载 , 安装 , 卸载 ...
   2. 启动    双击startup.bat    访问localhost:8080  出现一只猫  , 启动成功
   3.  端口占用解决办法 : 
      - 杀死占用进程 :  cmd ---  netstat -ano   --- 找PID    任务管理器杀死
      - 修改自身端口号 : server.xml         
        -  一般会将Tomcat端口号修改为80 , 因为 __80 是http协议的默认端口号 , 访问时不需要输入端口号__
   4. 部署 : 
      1. 手动部署 : 直接将项目文件夹或者war包放在webapps目录下 : 
         - 访问 : ip+端口/项目名/访问的文件名
         - 缺点 : 手动 , 麻烦
      2. 配置部署 : 在conf/server.xml中 , HOST标签体中配置如下内容 
         -   <Context docBase="项目文件目录" path="/自定义访问路径"/>
         - 访问 : ip+端口/自定义访问路径/访问的文件名
         - 缺点 : 需要修改tomcat配置文件 , 不安全 
      3. 配置部署 : 在conf/catalina/localhost 目录下创建任意名称的xml文件 , 添加如下内容 :
         -   <Context docBase="项目文件目录"/>
         - 访问 : ip+端口/xml文件名/访问的文件名
   5. 动态项目
      - 目录结构 :  
        - 项目名称
          - WEB_INF目录
            - web.xml文件  :  核心配置文件
            - classes目录 : 放置字节码文件
            - lib目录 :  放置项目依赖的jar包
   6. IDEA和Tomcat的相关配置
      1. idea会为每个tomcat部署的项目单独建立一份配置文件
      2. 