### Maven

1. 概念 : 一款强大的__项目管理__工具 apache

2. 功能 (项目管理) : 

   1. __管理jar包 __

      - 解决jar包之间的依赖冲突
      - 极大地减少项目所占磁盘空间的大小(jar包不在项目中存储 , 根据jar包的坐标去仓库中找到jar包 , jar包可重用)

   2. __项目构建__ :  编译 , 测试 , 运行 , 打包 , 安装 , 部署....

      - 内部集成了tomcat

      - 无需打包 , 直接 __mvn tomcat:run__  指令 , 即可启动项目

   3. 自动运行单元测试

3. maven目录结构 

   1. bin : mvn命令 , 用于构建项目
   2. boot : 自身运行所需要的类加载器
   3. conf : __settings.xml__  maven的配置文件
   4. lib : maven依赖的jar包

4. 环境变量配置 : 

   1. 新建MAVEN_HOME  , 路径为maven目录的路径
   2. path新增  %MAVEN_HOME%\bin
   3. 查看是否成功 : cmd --> __mvn -v__  查看maven版本信息

5. 仓库分类 : 

   1. 本地仓库  
   2. 远程仓库(公司私服)
   3. 中央仓库
   - 先从本地仓库找jar包 , 如果没有 , 去公司私服找 , 如果私服中也没有 , 则去中央仓库下载到私服 , 再下载到本地 
   - 私服中的jar包 可能从中央仓库下载而来 , 也可以从本地仓库上传而来
   
6. 本地仓库  settings.xml: 

   1. 默认 : Default: ${user.home}/.m2/repository
   2. 自定义 : <localRepository>D:\maven_repository</localRepository>
   
7. maven项目的标准目录结构:

   1. src/main/java : 核心代码
   2. src/main/resources  : 配置文件
   3. src/main/webapp : 页面资源 js css 图片
   4. src/test/java : 测试代码
   5. src/test/resources : 测试配置

8. maven常用命令 : 

   1. clean  :  清除编译信息 , 删除target目录
   2. compile : 编译核心代码 , 生成target目录
   3. test :  编译核心代码和测试代码
   4. package : 编译核心代码和测试代码 , 并生成war包 (pom中配置)
   5. insatll : 编译核心代码和测试代码 , 并生成war包 (pom中配置) , 并把项目当成jar包 , 放置到本地仓库

9. maven生命周期 : 

   1. 默认生命周期 : compile ->  test -> package -> install -> depoly
      - 后面的命令包括前面的命令
   2. 清理生命周期 : clean
   3. 站点生命周期 : 

10. idea集成maven : 

    1. 安装maven , 添加环境变量
    2. idea ->setting  -> maven ->设置maven安装位置及setting文件位置 -> Runner -> 设置参数 -DarchetypeCatalog=internal

11. 解决maven中编译依赖jar包和运行依赖jar包的冲突: 

    - ```xml
      <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>servlet-api</artifactId>
        <version>2.5</version>
          <!--表示此依赖只在编译时生效-->
        <scope>provided</scope>
      </dependency>
      ```

    - scope作用域的取值

      1. compile   :  编译-测试-运行          例如:spring-core
      2. test          :  测试 - 运行                 例如 : JUnit
      3. provided : 编译-测试                   例如 : servlet , jsp
      4. runtime   : 测试 - 运行                 例如 :JDBC驱动
      5. system    : 编译 - 测试                  例如 : 本地的 , maven仓库之外的类库

12. 配置tomcat插件 : Live Template

    - ```xml
      <build>
        <plugins>
          <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.2</version>
            <configuration>
              <port>9001</port>
            </configuration>
          </plugin>
        </plugins>
      </build>
      ```

13. 配置JDK : Live Template

    - ```xml
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <configuration>
          <target>1.8</target>
          <source>1.8</source>
          <encoding>utf-8</encoding>
        </configuration>
      </plugin>
      ```

