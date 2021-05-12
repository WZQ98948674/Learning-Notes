### 会话技术

#### 会话

1. 概念 : 一次会话中包含多次请求和响应 . 
   - 一次会话 : 浏览器第一次给服务器发送请求 , 建立会话 , 直到有一方断开 , 会话结束
2. 功能 : 在一次会话的多次请求间 , 共享数据
3. 方式 : 
   1. 客户端会话技术 : cookie
   2. 服务端会话技术 : session

#### Cookie

1. 概念 : 客户端会话技术 , 将数据保存到客户端 .

2. 使用 : 

   1. 创建Cookie对象 , 绑定数据

      1. ```java
         Cookie cookie = new Cookie(String name, String value);
         ```

   2. 发送Cookie对象到浏览器

      1. ```java
         response.addCookie(cookie);
         ```

   3. 服务器获取Cookie对象 , 拿到数据

      1. ```java
         Cookie[] cookies = request.getCookies();
         ```

3. 原理 : 

   - 基于响应头 setCookie  和请求头 Cookie实现的 . 

   1. 第一次请求和响应中 : 服务器给客户端返回cookie , 并设置响应头 set-cookie : name=value 
   2. 之后的请求和响应中 : 客户端会把cookie自动放在请求头中 cookie : name=value , 服务器从请求中获取cookie

4. 细节 

   1. Cookie一次可以发送多个 , 也可以接收多个 . 

   2. Cookie的存储时间 :

      1. 默认情况下 : 浏览器关闭 , Cookie销毁
      2. 设置Cookie声明周期 : Cookie.setMaxAge(int)      
         - 正数 : 将Cookie存储到硬盘的文件中,持久化存储,Cookie的存活时间 , 单位 : 秒
         - 负数 : 默认值
         - 0 : 销毁Cookie

   3. Cookie存储中文 :

      1. tomcat8之前 不支持 , 需要转码和解码 , 一般采用URL编码
      2. tomcat8之后 支持 , 但是不支持特殊字符

   4. Cookie的获取范围

      1. 默认情况下 , 同一个服务器部署的其他项目中不能获取

         - 设置 : setPath(String path);   同一服务器下部署的为path的虚拟目录的项目也可以共享cookie

         - 可以把path设置为"/"

      2. 不同服务器之间的共享问题

         - ```java
           cookie.setDomain(String path);
           //设置一级域名,则可以在相同一级域名的不同服务器共享session, 例如tieba.baidu.com 和news.baidu.com
           ```

   5. 案例:T003_Cookie记录上次访问时间



#### Session

1. 概念 : 服务器端会话技术 , 在一次会话的多次请求和响应中共享数据 , 将数据保存到服务器端的对象中 , HttpSession
2. 使用 : 
   1. 获取session对象 : request.getSession();
   2. 设置session内容 : session.setAttribute(String name , Object value);
   3. 获取session内容 : session.getAttribute(String name);
   4. 移除session : session.removeAttribute(String name);
3. 如何保证一次会话中,不同servlet获取的session是同一个?
   1. session的实现是基于cookie的.
   2. 第一次获取session时 , 创建session , 响应时设置响应头  set-cookie : JSESSIONID = id;
   3. 之后在此会话中再访问其他servlet时 , 会在请求头中带着此cookie , 所以获取的一定是同一个session
4. 细节 : 
   1. 客户端关闭 , 服务器不关 , 再次获取的session是同一个么 ? 
      - 默认不是 , 因为不是一次会话
      - 可以通过自建cookie , 携带JSESSIONID , 设置cookie的过期时间更改
   2. 客户端不关 , 服务器关闭 , 再次获取的session是同一个么 ? 
      -  不是 , 服务器关闭 , session已经被销毁了 
      - 但是 , 虽然不是同一个session , 要保证session中的数据不丢失
      -  
      - session的钝化和活化
        - 钝化 : 服务器正常关闭之前 , 将session序列化      
        - 活化 : 服务器启动之后 , 将session反序列化
        - Tomcat自动钝化和活化 , 使用idea部署不行
   3. session什么时候被销毁 ? 
      1. 服务器关闭
      2. session.invalidate();
      3. 默认失效时间:30分钟 , 可以设置在 web.xml  
         - <session-config>
               <session-timeout>60</session-timeout>
             </session-config>





#### Cookie和Session的区别

 1. Cookie的特点 : 

      1. Cookie存储数据在客户端浏览器 , 不太安全 ,容易被篡改

      2. 浏览器对单个Cookie的大小有限制(4KB)

      3. 浏览器对同一域名的cooki数量也有限制(20个)
 2. Cookie的作用 : 
      1. 用于少量的不敏感的数据
      2. 在不登录的情况下,完成服务器对客户端的身份识别
 3. Session的特点 : 
      1. session用于存储一次会话的多次请求的数据 , 存在服务器
      2. session可以存储任意类型 , 任意大小的数据

4. __区别__ : 
   1. session数据存储在服务器 , cookie数据存储在客户端
   2. session 数据安全 ,cookie相对于不安全
   3. session存储数据大小没有限制 , cookie大小不能超过4KB

