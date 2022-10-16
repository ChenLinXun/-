# JavaScript学习笔记

### 一、概述

#### 1. 什么是JavaScript

1. JavaScript是一门==跨平台==、==面向对象==的==脚本语言==，来控制网页行为的，它能使网页可交互
2. JavaScript 和 Java是==完全不同==的语言，不论是概念还是设计。但是基础语法类似

#### 2. W3C标准

​	W3C标准：网页主要由三部分组成

	1. 结构：HTML
	1. 表现：CSS
	1. 行为：JavaScript

### 二、引入方式

#### 1. 内部脚本

1. 内部脚本：将JS代码定义在HTML页面中
2. 在HTML中，JS代码必须位于`<script>`与`</script>`标签之间
3. 提示：
   1. 在HTML文档中，JS代码块可以在任意地方，放置任意数量的JS代码
   2. ==一般把脚本置于`<body>`元素的底部，可以改善显示速度，因为脚本执行会拖慢显示==

#### 2. 外部脚本

1. 外部脚本：将JS代码定义在外部JS文件中，然后引入到HTML页面中
2. 外部JS文件：demo.js
3. 引入外部JS文件：`<script src="../js/demo.js"></script>`
4. 注意：
   1. 外部脚本不能包含`<script>`标签
   2. `<script>`标签不能自闭合

### 三、基础语法

#### 1. 书写语法

1. 区分大小写：与Java一样，变量名、函数名以及其他一切东西都是区分大小写的
2. 每行结尾的分号可有可无
3. 注释：
   1. 单行注释：// 注释内容
   2. 多行注释：/* 注释内容 */ 
4. 大括号表示代码块

#### 2. 输出语句

1. 使用window.alert() 写入警告框
2. 使用document.write() 写入HTML输出
3. 使用console.log() 写入浏览器控制台

#### 3. 变量

1. JavaScript中用 var 关键字（variable的缩写）来声明变量

2. JavaScript是一门==弱类型语言==，变量**可以存放不同类型的值**

   例子：
   var test = 20;
   test = "张三"

3. 变量名需要遵循如下规则（==和Java一致==）：

   1. 组成字符可以是任何字母、数字、下划线(_)、或美元符号($)
   2. 数字不能开头
   3. 建议使用驼峰命名

4. 用var定义变量的特点：

   1. 作用域：**全局变量**
   2. 变量**可以重复定义**，例如：
      var age = 30;
      var age = 20;

5. 目前JS最新版本，ECMAScript 6新增了==let关键字==来定义变量。用法类似于var，但是所声明的变量只在let关键字所在的代码块有效(==不是全局变量==)，且不允许重复声明

6. ECMAScript 6新增了==const关键字==，用来声明一个==只读的常量==。一旦声明，常量值就不能改变

#### 4. 数据类型

1. JavaScript中分为：**原始类型 和 引用类型**

2. **5种原始类型：**
   number：数字（整数、小数、NaN(Not a Number)）
   string：字符、字符串、单双引皆可
   boolean：布尔值，true、false
   null：对象为空
   undefined：当声明的变量未初始化时，该变量的默认值是undefined

3. **类型转换：**

   1. ==其他类型转为number：==

      1. string：按照字符串的字面值，转为数字，如果字面值不是数字，则转为NaN
         方式：
         1. 在字符串前加 “+” 号，例如：var str = +"20"；（该方式不常用）
         2. 使用parseInt函数：var str = "20";   parseInt(str); （常用的方式）
      2. boolean：true转为1，false转为0
         方式：在boolean前面加 “+” 号，例如：var flag = +true; //转为1

   2. ==其他类型转为boolean：==

      1. number：0和NaN转为false，其他转为false
      2. string：空字符串转为false，其他内容转为true
      3. null：转为false
      4. undefined：转为false

      **在 if(其他类型) 中会自动将其他类型转换为boolean，**例如：
      var flag = 0;
      if(flag){...}

#### 5. 运算符

1. 分类：

   1. 一元运算符：++，--
   2. 算数运算符：+，-，*，/，%
   3. 赋值运算符：=，+=，-=
   4. 关系运算符：>，<，>=，<=，!=，==，**`===`**（全等于）
   5. 逻辑运算符：&&，||，！
   6. 三元运算符：条件表达式 ? true_value : false_value

