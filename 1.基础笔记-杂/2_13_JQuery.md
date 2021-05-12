### JQuery

1. 概念 :  JS的框架 , 简化JS开发

2. 版本 : 

   - 1.X版本 : 兼容ie678 , 使用广泛

   - 2.x版本 : 不兼容ie678 , 使用较少

   - 3.x版本 : 不兼容ie678 , 只支持最新的浏览器

3. jquery-xxx.js 和 jquery-xxx.min.js 的区别 : 

   - jquery-xxx.js : 开发版本 , 有缩进 , 便于查看源码 , 体积大
   - jquery-xxx.min.js : 生产版本 , 无缩进 , 不便查看源码 , 体积大

4. 使用步骤 : 

   1. 导入jquery.js文件

   2. 在页面中引入

      - ```jsp
        <script src="../js/jquery-3.3.1.min.js"></script>
        ```

   3. 使用jquery语法 , 简化开发

5. juqery 和 js 对象的相互转换 : 

   1. js --> juqery  :   $(js对象)
   2. juqery --> js  :   jquery对象[索引]     或    juqery对象.get(索引) .

6. JQuery 语法 : 

   1. 事件绑定  (标准绑定 / on,off绑定  / toggle事件切换)

      - jquery对象.click(function(){})    =     js对象.onclick = function(){}

      - ```js
        var bt1 = $("#bt1");
        bt1.click(function () {
            alert("点我了")
        })
        ```

   2. 入口函数
      - $(function(){ })          =      window.onload = function(){}

      - ```js
        $(function(){
            alert("加载....")
        })
        ```

      - 区别 : 
        
        -  window.onload只能定义一次 ,如果定义多次 , 后边会覆盖前边

   3. 样式控制 : 

      - jquery对象.css(键 , 值) ; 

      - ```js
        $("#div1").css("background-color","pink");
        ```

      - ```js
        //建议使用 , 可以用鼠标点击 试试看有没有拼错
        $("#div1").css("backgroundColor","pink");
        ```

   4. 选择器 : 

   5. DOM操作 : 

      1. 内容相关操作 : 
         1. html()
         2. text()
         3. val()
      2. 属性操作 : 
         1. 通用属性操作
            1. attr()
            2. removeAttr()
            3. prop()
            4. removeProp()
         2. class属性操作
            1. addClass()
            2. removeClass()
            3. toggleClass()

   6. 遍历 : 

      1. jquery对象.each(callback)
      2. $.each(obj , [callback] )
      3. for..of 
      
   7. 获取表单的数据 : 

      1. ```js
         //将表单数据序列化为字符串  "username=aaa&password=133"
         //可用于ajax异步提交表单时 传递数据
         $("#form").serialize() 
         ```

7. JQuery插件机制 : 

   1. 作用 : 增强功能
   2. 实现方式 : 
      1. $.fn.extend(object)
      2. $.extend(object)