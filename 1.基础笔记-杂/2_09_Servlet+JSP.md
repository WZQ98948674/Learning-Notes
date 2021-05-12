### Servlet

server + applet

1. 概念 : 运行在服务器端的小程序

2. 使用 : 

   1. 创建一个类,实现servlet接口

   2. 实现抽象方法

   3. 在web.xml中配置servlet

      - ```xml
        <servlet>
            <servlet-name>demo1</servlet-name>
            <servlet-class>com.javaWeb.D04_servlet.T001_ServletDemo001</servlet-class>
        </servlet>
        
        <servlet-mapping>
            <servlet-name>demo1</servlet-name>
            <url-pattern>/demo1</url-pattern>   <!--访问资源路径-->
        </servlet-mapping>
        ```

3. 方法 : 

   1. init : servlet创建时执行 () , 只会执行一次 , 一般用于加载资源
   2. servletConfig : 获取servlet配置对象
   3. service : servlet每次被访问 , 都会执行
   4. getServletInfo : 获取servlet的一些信息
   5. destory : servlet销毁时执行(服务器正常关闭时) , 一般用于释放资源

4. 生命周期 : 

   1. 被创建 : init

      - 默认第一次被访问时 , 可以再web.xml中的servlet标签中修改 ,     

      - 负数--默认    正数--服务器启动时创建

        - ```xml
          <load-on-startup>1</load-on-startup>
          ```

      - init方法只执行一次 , servlet在内存中只存在一个对象 , 单例

        - 所以会有线程安全问题 , 尽量不要在servlet中定义成员变量 , 定义了也不要修改成员变量的值

   2. 提供服务 : service

   3. 被销毁 : destory

      - 服务器非正常关闭 , 也不会执行此方法

5. servlet配置 :  

   1.   urlPatterns路径             servlet3.0版本之后

      - 注解配置 @WebServlet (urlPatterns="/demo")   

      - 可以简写为 @WebServlet ("/demo")  
      - 可以定义多个路径   @WebServlet ({"/demo","/demo2"}) 
      - 可以配置多级路径  @WebServlet ("/demo/abc")    或  @WebServlet ("/demo/*")

6. servlet的体系结构 : 

    Servlet  接口 --->  GenericServlet   抽象类    ---> HttpServlet   抽象类

   - GenericServlet   抽象类 :  除service方法外的方法都做了空实现
   - HttpServlet  抽象类 : 对http协议的封装 , 简化操作 , 需要重写doGet() 和doPost()方法



### JSP

1. 概念 : Java  Server  Page
   
   - 既可以写java代码 , 也可以写html代码
   
2. 本质 : JSP本质就是一个servlet
   - 过程 : 
     - index.jsp    ---访问--- >       index_jsp.java (extends  HttpJspBase  extends  HttpServlet)      ---编译--->  index_jsp.class
   
3. JSP脚本 : JSP定义java代码的方式
   1. __<% code %>__   :  定义的代码在java类中的service方法中 
   2. <%! code %>  :  定义的代码在java类中的成员位置
   3. __<%=code%>__   :  定义的代码会输出到页面上
   
4. JSP指令 : 
   1. 作用 : 用于配置JSP页面 , 导入资源文件 
   2. 格式 : <%@ 指令名称  属性名1=属性值1  属性名2=属性名2%>
   3. 分类 : 
      1. page     :      配置JSP页面
         - contentType :  等同于setContentType  , 设置MIME字符集
         
         - import  :  导包
         
         - errorPage  :  当前页面发生异常后 , 自动跳转的页面
         
         - isErrorPage : 用于标识当前页面是否为错误页面 , 为true 可以使用exception内置对象
         
         - ```jsp
           <%@ page import="java.util.ArrayList" %>
           <%@ page contentType="text/html;charset=UTF-8" buffer="16kb" errorPage="T002_errorPage.jsp"%>
           ```
      2. include :      页面包含 , 导入页面的资源文件
         
         - file  :  引入jsp页面
         
         - ```jsp
           <%@include file="T003_headerPage.jsp"%>
           ```
      3. tablib    :      导入资源 , 标签库
         - prefix :  自定义前缀 
         
         - url :  引入标签库的地址
         
         - ```jsp
           <%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
           ```
   