2. 解释：

   **==：**

   1. 判断类型是否一样，如果不一样，则进行==类型转换==

   2. 再去比较其值

      例如：var age1 = 20; var age2 = "20"; alert(age1 == age2); //为true

   **===：**

   1. ==判断类型是否一样==，如果不一样，直接返回false
   2. 再去比较其值

​	（除了等于：== 和 全等于：=== ，其他与java一样）

#### 6. 流程控制语句

1. if
2. switch
3. for
4. while
5. do...while

（**和Java一模一样**）

#### 7. 函数

1. 定义：JavaScript函数通过function关键词进行定义，

   1. 第一种定义格式：
      function functionName(参数1，参数2...){
      	要执行的代码
      }

      注意：

      1. **形式参数不需要类型。**因为JavaScript是弱类型语言
      2. **返回值也不需要定义类型，**可以在函数内部直接使用return返回即可

      例如：
      function add(a,b){
      	return a+b;
      }

   2. 第二种定义格式：
      var functionName = function(参数列表){
            要执行的代码
      }
      例如：
      var add = function(a,b){
             return a+b;
      }

2. 调用：函数名称(实际参数列表)；
   let result = add(1,2);
   在JS中，函数调用可以传递任意个数参数

### 四、对象

#### 1. 基础对象

1. ==Array数组对象：==

   1. 概述：JavaScript Array对象用于定义数组

   2. **定义：**
      方式一：**var 变量名 = new Array(元素列表);**  例如：var arr = new Array(1,2,3);
      方式二：**var 变量名 = [元素列表];**  例如：var arr = [1,2,3];

   3. 访问：
      arr[索引] = 值；  例如：arr[0] = 1;

   4. **特点：**
      **JS数组类似于Java集合，长度，类型都可变**，例如：

      1. 变长：
         var arr = [1,2,3];
         arr[10] = 10;
         alert(arr[10]); //输出10
         alert(arr[9]); //输出undefined
      2. 变类型：
         var arr = [1,2,3];
         arr[10] = "hello";
         alert(arr[10]); //输出"hello"

   5. 属性：

      length：数组中元素的个数

   6. **方法：**

      1. **push(元素)：添加元素**
      2. **splice(起始索引，删除元素个数)：删除元素**

2. ==String字符串对象：==

   1. 概述：JavaScript String对象用于定义字符串
   2. **定义：**
      方式一：**var 变量名 = new String(s);**   例如：var str = new String("hello");
      方式二：**var 变量名 = s; ** 例如：var str = "hello";  或  var str = 'hello';
   3. 属性：
      length：字符串的长度
   4. **方法：**
      1. **charAt()：返回指定位置的字符**
      2. indexOf()：检索字符串，返回字符串第一次出现的位置
      3. **trim()：去除字符串前后两段的空白字符**，例如：
         var str = '   abc    ';
         alert(1 + str + 1)； //输出：1  abc  1
         alert(1 + str.trim() + 1)； //输出：1abc1

3. ==自定义对象：==

   1. **概述：在JavaScript中用大括号包裹代表定义一个对象，而不需要定义类**

   2. **定义：**
      var 对象名称 = {
                                   属性名称1：属性值1，
                                   属性名称2：属性值2，
                                   ...
                                   函数名称：function(形参列表){}
                                   ...
                                 };

      例子：
      var person = {
            name:"zhangsan",
            age:23,
      	  eat:function(){
      			alert("干饭~");
            }
       };

   3. **调用：和 Java 一样**
      alert(person.name);
      person.eat();

4. ==RegExp正则表达式对象：==

   1. 概述：代表一个正则表达式
   2. 使用：创建正则表达式对象，例：
      var reg = new RegExp("^\\\w{6,12}$");

#### 2. BOM对象

1. Browser Object Model **浏览器对象模型**

2. JavaScript 将浏览器的各个组成部分封装为一个对象

3. 组成有：
   Window：浏览器窗口对象
   Navigator：浏览器对象
   Screen：屏幕对象
   History：历史记录对象
   Location：地址栏对象

