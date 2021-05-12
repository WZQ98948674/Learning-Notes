## JavaWeb

1. JavaWeb概念:
   - 使用Java语言开发的基于互联网的项目.
2. 软件架构
   - C/S
   - B/S
3. 资源分类:
   1. 静态资源
      - 使用静态网页技术发布的资源
      - 特点: 
        - 任何人访问 , 看到的效果都是一样的.
        - 如 : 文本 , 图片 , 音频 , 视频 , **HTML , CSS , JS(静态网页技术三剑客)** ...
        - 如果用户请求的是静态资源 , 那么服务器会直接将静态资源返回给浏览器 , 浏览器内置了解析静态资源文件的引擎 , 可以展示静态资源.
   2. 动态资源
      - 使用静态网页技术发布的资源
      - 特点 : 
        - 任何人访问 , 看到的效果可能不一样的.
        - 如 : jsp/servlet , php ,asp...
        - 如果用户请求的是动态资源 , 那么服务器会执行动态资源 , 并转换为静态资源返还给浏览器 , 因为浏览器只能解析静态资源 

4. 静态资源
   1. HTML (超文本标记语言): 用于搭建基础网页 , 展示网页内容
   2. CSS : 用于页面美化 , 页面布局
   3. JS : 用于页面动态效果



### HTML

- 超文本 : 超级文本
- 标记语言 : 由标签构成的语言 : HTML , XML
  - 标记语言不是编程语言 , 因为它没有逻辑



### CSS

- 层叠 : 多个样式可以作用在同一个HTML标签上 , 同时生效
- 样式表 : 控制样式



## JavaScript

- 概念 : 客户端脚本语言 . 
  
  - 脚本语言 : 不需要编译 , 直接被解释执行
- 功能 : 增强用户和HTML页面的交互 , 控制HTML元素 , 增强用户体验 . 
- 组成 : __ECMAScript + DOM + BOM__

### ECMAScript

- 数据类型
  1. 原始数据类型(基本数据类型)
     1. number : 数字   : 整数/小数/NaN(not a number  不是一个数字的数字类型)
     2. String  : 字符串  
     3. boolean : 布尔
     4. null : 一个对象为空的占位符
     5. undefined: 未定义 , 如果一个变量没有赋值 , 则会被默认是undefined
  2. 引用数据类型(对象)
  
- 常用方法/运算符 :

  - document.write();  打印到页面上
  - typeof(变量);   获取变量类型

- 特殊语法:

  - var不用:定义全局变量
  - var用 : 定义局部变量

