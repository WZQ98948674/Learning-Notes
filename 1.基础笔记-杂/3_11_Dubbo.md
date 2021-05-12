### Dubbo

1. 架构演变 : 

   1. 单体架构 : 

      - 全部功能集中在一个项目内 . 

      1. 优点 : 架构简单 , 前期开发成本低 , 开发周期短 , 适合小型项目
      2. 缺点 : 
         1. 对于大型项目不易维护
         2. 技术栈受限 , 只能使用一种语言开发
         3. 性能提升只能通过增加节点, 部署集群 (某个功能访问量大 , 其他功能也得再部署一份)

   2. 垂直架构

      - 按照业务切割 , 形成小的单体项目 , 但是切割的比较粗

      1. 优点 : 技术栈可扩展
      2. 缺点  : 
         1.  对于大型项目不易维护
         2. 性能提升只能通过增加节点, 部署集群
         3. 项目之间功能冗余 , 耦合性强

   3. SOA架构

      - 面向服务的架构 , 将重复功能或模块抽取成组件的形式 , 对外提供服务 , 在项目与服务之间使用ESB(企业服务总线)的形式作为通信桥梁 . 

      1. 优点 : 
         1. 提高开发效率
         2. 可重用性高
         3. 易于维护
      2. 缺点 : 
         1. 设计难度大 : 各个系统业务不同 , 很难确认功能/模块是否重复
         2. 抽取服务的粒度较大
         3. 系统和服务之间的耦合度高

   4. 微服务架构 :

      -  将系统服务层完全独立出来 , 抽取成一个个的微服务 , 抽取粒度更细 , 采用轻量级框架协议传输 . 

      1. 优点 : 
         1. 提高开发效率
         2. 系统性能提升方案更多
         3. 适用于互联网时代 , 产品迭代周期更短
      2. 缺点 :
         1. 服务过多 , 维护繁琐
         2. 技术成本高

2. Dubbo简介  

   - Dubbo是一款高性能的java RPC框架 , 可以与spring无缝集成 . 
   - PRC : 远程过程调用 
     - RPC并不是一个具体的技术 , 而是指整个网络远程调用过程

3. Dubbo三大核心能力:

   1. 面向接口的远程方法调用
   2. 智能容错和负载均衡
   3. 服务自动注册和发现

4. Dubbo架构

   <img src="C:\Users\user\Desktop\笔记\Dubbo原理.png" alt="Dubbo原理"  />

5. 服务注册中心

   - Zookeeper : 树形目录服务
   - ![Zookeeper架构](C:\Users\user\Desktop\笔记\Zookeeper架构.png)

6. 流程说明：

   - 服务提供者(Provider)启动时: 向 `/dubbo/com.foo.BarService/providers` 目录下写入自己的 URL 地址
   - 服务消费者(Consumer)启动时: 订阅 `/dubbo/com.foo.BarService/providers` 目录下的提供者 URL 地址。并向 `/dubbo/com.foo.BarService/consumers` 目录下写入自己的 URL 地址
   - 监控中心(Monitor)启动时: 订阅 `/dubbo/com.foo.BarService` 目录下的所有提供者和消费者 URL 地址

7. 安装Zookeeper : 

   - 安装步骤：

     第一步：安装 jdk（略）
     第二步：把 zookeeper 的压缩包（zookeeper-3.4.6.tar.gz）上传到 linux 系统
     第三步：解压缩压缩包
     ​	tar -zxvf zookeeper-3.4.6.tar.gz
     第四步：进入zookeeper-3.4.6目录，创建data目录
     ​	mkdir data
     第五步：进入conf目录 ，把zoo_sample.cfg 改名为zoo.cfg
     ​	cd conf
     ​	mv zoo_sample.cfg zoo.cfg
     第六步：打开zoo.cfg文件,  修改data属性：dataDir=/root/zookeeper-3.4.6/data

8. 启动 / 停止 Zookeeper

   - 进入Zookeeper的bin目录，启动服务命令
      ./zkServer.sh start

     停止服务命令
     ./zkServer.sh stop

     查看服务状态：
     ./zkServer.sh status