4. ==Window对象介绍：==

   1. **获取：**

      **直接使用window，其中window.可以省略，**例如：
      window.alert("hello");  或  alert("hello");

   2. **属性：**

      **获取其他BOM对象：**

      1. history：对History对象的只读引用。
      2. Navigator：对Navigator对象的只读引用。
      3. Screen：对Screen对象的只读引用。
      4. location：用于窗口或框架的Location对象。

   3. **方法：**

      1. alert()：显示带有一段消息和一个确认按钮的警告框
      2. **confirm()：**显示带有一段消息以及确认按钮和取消按钮的对话框（有返回值，确定返回true，取消返回false）
      3. **setInterval()：**定时器，按照指定的周期（毫秒）来调用函数或计算表达式（循环执行）
      4. **setTimeout()：**定时器，在指定的毫秒数后调用函数或计算表达式（执行一次）

5. ==History对象介绍：==

   1. **获取：**

      **使用window.history获取，其中window.可以省略**，例如：
      window.history.方法();  或  history.方法();

   2. **方法：**

      1. back()：加载 history 列表中的前一个URL
      2. forward()：加载 history 列表中的下一个URL

6. ==Location对象介绍：==

   1. **获取：**

      **使用window.location获取，其中window.可以省略**，例如：
      window.location.方法();  或  location.方法();

   2. **属性：**

      **href：设置或返回完整的URL，**例如：
      alert("要跳转了");
      location.href = "https://www.bilibili.com/"
      （一般可以和定时器配合使用，几秒后跳转页面）

#### 3. DOM对象

1. ==DOM概念：==

   1. Document Object Model **文档对象模型**
   2. JavaScript 将标记语言的各个组成部分封装为一个对象
   3. 组成有：
      1. Document：整个文档对象
      2. Element：元素对象
      3. Attribute：属性对象
      4. Text：文本对象
      5. Comment：注释对象
   4. **JavaScript 通过DOM，就能够对HTML进行操作了：**
      1. 改变HTML元素内容
      2. 改变HTML元素样式（CSS）
      3. 对HTML DOM 事件作出反应
      4. 添加和修改HTML元素

2. ==DOM详解：==

   1. **DOM是W3C(万维网联盟)的标准**

   2. DOM定义了访问HTML和XML文档的标准:w3C DOM标准被分为3个不同的部分:

      1. **核心DOM：针对任何结构化文档的标准模型**
         Document：整个文档对象
         Element：元素对象
         Attribute：属性对象
         Text：文本对象
         Comment：注释对象

      2. XML DOM：针对XML文档的标准模型（核心DOM的子集）

      3. **HTML DOM：针对HTML文档的标准模型**（核心DOM的子集）

         lmage: `<img>`
         Button : `<input type='button'>`

   3. DOM树：

      ![image-20220401180913719](..\image\image-20220401180913719.png)

3. ==Element对象的获取和使用：==

   1. **获取Element对象：**

      **使用Document对象的方法来获取：**

      1. getElementById()：根据id属性值获取，返回一个Element对象
      2. getElementsByTagName()：根据标签名称获取，返回Element对象数组
      3. getElementsByName()：根据name属性值获取，返回Element对象数组
      4. getElementsByClassName()：根据class属性值获取，返回Element对象数组

   2. **常见HTML Element对象的使用：**（十分多，具体使用查文档）

      1. **属性：**
      1. Element对象都有的属性为**通用属性**，有些Element对象有**特有的属性**
         
      2. **常见属性：**
            1. image对象特有属性：**src**：设置或返回图形的URL
            例：
               var img = document.getElementById("light");
               img.src = "../imgs/on.gif";  //修改图片URL
            
            2. 通用属性：**style**：设置元素CSS样式
            例：
               var div = document.getElementById("d1");
               d1.style.color = 'red';
            
            3. 通用属性：**innerHTML**：设置元素内容
               例：
               var d2 = document.getElementById("d2");
               d2.innerHTML == "呵呵";  // 修改了d2块元素中的文本信息
            4. checkbox对象特有属性：**checked**：设置或返回checkbox是否被选中
               例：
               var cb1 = document.getElementById("checkbox1");
               cb1.checked = true;  //修改checkbox的选中状态为被选中
         
      2. 方法：
      略。
      
      **更多HTML Element对象的使用，查阅手册：**
   https://www.w3school.com.cn/jsref/index.asp