- 基本对象 : 

  1. Function

  2. arguments : 方法中的内置对象 , 是一个封装了所有方法实际参数的数组 , 可通过遍历获取所有的值

  3. Array : 数组对象  push() ;  join() ;

  4. Date : 时间对象  toLocalString();  getTime();

  5. Math : 数学对象  不用创建对象,直接使用    PI   random();   round(a);  

  6. RegExp : 正则对象    

    1. 规则:

       - 单个字符 [] 
         - \d  : 单个数字字符 , 等同于[0-9]
         - \w : 单个单词字符 , 等同于[a-zA-Z]
       - 量词
         - ? :  0次或1c次
         - *:  0次或多次
         - +:  1次或多次
         - {m,n}   - m <= 次数 <= n
           - {,n}   m缺省 , 最多n次
           - {m,}  n缺省 , 最少m次
    2. 使用

       - var reg = new RegExp("正则"); 	---var reg = new RegExp("\w{6,12}");

       - var reg = /正则/;   --- var reg = /\w{6,12}/;

       注意 : 正则表达式一般有开头和结尾符号   

       - 开头 :　＾
       - 结尾：  $ 
            	- 例如 : var reg = new RegExp("^\\w{6,12}");         var reg = /^\w(6,12)$/;

    3. 方法 test(参数)      返回值 : true/false
       - var username = "zhangsan";
       - reg.test(username); 

  7. Global

     1. 特点 : 全局对象 , 这个Global对象中封装的方法不需要对象就可以直接调用

     2. 方法 : 

        - encodeURI("要编码的字符") : url编码 , 返回值为编码后的字符串
        - decodeURI("要解码的字符") : url解码 , 返回值为解码后的字符串
        - encodeURIComponent("要编码的字符") : url编码
        - decodeURIComponent("要解码的字符") : url解码

        - 这两组方法的区别 : encodeURIComponent方法会将符号之类的都编码 , encodeURI只编码中文
        -   
        - parseInt("字符串") : 把字符串逐个转为number , 如果遇到不是数字则停止转换
        - eval("字符串") : 将js中的字符串转为脚本执行
        - isNan(要判断的对象) : 判断是否为Nan

     3. URL编码 : 

        - 在浏览器的地址栏需要传递不能是中文 , 所以要编码

        - 一个汉字在GBK中占2个字节 , 在UTF8中占3个字节
        - 一个字节是8位 , 转码时每4个位转为一个16进制的数字 , 每个字节前用%分割 , 例如:
          - 1001 0101        ---> %95
        - 所以有多少个百分号就有多少个字节

  ### DOM

  1. 概念 : 文档对象模型 , 定义了html和xml文档的标准
     - 将标记语言文档的各个部分封装成对象 , 使用这些对象 , 对标记语言文档进行动态操作
     - dom树
  2. 组成
     - DOM被分成三个部分:  
       - 核心DOM
         1. __Document : 文档对象__
         2. __Element : 元素对象__
         3. Attribute : 属性对象
         4. Text : 文本对象
         5. Comment : 注释对象
         6. __Node : 节点对象 , 其他5个对象的父对象__
       - XML  DOM
       - HEML  DOM
  3. 核心DOM模型
     1. Document : 文档对象
        1. 获取
           - 在html dom中 window.document   或者  document
        2. 方法
           1. 获取element对象
              - getElementById()     根据id获取唯一元素
              - getElementsByName()   根据name获取一组元素 , 返回值为数组
              - getElementsByTagName()  根据标签名称获取一组元素 , 返回值为数组
              - getElementsByClassName()    根据classname获取一组元素 , 返回值为数组
           2. 创建其他dom对象
              - createAttribute(name)
              - createComment()
              - createElement(name)         动态创建元素,后续可以对其设置属性...
              - createTextNode()
        3. 属性
     2. Element : 元素对象
        1. 获取
           - 通过document创建
        2. 方法
           - removeAttribute()         移除属性 
           - setAttribute()                 设置属性       给标签动态的设置或删除属性！
        3. 属性
           - value     获取内容
     3. Node : 节点对象 , 其他5个对象的父对象
        1. 所有dom对象都可以被认为是一个节点
        2. 方法 : 用于对dom树进行CRUD
           - ele.removeChild(ele2);     删除子节点
           - ele.appendChild(ele2);     添加子节点
           - ele.replaceChild(ele1,ele2);     替换子节点
        3. 属性 :
           - ele.parentNode     返回节点的父节点
  4. HTML  DOM
     1. 标签体的设置和获取 : innerHTML   详见011.html文件和010.html文件
     2. 使用html元素对象的属性
     3. 控制样式 (详见011.html)
        1. 使用元素的style属性修改样式   
        2. 提前定义好类选择器样式,通过元素的className属性来设置样式

  

  #### 事件

  1. 功能 : 某些组件被执行了某些操作后 , 触发某些代码的执行
     - 事件/操作 : 如单击  双击   鼠标按下   键盘按下
     - 事件源/组件 : 按钮   文本输入框
     - 监听器 : 代码
     - 注册监听 : 将事件 , 事件源 , 监听器结合在一起 , 当事件源发生了某个事件 , 则触发执行某个监听器代码
  2. 常见事件Event :   (详见012.html)
     1. 点击事件 : 
        -  onclick : 单击事件
        - ondblclick : 双击事件
     2. 焦点事件 :
        - onfocus : 聚焦事件
        - onblur : 失焦事件
     3. 加载事件
        - onload : 加载事件
     4. 鼠标事件 :
        - onmousedown : 鼠标被按下
        - onmouseup : 鼠标被松开
        - onmousemove : 鼠标移动
        - onmouseover : 鼠标移到某一个元素上
        - onmouseout : 鼠标从某元素移开
     5. 键盘事件 : 
        - onkeydown : 键盘按下
        - onkeyup : 键盘松开
        - onkeypress : 键盘按下并松开
     6. 选择和改变 : 
        - onselect : 文本被选中
        - onchange : 域的内容被改变 (下拉框联动)
     7. 表单事件 : 
        - onsubmit : 确认按钮被点击 (用于表单提交时的校验. 返回false可以阻挡表单提交)
        - onreset : 重置按钮被点击
  3. 绑定事件
     1. 直接在html标签上指定事件  onclick()
     2. 通过js获取元素对象,指定事件属性

  ### BOM

  1. 概念 : 浏览器对象模型
  2. 组成:
     1. _浏览器对象 nacigator_
     2. __窗口对象 window__
     3. 地址栏对象 location
     4. 历史记录对象 history
     5. _显示器屏幕对象 Screen_
  3. window对象
     1. 特点 : 
        - window对象不需要创建 , 可以直接使用 window.方法()
        - window对象可以省略   方法();
     2. 方法 : 
        - 与弹出框有关
          - alter("str")          --警告框
          - confim("str")     ---确认框      点击确定返回true ,点击取消返回false
          - prompt("str")    ---输入框      返回用户输入的值
        - 打开/关闭窗口
          - open();    --可传参,String类型的网址,打开指定网址的窗口,返回值为window对象
          - close();   --谁调用关闭谁
        - 定时器
          - setTimeout(js代码或function对象 , 毫秒值)           只执行一次 , 返回值为定时器的唯一标识 , 用于取消定时器
          - clearTimeout(定时器的唯一标识)          取消定时器
          -   
          - setInterval(fun , 毫秒值)         循环定时器 , 每隔一段时间执行一次 , 返回值为定时器的唯一标识 , 用于取消定时器
          - clearInterval(定时器的唯一标识)
     3.  属性 :
        - 可以获取其他的BOM对象 window.nacigator , location , history , Screen
  4. Location对象
     1. 获取 : window.location;
     2. 方法 : location.reload();      刷新当前页面
     3. 属性 : href   获取地址栏路径      可以对其进行设置
  5. History对象 : 当前窗口浏览历史记录URL
     1. 获取 : window.history;
     2. 方法 : 
        1. back();      加载history列表中的前一个URL
        2. forward();      加载history列表中的后一个URL
        3. go(数字);         加载history列表中的具体某一个URL , 参数为数字,正数负数皆可,正数代表前进几个页面 , 负数代表后退几个页面
     3. 属性 : 
        - length   返回当前窗口的历史记录URL数量unsatisfiedlinkerror