9. Dubbo管理控制台 : 

   - 我们在开发时，需要知道Zookeeper注册中心都注册了哪些服务，有哪些消费者来消费这些服务。我们可以通过部署一个管理中心来实现。其实管理中心就是一个web应用，部署到tomcat即可。

   - 安装步骤：

     （1）将资料中的dubbo-admin-2.6.0.war文件复制到tomcat的webapps目录下

     （2）启动tomcat，此war文件会自动解压

     （3）修改WEB-INF下的dubbo.properties文件，注意dubbo.registry.address对应的值需要对应当前使用的Zookeeper的ip地址和端口号

     ​	dubbo.registry.address=zookeeper://192.168.134.129:2181
     ​	dubbo.admin.root.password=root
     ​	dubbo.admin.guest.password=guest

     （4）重启tomcat

   - 使用 : 

     （1）访问http://localhost:8080/dubbo-admin-2.6.0/，输入用户名(root)和密码(root)

     （2）启动服务提供者工程和服务消费者工程，可以在查看到对应的信息

10. Dubbo相关配置说明

    1. 包扫描

       - ```xml
         <dubbo:annotation package="com.itheima.service" />
         ```

       - 服务提供者和服务消费者都需要配置，表示包扫描，作用是扫描指定包(包括子包)下的类。

    2. 协议 : 

       - ```xml
         <dubbo:protocol name="dubbo" port="20880"/>
         ```

       - 一般在服务提供者一方配置，可以指定使用的协议名称和端口号。

       - 其中Dubbo支持的协议有：dubbo、rmi、hessian、http、webservice、rest、redis等。

       - 推荐使用的是dubbo协议。

         dubbo 协议采用单一长连接和 NIO 异步通讯，适合于小数据量大并发的服务调用，以及服务消费者机器数远大于服务提供者机器数的情况。不适合传送大数据量的服务，比如传文件，传视频等，除非请求量很低。

       - ```xml
         <!-- 多协议配置 不建议使用xml配置-->
         <dubbo:protocol name="dubbo" port="20880" />
         <dubbo:protocol name="rmi" port="1099" />
         <!-- 使用dubbo协议暴露服务 -->
         <dubbo:service interface="com.itheima.api.HelloService" ref="helloService" protocol="dubbo" />
         <!-- 使用rmi协议暴露服务 -->
         <dubbo:service interface="com.itheima.api.DemoService" ref="demoService" protocol="rmi" /> 
         ```

       - 可以在@Service(protocol = "rmi") 上指定协议方式

    3. 启动时检查

       - ```XML
         <dubbo:consumer check="false"/>
         ```

       - 上面这个配置需要配置在服务消费者一方，如果不配置默认check值为true。Dubbo 缺省会在启动时检查依赖的服务是否可用，不可用时会抛出异常，阻止 Spring 初始化完成，以便上线时，能及早发现问题。可以通过将check值改为false来关闭检查。

       - 建议在开发阶段将check值设置为false，在生产环境下改为true。

    4. 负载均衡

       - 负载均衡（Load Balance）：其实就是将请求分摊到多个操作单元上进行执行，从而共同完成工作任务。

       - 在集群负载均衡时，Dubbo 提供了多种均衡策略（包括随机、轮询、最少活跃调用数、一致性Hash），缺省为random随机调用。

       - 配置负载均衡策略，既可以在服务提供者一方配置，也可以在服务消费者一方配置，如下：

         ```java
         @Controller
         @RequestMapping("/demo")
         public class HelloController {
         	//在服务消费者一方配置负载均衡策略
         	@Reference(check = false,loadbalance = "random")
         	private HelloService helloService;
         
             @RequestMapping("/hello")
             @ResponseBody
             public String getName(String name){
             //远程调用
             String result = helloService.sayHello(name);
             System.out.println(result);
             return result;
             }
         }
         ```

         ```java
         //在服务提供者一方配置负载均衡
         @Service(loadbalance = "random")
         public class HelloServiceImpl implements HelloService {
             public String sayHello(String name) {
                 return "hello " + name;
             }
         }
         ```

11. 解决Dubbo无法发布被事务代理的Service问题

    前面我们已经完成了Dubbo的入门案例，通过入门案例我们可以看到通过Dubbo提供的标签配置就可以进行包扫描，扫描到@Service注解的类就可以被发布为服务。

    但是我们如果在服务提供者类上加入@Transactional事务控制注解后，服务就发布不成功了。原因是事务控制的底层原理是为服务提供者类创建代理对象，而默认情况下Spring是基于JDK动态代理方式创建代理对象，而此代理对象的完整类名为com.sun.proxy.$Proxy42（最后两位数字不是固定的），导致Dubbo在发布服务前进行包匹配时无法完成匹配，进而没有进行服务的发布。

    - 解决方案 : 

      （1）修改applicationContext-service.xml配置文件，开启事务控制注解支持时指定proxy-target-class属性，值为true。其作用是使用cglib代理方式为Service类创建代理对象

      - ```xml
        <!--开启事务控制的注解支持-->
        <tx:annotation-driven transaction-manager="transactionManager" proxy-target-class="true"/>
        ```

      （2）修改HelloServiceImpl类，在Service注解中加入interfaceClass属性，值为HelloService.class，作用是指定服务的接口类型
      
      - ```java
        @Service(interfaceClass = HelloService.class)
        @Transactional
        public class HelloServiceImpl implements HelloService {
            public String sayHello(String name) {
                return "hello " + name;
            }
        }
        ```
    