### 五、事件监听

#### 1. 概述

1. 事件：HTML事件是发生在HTML元素上的“事情”。比如：
   1. 按钮被点击
   2. 鼠标移动到元素之上
   3. 按下键盘按键
2. 事件监听：JavaScript 可以在事件被侦测到时**执行代码**

#### 2. 事件绑定

事件绑定有两种方式：

1. 方式一：通过HTML标签中的事件属性进行绑定，例：
   `<input type="button" onclick='on()'>`

   function on(){
         alert("我被点了");
   }

2. 方式二：**通过DOM元素属性绑定，**例：
   `<input type="button" id="btn">`
   document.getElementById("btn").onclick = function(){
        alert("我被点了");
   }

#### 3. 常见事件

1. 常见事件可参考文档：HTML DOM事件（DOM Event）（通过DOM元素属性绑定）

   https://www.w3school.com.cn/jsref/dom_obj_event.asp

2. 最常见有以下几个：

   | 事件        | 描述                         |
   | :---------- | :--------------------------- |
   | onchange    | HTML 元素已被改变            |
   | onclick     | 用户点击了 HTML 元素         |
   | onmouseover | 用户把鼠标移动到 HTML 元素上 |
   | onmouseout  | 用户把鼠标移开 HTML 元素     |
   | onkeydown   | 用户按下键盘按键             |
   | onload      | 浏览器已经完成页面加载       |

### 六、正则表达式

#### 1. 概念

​      **正则表达式定义了字符串组成的规则**

#### 2. 定义

1. 直接量：**注意不要加引号**
   var reg = /^w{6,12}$/;  **用两个"/"符号包起来，代表这个量是正则表达式**

2. 创建 RegExp对象
   var reg = new RegExp("^\\\w{6,12}$");

3. 方法：
   test(str)：判断指定字符串是否符合规则，返回true或false

4. **语法：**

   | 符号 | 含义                                                   |
   | ---- | ------------------------------------------------------ |
   | ^    | 表示开始                                               |
   | $    | 表示结束                                               |
   | []   | 代表某个范围内的单个字符，比如：[0-9]单个数字字符      |
   | .    | 代表任意单个字符，除了换行和行结束符                   |
   | \w   | 代表单词字符：字母、数字、下划线(_)，相当于[A-Za-z0-9] |
   | \d   | 代表数字字符：相当于[0-9]                              |

   | 量词  | 含义             |
   | ----- | ---------------- |
   | +     | 至少一个         |
   | *     | 零个或多个       |
   | ?     | 零个或一个       |
   | {x}   | x个              |
   | {m,}  | 至少m个          |
   | {m,n} | 至少m个，最多n个 |

#### 3. 例子

1. 练习：
   1. var reg = /^\w+$/;     至少有一个单词（字母、数字、下划线）
   2. var reg = /^\w{6,12}$/;   至少有6个单词，最多有12个单词
   3. var reg = /^[1]\d{10}$/;   长度11，数字组成，第一位是1
2. 常用的一些正则表达式验证：
   1. 提取信息中的邮件地址：\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*
   2. 提取信息中的中国手机号码：(86)*0*13\d{9}
   3. 提取信息中的中国邮政编码：[1-9]{1}(\d+){5}
   4. 提取信息中的中国身份证号码：\d{18}|\d{15}
   5. 匹配邮箱：/^([a-zA-Z0-9_-])+@([a-zA-Z0-9_-])+(.[a-zA-Z0-9_-])+/
   6. 匹配Email地址的正则表达式：\w+([-+.]\w+)*@\w+([-.] \w+)*\.\w+([-.]\w+)*
   7. 匹配帐号是否合法(字母开头，允许5-16 字节，允许字母数字下划线)：
      `^[a-zA-Z][a-zA-Z0-9_]{4,15}$`
   8. 匹配腾讯QQ号：`[1-9][0-9]{4,}`
   9. 匹配国内电话号码：\d{3}-\d{8}|\d{4}-\d{7}
   10. 匹配中国邮政编码：[1-9]\d{5}(?! \d)
   11. 匹配身份证：\d{15}|\d{18}



































































