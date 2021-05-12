### Filter 过滤器

1. 过滤器的作用 : 

   - 一般用于通用的操作 : 登录验证 , 统一编码处理 , 敏感字符过滤

2. 使用 : 

   1. 定义过滤器实现Filter

   2. 实现方法

   3. 配置拦截路径

      - web.xml

        ```xml
        <filter>
            <filter-name>filterDemo1</filter-name>
            <filter-class>com.web.servlet.filter.T001_myFilter01</filter-class>
        </filter>
        <filter-mapping>
            <filter-name>filterDemo1</filter-name>
            <!--拦截路径-->
            <url-pattern>/*</url-pattern>
        </filter-mapping>
        ```

      - 注解配置   

        ```java
        @WebFilter("/*")
        ```

   4. 放行 : 

      1. ```java
         @Override
             public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
                 //此行代码代表放行 , 请求可以继续向下访问
                 filterChain.doFilter(servletRequest,servletResponse);
             }
         ```

3. 过滤器的执行流程

   1. 执行过滤器的doFilter()方法放行前的代码  (对request对象请求消息增强)
   2. 执行请求资源的代码
   3. 执行过滤器的doFilter()方法放行后的代码  (对response对象响应消息增强)

4. 过滤的生命周期

   1. init() : 服务器启动 , 创建Filter对象时调用                   执行一次  (加载资源)
   2. destroy()  :  服务器正常关闭 , 会调用此方法              执行一次  (释放资源)
   3. doFilter()  : 请求每次被拦截 , 都会执行                       执行多次