12.Dubbo快速入门

    Dubbo作为一个RPC框架，其最核心的功能就是要实现跨网络的远程调用。本小节就是要创建两个应用，一个作为服务的提供方，一个作为服务的消费方。通过Dubbo来实现服务消费方远程调用服务提供方的方法。
    
    12.1 服务提供方开发
    
    开发步骤：
    
    （1）创建maven工程（打包方式为war）dubbodemo_provider，在pom.xml文件中导入如下坐标
    
    ~~~xml
    <properties>
      <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
      <maven.compiler.source>1.8</maven.compiler.source>
      <maven.compiler.target>1.8</maven.compiler.target>
      <spring.version>5.0.5.RELEASE</spring.version>
    </properties>
    <dependencies>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-beans</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jms</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context-support</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <!-- dubbo相关 -->
      <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>dubbo</artifactId>
        <version>2.6.0</version>
      </dependency>
      <dependency>
        <groupId>org.apache.zookeeper</groupId>
        <artifactId>zookeeper</artifactId>
        <version>3.4.7</version>
      </dependency>
      <dependency>
        <groupId>com.github.sgroschupf</groupId>
        <artifactId>zkclient</artifactId>
        <version>0.1</version>
      </dependency>
      <dependency>
        <groupId>javassist</groupId>
        <artifactId>javassist</artifactId>
        <version>3.12.1.GA</version>
      </dependency>
      <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.47</version>
      </dependency>
    </dependencies>
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>2.3.2</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
        <plugin>
          <groupId>org.apache.tomcat.maven</groupId>
          <artifactId>tomcat7-maven-plugin</artifactId>
          <configuration>
            <!-- 指定端口 -->
            <port>8081</port>
            <!-- 请求路径 -->
            <path>/</path>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ~~~
    
    （2）配置web.xml文件
    
    ~~~xml
    <!DOCTYPE web-app PUBLIC
     "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
     "http://java.sun.com/dtd/web-app_2_3.dtd" >
    <web-app>
      <display-name>Archetype Created Web Application</display-name>
      <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext*.xml</param-value>
      </context-param>
      <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
      </listener>
    </web-app>
    
    ~~~
    
    （3）创建服务接口
    
    ~~~java
    package com.itheima.service;
    public interface HelloService {
        public String sayHello(String name);
    }
    ~~~
    
    （4）创建服务实现类
    
    ~~~java
    package com.itheima.service.impl;
    import com.alibaba.dubbo.config.annotation.Service;
    import com.itheima.service.HelloService;
    
    @Service
    public class HelloServiceImpl implements HelloService {
        public String sayHello(String name) {
            return "hello " + name;
        }
    }
    ~~~
    
    注意：服务实现类上使用的Service注解是Dubbo提供的，用于对外发布服务
    
    （5）在src/main/resources下创建applicationContext-service.xml 
    
    ~~~xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
    		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    	    xmlns:p="http://www.springframework.org/schema/p"
    		xmlns:context="http://www.springframework.org/schema/context"
    		xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
    	    xmlns:mvc="http://www.springframework.org/schema/mvc"
    		xsi:schemaLocation="http://www.springframework.org/schema/beans
    		http://www.springframework.org/schema/beans/spring-beans.xsd
             http://www.springframework.org/schema/mvc
             http://www.springframework.org/schema/mvc/spring-mvc.xsd
             http://code.alibabatech.com/schema/dubbo
             http://code.alibabatech.com/schema/dubbo/dubbo.xsd
             http://www.springframework.org/schema/context
             http://www.springframework.org/schema/context/spring-context.xsd">
    	<!-- 当前应用名称，用于注册中心计算应用间依赖关系，注意：消费者和提供者应用名不要一样 -->
    	<dubbo:application name="dubbodemo_provider" />
    	<!-- 连接服务注册中心zookeeper ip为zookeeper所在服务器的ip地址-->
    	<dubbo:registry address="zookeeper://192.168.134.129:2181"/>
    	<!-- 注册  协议和port   端口默认是20880 -->
    	<dubbo:protocol name="dubbo" port="20881"></dubbo:protocol>
    	<!-- 扫描指定包，加入@Service注解的类会被发布为服务  -->
    	<dubbo:annotation package="com.itheima.service.impl" />
    </beans>
    ~~~
    
    （6）启动服务
    
    tomcat7:run
    
    12.2 服务消费方开发
    
    开发步骤：
    
    （1）创建maven工程（打包方式为war）dubbodemo_consumer，pom.xml配置和上面服务提供者相同，只需要将Tomcat插件的端口号改为8082即可
    
    （2）配置web.xml文件
    
    ~~~xml
    <!DOCTYPE web-app PUBLIC
     "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
     "http://java.sun.com/dtd/web-app_2_3.dtd" >
    <web-app>
      <display-name>Archetype Created Web Application</display-name>
      <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- 指定加载的配置文件 ，通过参数contextConfigLocation加载 -->
        <init-param>
          <param-name>contextConfigLocation</param-name>
          <param-value>classpath:applicationContext-web.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
      </servlet>
      <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>*.do</url-pattern>
      </servlet-mapping>
    </web-app>
    ~~~
    
    （3）将服务提供者工程中的HelloService接口复制到当前工程
    
    （4）编写Controller
    
    ~~~java
    package com.itheima.controller;
    import com.alibaba.dubbo.config.annotation.Reference;
    import com.itheima.service.HelloService;
    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.ResponseBody;
    
    @Controller
    @RequestMapping("/demo")
    public class HelloController {
        @Reference
        private HelloService helloService;
    
        @RequestMapping("/hello")
        @ResponseBody
        public String getName(String name){
            //远程调用
            String result = helloService.sayHello(name);
            System.out.println(result);
            return result;
        }
    }
    ~~~
    
    注意：Controller中注入HelloService使用的是Dubbo提供的@Reference注解
    
    （5）在src/main/resources下创建applicationContext-web.xml
    
    ~~~xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
    	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    	xmlns:p="http://www.springframework.org/schema/p"
    	xmlns:context="http://www.springframework.org/schema/context"
    	xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
    	xmlns:mvc="http://www.springframework.org/schema/mvc"
    	xsi:schemaLocation="http://www.springframework.org/schema/beans
    			http://www.springframework.org/schema/beans/spring-beans.xsd
    			http://www.springframework.org/schema/mvc
    			http://www.springframework.org/schema/mvc/spring-mvc.xsd
    			http://code.alibabatech.com/schema/dubbo
    			http://code.alibabatech.com/schema/dubbo/dubbo.xsd
    			http://www.springframework.org/schema/context
    			http://www.springframework.org/schema/context/spring-context.xsd">
    
    	<!-- 当前应用名称，用于注册中心计算应用间依赖关系，注意：消费者和提供者应用名不要一样 -->
    	<dubbo:application name="dubbodemo-consumer" />
    	<!-- 连接服务注册中心zookeeper ip为zookeeper所在服务器的ip地址-->
    	<dubbo:registry address="zookeeper://192.168.134.129:2181"/>
    	<!-- 扫描的方式暴露接口  -->
    	<dubbo:annotation package="com.itheima.controller" />
    </beans>
    ~~~
    
    （6）运行测试
    
    tomcat7:run启动
    
    在浏览器输入http://localhost:8082/demo/hello.do?name=Jack，查看浏览器输出结果