5. JSP内置对象 : 在JSP页面可以直接使用的对象

   ​        变量名 										真实类型 										作用

   1. pageContext							PageContext								当前页面共享数据 , 获取其他8个对象
   2. request                                     HttpServletRequest                    一次请求间共享数据(转发)
   3. session                                      HttpSession                                一次会话间共享数据
   4. application                                ServletContext                            所有用户共享数据
   5. response                                   HttpServletResponse                 相应对象
   6. page                                          Object                                            当前页面的对象   this
   7. out                                             JspWriter                                       输出流对象
      - response.getWriter()会优先于out输出
   8. config                                        ServletConfig                               Servlet的配置对象
   9. exception                                  Throwable                                   异常对象



### EL表达式

1. 概念 : Expression Language 表达式语言

2. 作用：替换和简化jsp页面中java代码的编写

3. 语法：${表达式}

4. 注意 : 

   - jsp默认支持el表达式的。如果要忽略el表达式

     - 设置jsp中page指令中：isELIgnored="true" 忽略当前jsp页面中所有的el表达式

     - ```jsp
       \${表达式} ：忽略当前这个el表达式
       ```

5. 使用 : 

   1. 运算符：

      1. 算数运算符： + - * /(div) %(mod)
      2. 比较运算符： > < >= <= == !=
      3. 逻辑运算符： &&(and) ||(or) !(not)
      4. 空运算符： empty
         - 功能：用于判断字符串、集合、数组对象是否为null或者长度是否为0
         - ${empty list}:判断字符串、集合、数组对象是否为null或者长度为0
         - ${not empty str}:表示判断字符串、集合、数组对象是否不为null 并且 长度>0
   2. __获取值__
      - el表达式只能从域对象中获取值

      - 语法：

        - ${域名称.键名}：从指定域中获取指定键的值
           * 域名称：
                1. pageScope		--> pageContext
                2. requestScope 	--> request
                3. sessionScope 	--> session
                4. applicationScope --> application（ServletContext）
                   - 举例：在request域中存储了name=张三
                   - 获取：${requestScope.name}
        - ${键名}：表示依次从最小的域中查找是否有该键对应的值，直到找到为止。
   3. 获取对象 , List/Map集合的值
      1. 对象 : 
         - ${域名称.键名.属性名}
         - 本质上会去调用对象的getter方法
      2. List集合 : 
         - ${域名称.键名[索引]}
      3. Map集合 : 
         - ${域名称.键名.key名称}
         - ${域名称.键名["key名称"]}
   4. 隐式对象 : 
      - EL表达式有11个隐式对象
      - pageContext 
        - 获取jsp其他八个内置对象     ${pageContext.request}
        - ${pageContext.request.contextPath}：动态获取虚拟目录



### JSTL标签

1. 概念 : JavaServer Pages Tag Library  JSP标准标签库

   - 是由Apache组织提供的开源的免费的jsp标签

2. 作用：用于简化和替换jsp页面上的java代码

3. 使用步骤 : 

   1. 导入jstl相关jar包

   2. 引入标签库：taglib指令：  <%@ taglib %>

      - ```jsp
        <%@taglib prefix="c" uri="http://java.sun.com/jstl/core" %>
        ```

   3. 使用标签

4. 常用的JSTL标签

   1. if 标签 : 相当于java代码的if语句 

      1.  属性： 
         - test 必须属性，接受boolean表达式 
           - 如果表达式为true，则显示if标签体内容，如果为false，则不显示标签体内容
           - 一般情况下，test属性值会结合el表达式一起使用
      2. 注意：
         - c:if标签没有else情况，想要else情况，则可以再定义一个c:if标签

   2. choose标签 : 相当于java代码的switch语句

      - ```jsp
        <c:choose>
            <c:when test="${number==1}">星期1</c:when>
            <c:when test="${number==2}">星期2</c:when>
            <c:otherwise>我是Default</c:otherwise>
        </c:choose>
        ```

   3. foreach标签 : 相当于java代码的for语句

      1. 属性 : 

         - begin : 开始值

         - end : 结束值

         - step : 步长

         - var : foreach中的变量名

         - items : foreach中的容器名

         - varStatus : 循环状态 

           - count : 循环次数 , 从1开始
           - index : 容器中的元素索引 , 从0开始

         - ```jsp
           <c:forEach items="${users}" var="user" varStatus="u">
               ${u.index}   ${u.count}   
           </c:forEach>
           ```

