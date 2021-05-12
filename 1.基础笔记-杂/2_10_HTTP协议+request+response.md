### HTTP协议 / Request /Response

#### HTTP协议

1. 概念 : Hyper Text Transfer Protocol  超文本传输协议

   - 传输协议 : 定义了客户端和服务端通信时 , 发送数据的格式
2. 特点 : 

   1. 基于TCP/IP的高级协议
   2. 默认端口 80 , 即设置端口号为80 , 访问时不需要加端口号
   3. 基于请求/响应模型的 : 一次请求对应一次响应
   4. 无状态的 : 每次请求之间相互独立 , 不能交互数据
3. 历史版本 : 

   - 1.0版本 : 每次请求响应都会建立一个新的连接
   - 1.1版本 : 可以复用连接 , 响应完成后不会立马断开连接 , 在一段时间内没有其他请求则断开
4. 请求消息数据格式 : 

   1. 请求行

      - 格式  :  请求方式    请求url     请求协议/版本
      -   GET      /login.html      HTTP/1.1
      -  
      - 请求方式 :  HTTP常用的请求方式有7种 , 常用的有2种 
        - __GET__ : 
          1. 请求参数在请求行中 , 在url后
          2. 请求的url长度有限制
          3. 不太安全
        - __POST__  : 
          1. 请求参数在请求体中
          2. 请求的url长度没有限制
          3. 相对安全

   2. 请求头  : 

      - 作用 : 浏览器告诉服务器一些信息

      - 格式 : 键值对              请求头名称 : 请求头值
      -  
      - 常见请求头 
        1. Host : 请求服务器地址
        2. User-Agent : 告诉服务器 , 浏览器的版本信息         --可以解决兼容性问题
        3. Accept : 告诉服务器 , 浏览器可以解析的响应格式
        4. Accept -Language : 告诉服务器 , 浏览器可以解析的语言
        5. Referer :  告诉服务器 , 当前请求从哪里来
           - 防盗链 : 我的电影网站 -- > 优酷的电影播放界面
           - 统计信息 : 统计请求来源
        6. Connection : keep-alive  可以复用

   3. 请求空行

      - 就是一行空白 , 分隔POST请求头和请求体

   4. 请求体 (正文)

      - 作用 : 封装POST请求的请求参数 , GET请求没有请求参数
5. 响应消息数据格式 : 
   1. 响应行
      - 格式 ： 协议/版本  状态码   状态码描述
      - HTTP/1.1      200      OK
      - 常见状态码 : 
        - 1XX : 服务器接收客户端的消息接收了一部分,等待一段时间之后,返回1XX
        - 200 --成功 
        - 302--重定向   304--访问缓存,没有更改情况下   
        - 4XX -- 客户端错误 :  404--路径不存在      405--请求方式没有对应的doXX方法
        - 5XX--服务器端错误 : 500--服务器内部错误
   2. 响应头
      - 常见响应头 : 	
        - content-type : 
        - content-
        - location : 重定向地址
   3. 响应空行
   4. 响应体
      - 响应的数据内容
        1. 获取输出流  :  字符/字节
        2. 使用输出流 , 传递数据



#### Request对象

1. 原理 : 

   1. Request对象和Response对象是由服务器创建的
   2. Request对象是来获取请求对象的 , Response对象是来设置响应消息的

2. 体系结构

   1. ServletRequest(接口)  --->     HttpServletRequest(接口 , 继承)  ---->  RequestFacade(类 , tomcat实现)