​    

    **思考一：**上面的Dubbo入门案例中我们是将HelloService接口从服务提供者工程(dubbodemo_provider)复制到服务消费者工程(dubbodemo_consumer)中，这种做法是否合适？还有没有更好的方式？
    
    **答：**这种做法显然是不好的，同一个接口被复制了两份，不利于后期维护。更好的方式是单独创建一个maven工程，将此接口创建在这个maven工程中。需要依赖此接口的工程只需要在自己工程的pom.xml文件中引入maven坐标即可。
    
    **思考二：**在服务消费者工程(dubbodemo_consumer)中只是引用了HelloService接口，并没有提供实现类，Dubbo是如何做到远程调用的？
    
    **答：**Dubbo底层是基于代理技术为HelloService接口创建代理对象，远程调用是通过此代理对象完成的。可以通过开发工具的debug功能查看此代理对象的内部结构。另外，Dubbo实现网络传输底层是基于Netty框架完成的。
    
    **思考三：**上面的Dubbo入门案例中我们使用Zookeeper作为服务注册中心，服务提供者需要将自己的服务信息注册到Zookeeper，服务消费者需要从Zookeeper订阅自己所需要的服务，此时Zookeeper服务就变得非常重要了，那如何防止Zookeeper单点故障呢？
    
    **答：**Zookeeper其实是支持集群模式的，可以配置Zookeeper集群来达到Zookeeper服务的高可用，防止出现单点故障。