5. 配置详解 : 

   1. 拦截路径配置 : 

      1. 具体资源路径  :     /index.jsp        只有访问index.jsp时才会被拦截
      2. 具体目录 ;           /user/*               访问user目录下的所有资源都会拦截
      3. 后缀名拦截  :       *.jsp                   访问所有后缀名为jsp的资源都会拦截
      4. 拦截所有资源 :     /*                       访问所有资源 都会拦截

   2. 拦截方式的配置 :   即资源被访问的方式

      1. 注解配置    

         - 设置dispatchType  ,  可配置多个

           - REQUEST  : 默认值 , 浏览器直接请求 才会被拦截
           - FORWARD  :  转发访问资源  才会被拦截
           - INCLUDE  :  包含访问资源   才会被拦截
           - ERROR : 错误跳转资源   才会被拦截
           - ASYNC  : 异步访问资源   才会被拦截

         - ```java
           @WebFilter(value = "/*" , dispatcherTypes = {DispatcherType.REQUEST , DispatcherType.FORWARD})
           ```

      2. web.xml配置

         - ```xml
           <filter-mapping>
               <filter-name>filterDemo1</filter-name>
               <url-pattern>/*</url-pattern>
               <dispatcher>REQUEST</dispatcher>
           </filter-mapping>
           ```

6. 过滤器链 : 

   1. 在一个项目中可以设置多个过滤器 
   2. 过滤器的先后顺序 : 
      1. 注解配置  :  按照类名的字符串顺序比较  ,  谁小谁先执行    
         - 如 AFilter  和  BFilter     ,     AFilter先执行
      2. xml配置   :  谁定义在上边 , 谁先执行
   3. 执行顺序 : 
      1. 过滤器 1 
      2. 过滤器 2 
      3. 访问的资源
      4. 过滤器 2
      5. 过滤器 1 

7. 项目中使用Filter的地方

   1. 转换HttpServlet  :  拦截路径为   /* 

      - ```java
        public class OutputReplaceFilter implements Filter {
        	
        	public void init(FilterConfig config) throws ServletException {
        		
        	}
        
        	public void doFilter(ServletRequest request, ServletResponse response,
        			FilterChain chain) throws IOException, ServletException {
        
        		HttpServletRequest rq = (HttpServletRequest) request;
        		HttpServletResponse rp = (HttpServletResponse)response;
        		chain.doFilter(new HTMLCharecterRequest(rq), rp);
        	}
        
        	public void destroy() {
        
        	}
        }
        ```

   2. 转换请求的编码  :   拦截路径为 *.jsp 和 action

      - ```java
        public class EncodingFilter implements Filter{
            private String encoding;
        
            public EncodingFilter() {
            }
        
            public void init(FilterConfig config){
                encoding=config.getInitParameter("encoding");
                if(encoding==null||encoding.equals("")){
                    encoding="GBK";
                }
            }
        
            public void doFilter(ServletRequest request,
                                 ServletResponse response,
                                 FilterChain chain) throws ServletException,IOException{
                try{
                    request.setCharacterEncoding(encoding);
                }
                catch(UnsupportedEncodingException e){
                }
                chain.doFilter(request,response);
            }
        
            public void destroy(){
            }
        }
        ```

   3. 访问权限校验 及 特别通知公告校验 :  拦截路径为action

      - ```java
        public class FundbizRejectNoticeFilter implements Filter {
        	
        	public void init(FilterConfig config) throws ServletException {
        	}
        
        	public void doFilter(ServletRequest request, ServletResponse response,
        			FilterChain chain) throws IOException, ServletException {
        		HttpServletRequest rq = (HttpServletRequest) request;
        			String url = rq.getRequestURI();
        			String mname = rq.getParameter("mname")==null?"":rq.getParameter("mname").toString();
        			//消息中心报错处理
        			boolean fundbizFlag = false;
        			if((url+"?mname="+mname).indexOf("msg.do?mname=enter")>-1||(url+"?mname="+mname).indexOf("msg.do?mname=userFirstPage")>-1){
        				UserBean user = (UserBean)rq.getSession().getAttribute(SessionConstants.USER);
        				Object entity = null;
        				
        				Iterator it=user.getRoles().iterator();
        				while (it.hasNext()) {
        					Role role = (Role) it.next();
        					if(role.getRoleId() == NewApplicationConstants.BIZ_PARTICIPANT_FUND_MANAGER){
        						entity = SessionUtil.getParticipantFromSession(rq,NewApplicationConstants.BIZ_PARTICIPANT_FUND_MANAGER);
        						fundbizFlag = true;
        					}else if (role.getRoleId() == NewApplicationConstants.BIZ_PARTICIPANT_FUND_CUSTODIAN) {
        						entity = SessionUtil.getParticipantFromSession(rq,NewApplicationConstants.BIZ_PARTICIPANT_FUND_CUSTODIAN);
        						fundbizFlag = true;
        					}else if (role.getRoleId() == NewApplicationConstants.BIZ_PARTICIPANT_FUND_AGENT) {
        						entity = SessionUtil.getParticipantFromSession(rq,NewApplicationConstants.BIZ_PARTICIPANT_FUND_AGENT);
        						fundbizFlag = true;
        					}
        				}
        				
        				if(fundbizFlag&&entity==null&&(url+"?mname="+mname).indexOf("msg.do?mname=enter")>-1){
        					request.setAttribute("showContent", "该用户未分配代码，请联系系统管理员！");
        					request.getRequestDispatcher("/WEB-INF/jsp/fundbizNoPerm.jsp").forward(request, response);
        				}
        				if(fundbizFlag&&entity==null&&(url+"?mname="+mname).indexOf("msg.do?mname=userFirstPage")>-1){
        					return;
        				}
        			}
        			boolean fundbizRejectNoticeUrlFlag = false;
        			List<String> rejectNoticeList = FundBizConstants.rejectNoticeList;
        			for (String rejectUrl : rejectNoticeList) {
        				if(url.indexOf(rejectUrl)>-1||(url+"?mname="+mname).indexOf(rejectUrl)>-1){
        					fundbizRejectNoticeUrlFlag = true;
        					break;
        				}
        			}
        			if(fundbizRejectNoticeUrlFlag){
        				String fundbizOpFlag = "";
        				Object obj = rq.getSession().getAttribute("fundbizOpFlag");
        				if(obj!=null){
        					fundbizOpFlag = String.valueOf(obj);
        					if(FundBizConstants.NO_VALLUE.equals(fundbizOpFlag)){
        						request.setAttribute("showContent", "您有不同意的特别公告，只能使用电子合同的查询功能，无法使用本系统办理其他业务,请在通知公告里修改为同意。");
        						request.getRequestDispatcher("/WEB-INF/jsp/fundbizNoPerm.jsp").forward(request, response);
        					}else {
        						chain.doFilter(request,response);
        					}
        				}else {
        					chain.doFilter(request,response);
        				}
        				
        			}else {
        				chain.doFilter(request,response);
        			}
        	}
        
        	public void destroy() {
        	}	
        }
        ```
        



### Listener 监听器

1. 概念 : web的三大组件之一 (servlet , filter)

2. 事件监听机制 : 

   1. 事件             : 一件事情
   2. 事件源         : 发生事情的地方
   3. 监听器         : 一个对象
   4. 注册监听     : 将事件 , 事件源 , 监听器 绑定在一起 , 当事件源上发生某个事件后 , 执行监听器的代码

3. ServletContextListener : 接口 

   1. 方法
      1. void  contextDestoryed     :    ServletContext对象被销毁时调用
      2. void contextInitialized       :     ServletContext对象创建时调用

4. 使用步骤 : 

   1. 定义类 ,实现接口

   2. 重写方法

      - ```java
        public class MyServletListener implements ServletContextListener {
            @Override
            public void contextDestroyed(ServletContextEvent sce) {
                System.out.println("context对象被销毁了");
            }
        
            @Override
            public void contextInitialized(ServletContextEvent sce) {
                System.out.println("context对象被创建了");
            }
        }
        ```

   3. 配置

      1. 注解配置

         - @WebListener

      2. web.xml配置

         - ```xml
           <listener>
               <listener-class>com.web.listener.MyServletListener</listener-class>
           </listener>
           ```

