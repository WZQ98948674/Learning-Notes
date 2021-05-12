### Nginx

1. 概念 : 一款高性能的http服务器/反向代理服务器 , 能够支撑5万的并发量 , 开源免费.

2. 应用场景  : 

   1. http服务器 : 可以做网页静态服务器
   2. 虚拟主机 :  可以实现在一台服务器虚拟多个网站 (修改配置文件的域名或端口)
   3. 反向代理/负载均衡服务器 : 避免集群状态下 , 某台服务器负载过高宕机 , 或者某台服务器一直闲置.

3. 在Linux系统下的安装 : 

   - 看文档

4. 配置文件 : 

   - /conf/nginx.conf

     - 可以设置静态页面的默认路径

       ```conf
       server {
               listen       81; # 监听的端口
               server_name  localhost; # 域名或ip
               location / {	# 访问路径配置
                   root   index;# 根目录
                   index  index.html index.htm; # 默认首页
               }
               error_page   500 502 503 504  /50x.html;	# 错误页面
               location = /50x.html {
                   root   html;
               }
           }
       ```

     - 可以设置多个server , 相当于部署了多个网站 , 每个都有自己的配置 (端口/域名)

       ```conf
       //通过端口号配置
       server {
               listen       81; # 监听的端口
               server_name  localhost; # 域名或ip
               location / {	# 访问路径配置
                   root   index;# 根目录
                   index  index.html index.htm; # 默认首页
               }
               error_page   500 502 503 504  /50x.html;	# 错误页面
               location = /50x.html {
                   root   html;
               }
           }
       server {
               listen       82; # 监听的端口
               server_name  localhost; # 域名或ip
               location / {	# 访问路径配置
                   root   regist;# 根目录
                   index  regist.html; # 默认首页
               }
               error_page   500 502 503 504  /50x.html;	# 错误页面
               location = /50x.html {
                   root   html;
               }
           }
       ```

       ```conf
        //通过域名配置
        server {
               listen       80;
               server_name  www.hmtravel.com;
               location / {
                   root   cart;
                   index  cart.html;
               }
           }
       server {
        		listen       80;
               server_name  regist.hmtravel.com;
               location / {
                   root   search;
                   index  search.html;
               }
           }
       ```

5. 代理 :  代理的是客户端  (例如 , 本机不能上网 , 可以通过配置一台能上网的主机作为代理服务器 , 本机的所有请求通过代理服务器发送到互联网 , 再又代理服务器返回给本机 , 实现从局域网到互联网 , 翻墙)

6. 反向代理 : 代理的是服务器 ( 将多台服务器配置到代理服务器上 , 可以做负载均衡 , 并且所有请求都打到代理服务器上 , 可以在代理服务器和真实服务器之间增加安全措施 , 实现从外网到内网的转换 , 更加安全 )

7. 配置反向代理 : 

   ```conf
   	//配置反向代理 名称及服务器ip,端口
   	upstream tomcat-travel{
   	   server 192.168.177.129:8080;
       }
   
       server {
           listen       80; 
           server_name  www.hmtravel.com; 
           
           location / {	
           	# root   index;
           	//配置要走的反向代理名称
   			proxy_pass http://tomcat-travel;
           	index  index.html index.htm; 
       	}
   	}
   ```

8. 配置负载均衡 :

   ```conf
   	//配置负载均衡 默认权重
   	upstream tomcat-travel{
   	   server 192.168.177.129:8080;
   	   server 192.168.177.130:8080;
   	   server 192.168.177.131:8080;
       }
   ```

   ```conf
   	//配置负载均衡 配置权重, 默认权重都是1
   	upstream tomcat-travel{
   	   server 192.168.177.129:8080 weight=2;
   	   server 192.168.177.130:8080;
   	   server 192.168.177.131:8080;
       }
   ```

   

   