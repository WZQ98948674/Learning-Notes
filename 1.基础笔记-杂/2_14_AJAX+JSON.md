### AJAX

1. 概念 :  ASync JavaScript And  Xml
   - 异步发送请求 
   - 无需重新加载整个网页 , 就可更新部分网页的技术
   - 提升用户体验
2. 实现方式 : 
   1. 原生JS实现
   2. JQuery实现 

      1. 通用        :  $.ajax()

         - ```js
           $.ajax({
               url : "../ajaxServlet1",                 //请求路径
               type : "POST",                          //请求方式
               //data : "username=zhangsan & age=23",    //携带数据
              data : {"username":"zhangsan"},
              datatype : "text",                      //设置响应的数据格式
              success : function () {                 //请求成功之后的回调函数
                  alert("ajax请求成功")
              },
              error : function () {
                  alert("ajax请求出错")               //请求出错之后的回调函数
               }
           });
           ```

      2. get请求  : $.get()        书写方式更为简单

         - ```js
           function fun2() {
               //格式 : $.get(url,data,callback,type);
               $.get("../ajaxServlet",{username:"rack"},function (data) {
                   alert("get-Ajax请求成功");
                   alert(data);
               },"text")
           }
           ```

      3. post请求 : $.post()     书写方式更为简单

         - ```js
           function fun2() {
               //格式 : $.post(url,data,callback,type);
               $.post("../ajaxServlet",{username:"rack"},function (data) {
                   alert("get-Ajax请求成功");
                   alert(data);
               },"text")
           }
           ```



### json

1. 概念 : JavaScript Object Natation               JavaScript 对象表示法

   - 用json表示对象

2. 作用 : 

   1. 存储和交换文本信息
   2. 网络中传输数据
   3. 比xml更小,更快,更易于解析

3. 语法

   1. 基本规则 : 

      - 由键值对构成  , 键用不用引号都可以
      - 多个键值对由逗号分隔
      - 花括号保存对象
      - 大括号表示数组

   2. 值 : 

      1. 数字
      2. 字符串
      3. true/false
      4. 数组(方括号 , 可以和花括号嵌套)
      5. 对象(花括号 , 可以相互嵌套)

   3. 获取json中的数据

      1. json对象.键名

         - ```js
           var person = {"name":"张三","age":23,"gender":true};
           alert(person.name)
           ```

      2. json对象["键名"]

         - ```js
           var person = {"name":"张三","age":23,"gender":true};
           alert(person["age"])
           ```

      3. 数组对象[索引]

         - ```js
           var persons = [{"name":"张三","age":23,"gender":true},
               		   {"name":"李四","age":25,"gender":true},
              			   {"name":"王五","age":27,"gender":false}
           ];
           alert(persons[2].name);
           
           var persons =  {"persons":[{"name":"张三","age":23,"gender":true},
                       {"name":"李四","age":25,"gender":true},
                       {"name":"王五","age":27,"gender":false}
                   ]};
           alert(persons.persons[2].name);
           ```
      
   4. 遍历json

      - ```js
        var person = {"name":"张三","age":23,"gender":true};
        for (var key in person){
            alert(key + ":" + person[key])
        }
        ```
        
   
4. JSON解析器  : 

   1. Jsonlib     : 官方  
   2. Gson       : 谷歌
   3. fastjson  : 阿里
   4. jackson   : springMVC内置

5. jackson 使用步骤 :

   1. 导包
      - jackson-annotations-2.2.3.jar   /   jackson-core-2.2.3.jar   /   jackson-databind-2.2.3.jar
   2. 创建Jackson核心对象 ObjectMapper
   3. 调用ObjectMapper的相关方法进行转换

6. JSON和java对象的转换

   1. java对象转为 JSON 

      1. ```java
         //将java对象转为json字符串
         String jsonString = mapper.writeValueAsString(obj);
         ```
         
      2. ```java
            //重载方法
            mapper.writeValue(arg1, obj);
            //arg1有多重类型
            	1.Writer         将obj对象转为json字符串 , 并将json数据放入到字符输出流中 并输出
                2.OutPutStream   将obj对象转为json字符串 , 并将json数据放入到字节输出流中 并输出
                3.File           将obj对象转为json字符串 , 并将json数据保存到文件中
            ```

      - 注解的使用 : 

        - @JsonIgnore   排除该属性  , 加在java类的成员变量上 , 则不对其进行转换

        - @JsonFormat  格式化该属性 , 加在java类的成员变量上 , 则在转换时对其进行格式化

          - ```java
            @JsonFormat(pattern = "yyyy-MM-dd")
            private Date birthday;
            ```

   2. JSON转java对象

         1. ```java
               //将JSON字符串转为java对象
               Person person = mapper.readValue(jsonString, Person.class);
               ```
   
7. 客户端接收JSON格式的数据

   1. 在ajax请求中设置type类型为json

   2. 在response响应中设置返回类型为 json 

      - ```java
        response.setContentType("application/json;charset=utf-8");
        ```