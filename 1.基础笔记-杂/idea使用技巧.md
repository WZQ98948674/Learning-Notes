### idea使用技巧

1. 定制动态模板 
   
   1.  settings-> Live Templates ->  +  ->Template Group ->Live Template -> 输入模板名称 及 模板代码  ->选择模板使用的文件类型 ->OK
   
2. 配置maven中tomcat启动快捷方式
   
   1. 打开tomcat配置 -> + -> maven -> command line : tomcat7:run -> name : tomcat ->apply ->Ok
   
3. idea进行SpringBoot热部署失败

   1. 出现这种情况，并不是热部署配置问题，其根本原因是因为Intellij IEDA默认情况下不会自动编译，需要对IDEA进行自动编译的设置，如下：

      <img src="C:/Users/user/Desktop/笔记/img/springBoot/19.png" style="zoom:80%;" />

      然后 Shift+Ctrl+Alt+/，选择Registry

      <img src="C:/Users/user/Desktop/笔记/img/springBoot/20.png" style="zoom:80%;" />

### 快捷键

1. ctrl + alt + b  查看接口的实现类 
2. shift + shift  搜索