3. 功能

   1. 获取请求行数据

      - GET      /day14/demo1?name=zhangsan      HTTP/1.1

      1. 获取请求方式     : GET
         - String  getMethod();
      2. __获取虚拟目录__     :day14
         - String  getContextPath();
      3. 获取Servlet路径 : demo1
         - String  getServletPath();
      4. 获取get方式的请求参数 : name=zhangsan
         - String  getQueryString();
      5. __获取请求的URI__
         - String  getRequestURI :   /day14/demo1
         - String  getRequestURL  :  http://localhost/day14/demo1
         -  
         - URI和URL的区别
           - URI  : 统一资源标识符  , 范围更大              口香糖 
           - URL : 统一资源定位符                                 大大牌口香糖
      6. 获取协议及版本号 :HTTP/1.1
         - String  getProtocol()
      7. 获取客户机的IP
         - String  getRemoteAddr();

   2. 获取请求头数据

      1. 方法 : 
         - Enumeration<String>   getHeaderNames();          获取所有的请求头名称
         - __String  getHeader(String name); __           获取请求头的值

   3. 获取请求体数据

      1. 获取流对象
         - BufferReader   getReader();
         - ServletInputStream  getInputStream();
      2. 从流对象中获取数据

   4. 其他功能

      1. 获取请求参数的通用方法(GET和POST皆可)  

         -  解决中文乱码   req.setCharacterEncoding("UTF-8");

         - __String  getParameter(String name);__   根据请求参数名称获取请求参数值
         - String[]  getParameterValues();         根据请求参数名称获取请求参数值的数组
         - Enumeration<String>  getParameterNames();   获取所有请求参数名称
         - __Map<String,String[]>  getParameterMap(); __获取所有请求参数名称和请求参数值的map集合

      2. __请求转发__

         - ```
           RequestDispatcher requestDispatcher = req.getRequestDispatcher("/demo1");
           requestDispatcher.forward(req,resp);
           ```

         - 特点 : 

           1. 浏览器地址栏不会发生变化
           2. 只能访问服务器内部的资源
           3. 转发是一次请求

      3. 共享数据 

         1. 域对象 : 一个有作用范围的对象 , 可以在范围内共享数据
         2. request域 : 代表一次请求的范围 , 一般用于请求转发的多个资源中共享数据
         3. 方法 : 
            1. __void setAttribute(String name , Object value) ;__      放入键值对
            2. __Object  getAttribute(name) ;__      根据键获取值
            3. void  removeAttribute(name);     根据键移除键值对

      4. 获取ServletContext 

         1. __ServletContext getServletContext();__
   
#### Response对象

 1. 重定向方法 : 

     1. ```java
        //1.设置响应状态码
         resp.setStatus(302);
         //2.设置响应头 的 重定向地址
         resp.setHeader("location","demo7");
        ```

    2. ```java
       //简单的重定向  ,  虚拟路径建议动态获取
        resp.sendRedirect("/webtest/demo7");
       ```

	2. 输出内容到浏览器

    	1. ```java
        //输出字符数据
        //设置输出流的编码格式 , 告诉浏览器以utf-8解码 , 防止乱码
        resp.setContentType("text/html;charset=utf-8");
        PrintWriter writer = resp.getWriter();
        writer.write("你好");
        ```

    	2. ```java
        //输出字节数据
        //设置输出流的编码格式 , 告诉浏览器以utf-8解码 , 防止乱码
        resp.setContentType("text/html;charset=utf-8");
        ServletOutputStream os = resp.getOutputStream();
        os.write("中文".getBytes("utf-8"));
        ```

    3. ```java
       //设置解析文件的响应头 filename为弹出提示框内的文件名
       resp.setHeader("content-disposition","attachment;filename=a.jpg");
       ```

#### ServletContext对象

1. 概念 : 代表整个web项目 , 可以和服务器通信
2. 获取 : 
   1. 通过request对象获取
      - request.getContextServlet();
   2. 通过HttpServlet对象获取
      - this.getServletContext();
3. 功能 : 
   1. 获取MIME类型
      1. MIME格式 : 在互联网通信中的一种数据类型
      2. 格式 :          大类型/小类型         text/html        image/jpg
      3. 获取 :        String  getMimeType(String  filename);
   2. 域对象 , 共享数据
      1. 范围 : 全局 , 共享所有用户的数据 , 不安全
   3. 获取文件的真实路径
      1. String  getRealPath(path);
         - src目录下 : path = WEB-INF/classes/文件名
         - web目录下 : path = /文件名
         - WEB-INF目录下 : path = WEB-INF/文件名

#### 请求转发和重定向

 1. ##### 请求转发
     
     1. 特点 : 
        1. 浏览器地址栏不会发生变化
        2. 只能访问服务器内部的资源
        3. 转发是一次请求
  2. ##### 重定向
     
     1. 特点 : 
        1. 浏览器地址栏会发生变化
        2. 可以访问服务器外部的资源
        3. 转发是两次请求 

