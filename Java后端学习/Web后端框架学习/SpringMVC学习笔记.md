# SpringMVC学习笔记

### 一、SpringMVC概述

> SpringMVC 是由Spring官方提供的基于MVC设计理念的web框架
>
> SpringMVC 是基于Servlet封装的用于实现MVC控制的框架，实现前端和服务端的交互

#### 1. 优势

- 严格遵守了MVC分层思想
- 采用了松耦合、插件式结构；相比于Servlet原生开发以及其他的MVC框架，更灵活，更具扩展性
- SpringMVC是基于Spring的扩展、提供了一套完善的MVC注解
- SpringMVC在数据绑定、视图解析都提供了多种处理方式，可灵活配置
- SpringMVC对RESTful URL设计方法提供了良好的支持

#### 2. 本质工作

- 接收并解析请求
- 处理请求
- 数据渲染、响应请求

### 二、SpringMVC框架部署

#### 1. 创建Maven web项目

#### 2. 添加依赖

1. spring依赖：
   - spring-context（spring核心包）
   - spring-aspects（aop）
   - spring-jdbc（mybatis整合，该阶段可以先不导）
2. spring mvc依赖：
   - spring-web（spring对web的支持）
   - spring-webmvc（spring mvc框架的jar包）
3. 单元测试：
   - spring-test

```xml
<!--spring依赖版本，统一管理-->
<properties>
    <spring.version>5.2.13.RELEASE</spring.version>
</properties>

<dependencies>
    <!--spring依赖-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>${spring.version}</version>
    </dependency>
</dependencies>
```

#### 3. 创建SpringMVC配置文件

- 在resource目录下创建名为spring-servlet.xml的配置文件
- 添加MVC命名空间

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--声明使用注解配置-->
    <context:annotation-config/>
    <!--声明Spring工厂注解的扫描范围-->
    <context:component-scan base-package="com.feng"/>

    <!--声明mvc使用注解驱动-->
    <mvc:annotation-driven/>

</beans>
```

#### 4. web.xml中配置前端控制器

> SpringMVC提供了一个名为DispatcherServlet的类（SpringMVC中央处理器，也就是前端控制器），用于拦截用户请求交由SpringMVC处理
>
> 在 web.xml中完成配置，因为DispatcherServlet类是SpringMVC提供的第三方类，不能直接在其上注解配置

```xml
<!--配置SpringMVC前端控制器（中央控制器）-->
<servlet>
    <servlet-name>SpringMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!--加载Servlet初始化参数：加载配置文件-->
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring-servlet.xml</param-value>
    </init-param>
    <!--tomcat服务器初始化时创建该servlet实例-->
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>SpringMVC</servlet-name>
    <!--DispatcherServlet拦截的资源路径-->
    <url-pattern>/*</url-pattern>
</servlet-mapping>
```

### 三、SpringMVC框架使用

> 在SpringMVC中，我们把接收用户请求、处理用户请求的类称之为Controller（控制器）

#### 1. 创建控制器

##### 1.1 创建控制器类

- 创建controller层的包com.feng.controller（需要在Spring注解扫描范围内）
- 在包下创建java类（无需做任何继承和实现，也就是不用再继承HttpServlet）
- 在类上添加==@Controller==注解，声明此类为SpringMVC的控制器
- 在类上添加==@RequestMapping("url")==注解，声明该控制器的请求url

```java
@Controller
@RequestMapping("/book")
public class BookController {
}
```

##### 1.2 定义处理请求方法

- 在一个控制器类中可以定义多个方法处理不同的请求（添加、查询等），不再需要写各种请求的转发
- 在每个方法上添加==@RequestMapping("url")==用于声明当前方法请求的url

```java
@Controller
@RequestMapping("/book")
public class BookController {

    @RequestMapping("/add")
    public void add(){
        System.out.println("add book......");
    }

    @RequestMapping("/list")
    public void list(){
        System.out.println("list book......");
    }
}
```

##### 1.3 控制器访问

- http://localhost:8080/SpringMVCTemplate/book/add

- http://localhost:8080/SpringMVCTemplate/book/list

#### 2. 静态资源配置

> 静态资源：项目中的HTML、CSS、js、图片、字体等

##### 2.1 前端控制器中的配置

> 在前端控制器中做了如下拦截所有资源的配置：

```xml
<servlet-mapping>
    <servlet-name>SpringMVC</servlet-name>
    <!--DispatcherServlet拦截的资源路径-->
    <url-pattern>/*</url-pattern>
</servlet-mapping>
```

##### 2.2 /*和/的区别

- /* 拦截所有HTTP请求，包括.jsp的请求，都做为控制器类的请求路径来处理
- / 拦截所有的HTTP请求，但不包括.jsp的请求，也不会放行静态资源的请求（html、CSS、js、图片）

##### 2.3 静态资源放行配置

> 在SpringMVC配置文件spring-servlet.xml，添加如下静态资源放行配置
>
> - mapping属性：请求url
> - location属性：文件夹路径（存放静态资源）

```xml
<!--静态资源放行配置-->
<mvc:resources mapping="/css/**" location="/css/"/>
<mvc:resources mapping="/imgs/**" location="/imgs/"/>
<mvc:resources mapping="/js/**" location="/js/"/>
<mvc:resources mapping="/pages/**" location="/pages/"/>
```

#### 3. 前端数据提交到控制器

##### 3.1 表单提交

> 表单提交：输入框需要提供name属性，SpringMVC控制器通过name属性值进行取值

```html
<body>
    <h3>表单提交</h3>
    <form action="book/add" method="post">
        <p>图书名称<input type="text" name="bookName"/></p>
        <p>图书作者<input type="text" name="bookAuthor"/></p>
        <p>图书价格<input type="text" name="bookPrice"/></p>
        <p><input type="submit" value="提交"/></p>
    </form>
</body>
```

##### 3.2 超链接提交

```html
<a href="/book/add?bookName=Java">超链接提交</a>
```

##### 3.3 AJAX提交

> AJAX提交：请求行、请求头、请求体都可以用来传值

```html
<h3>AJAX提交</h3>
<input type="button" value="ajax提交" id="btn1"/>
<script type="text/javascript" src="js/jquery-3.4.1.min.js"></script>
<script type="text/javascript">
    $("#btn1").click(function () {
        var obj = {};
        obj.bookName = "Java";
        obj.bookAuthor="张三";
        obj.bookPrice=3.33;

        $.ajax({
            url:"book/add",
            type:"post",
            headers:{

            },
            contentType:"application/json",
            data:obj,
            success:function (res) {
                console.log(res);
            }
        });
    });
</script>
```

#### 4. 控制器接收前端数据

##### 4.1 请求行传值

> 前端提交形式：
>
> - 表单提交（无论post、get提交都是在请求行中传值，只是post在请求行中看不到）
> - URL（超链接）提交
> - $.ajax()请求的url传值
> - $.post()/$.get()中的{}传值

==@RequestParam==注解用于接收请求行传递的数据

- 前端提交数据：

  ```html
  <form action="book/add" method="post">
      <p>图书名称<input type="text" name="bookName"/></p>
      <p>图书作者<input type="text" name="bookAuthor"/></p>
      <p>图书价格<input type="text" name="bookPrice"/></p>
      <p><input type="submit" value="提交"/></p>
  </form>
  ```

- 控制器**使用注解**接收数据：

  ==注意==：**如果在控制器方法中接收数据的参数名和请求行传值的key一致，则@RequestParam注解可省略**
  
  ```java
  /**
   * 接收请求行数据
   */
  @RequestMapping("/add")
  public void add(@RequestParam("bookName") String name,
                  @RequestParam("bookAuthor") String author,
                  @RequestParam("bookPrice") double price){
      System.out.println("add book......");
      System.out.println("书名："+name);
      System.out.println("作者："+author);
      System.out.println("价格："+price);
  }
  ```

- 控制器**使用对象**接收数据：

  如果表单提交的信息刚好是一个对象的属性，则可以直接使用对象接收，

  ==注意==：**提交的数据的key 必须与对象的属性名一致**

  ```java
  @RequestMapping("/add")
  public void add(Book book){
      System.out.println("add book......");
      System.out.println("书名："+book.getBookName());
      System.out.println("作者："+book.getBookAuthor());
      System.out.println("价格："+book.getBookPrice());
  }
  ```

##### 4.2 请求头传值

> 前端提交形式：
>
> - ajax封装请求头数据
>
>   ```javascript
>   $.ajax({
>   	...,
>   	headers:{
>   	                              
>   	},
>   	...
>   })
>   ```

==@RequestHeader==注解用于接收请求头传递的数据

- 前端提交数据

  ```html
  <h3>AJAX提交</h3>
  <input type="button" value="ajax提交" id="btn1"/>
  <script type="text/javascript" src="js/jquery-3.4.1.min.js"></script>
  <script type="text/javascript">
      $("#btn1").click(function () {
          $.ajax({
              url:"book/list",
              type:"post",
              headers:{
                  token:"xixihaha"
              },
              success:function (res) {
                  console.log(res);
              }
          });
      });
  </script>
  ```

- 控制器接收数据

  ```java
  /**
   * 接收请求头数据
   */
  @RequestMapping("/list")
  public void list(@RequestHeader("token") String token){
      System.out.println("list book......");
      System.out.println("token:"+token);
  }
  ```

##### 4.3 请求体传值

> 前端提交形式：
>
> - ajax封装请求体数据
>
>   ```javascript
>   $.ajax({
>   	...,
>   	contentType:"application/json",
>       data:obj,
>   	...
>   })
>   ```

==@RequestBody==注解用于接收请求体传递的数据

- 前端传递数据

  ```html
  <h3>AJAX提交（请求体传值）</h3>
  <input type="button" value="ajax提交2" id="btn2"/>
  <script type="text/javascript" src="js/jquery-3.4.1.min.js"></script>
  <script type="text/javascript">
      $("#btn2").click(function () {
          var obj = {};
          obj.bookName = "Java";
          obj.bookAuthor="张三";
          obj.bookPrice=3.33;
  
          $.ajax({
              url:"book/update",
              type:"post",
              contentType:"application/json",
              data:JSON.stringify(obj),//将对象转换成JSON格式
              success:function (res) {
                  console.log(res);
              }
          });
      });
  </script>
  ```

- 后端接收数据

  > @RequestBody 将前端请求体提交的JSON格式数据转换成功Java对象，依赖于jackson包

  导入依赖：

  ```xml
  <!--jackson依赖-->
  <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.12.1</version>
  </dependency>
  ```

  控制器：

  ```java
  /**
   * 接收请求体数据
   */
  @RequestMapping("/update")
  public void update(@RequestBody Book book){
      System.out.println("update book......");
      System.out.println(book);
  }
  ```

#### 5. 控制器响应前端请求

##### 5.1 响应同步请求

> 同步请求：form表单、超链接
>
> 将处理同步请求的方法的返回类型，定义为String或者ModelAndView，以实现页面的跳转

- **返回类型为String**：

  **转发：**

  直接返回跳转到的页面路径，不加”/“表示从当前路径跳转，加”/“表示从根路径跳转

  ```java
  @RequestMapping("/add")
  public String add(@RequestParam("bookName") String name,
                  @RequestParam("bookAuthor") String author,
                  @RequestParam("bookPrice") double price){
      System.out.println("add book......");
      //如何跳转到指定的页面呢？
      return "/tips.jsp";
  }
  ```

  **重定向：**

  在页面路径前加上 redirect: 前缀

  ```java
  @RequestMapping("/add")
  public String add(@RequestParam("bookName") String name,
                    @RequestParam("bookAuthor") String author,
                    @RequestParam("bookPrice") double price){
      System.out.println("add book......");
      //如何跳转到指定的页面呢？
      return "redirect:/tips.jsp";
  }
  ```

- **返回类型为ModelAndView**：

  **转发：**

  创建ModelAndView对象并返回，参数为跳转页面路径

  ```java
  @RequestMapping("/add")
  public ModelAndView add(@RequestParam("bookName") String name,
                          @RequestParam("bookAuthor") String author,
                          @RequestParam("bookPrice") double price){
      System.out.println("add book......");
      //如何跳转到指定的页面呢？
      ModelAndView modelAndView = new ModelAndView("/tips.jsp");
      return modelAndView;
  }
  ```

  **重定向：**

  在页面路径前加上 redirect: 前缀

  ```java
  @RequestMapping("/add")
  public ModelAndView add(@RequestParam("bookName") String name,
                          @RequestParam("bookAuthor") String author,
                          @RequestParam("bookPrice") double price){
      System.out.println("add book......");
      //如何跳转到指定的页面呢？
      ModelAndView modelAndView = new ModelAndView(" redirect:/tips.jsp");
      return modelAndView;
  }
  ```

##### 5.2 同步请求的数据传递

> 对应同步请求的转发响应，我们可以传递参数到转发的页面

- **返回类型为String**：

  ```java
  /*
      数据传递：
      1.在方法参数中添加一个Model类型的参数（相当于response对象）
      2.在return页面之前，向model中添加键值对，该键值对会传递到下个页面
      3.除了使用model对象，也可以直接使用HttpServletResponse对象
   */
  @RequestMapping("/add")
  public String add(@RequestParam("bookName") String name,
                    @RequestParam("bookAuthor") String author,
                    @RequestParam("bookPrice") double price,
                    Model model){
      System.out.println("add book......");
      //如何跳转到指定的页面呢？
      model.addAttribute("book",new Book(1,"Java","老张",2.22));
      return "/tips.jsp";
  }
  ```

- **返回类型为ModelAndView**：

  ```java
  /*
      数据传递：
      1.创建ModelAndView对象
      2.在ModelAndView对象中添加键值对，该键值对会传递到下个页面
   */
  @RequestMapping("/add2")
  public ModelAndView add2(@RequestParam("bookName") String name,
                           @RequestParam("bookAuthor") String author,
                           @RequestParam("bookPrice") double price){
      System.out.println("add2 book......");
      //如何跳转到指定的页面呢？
      ModelAndView modelAndView = new ModelAndView("/tips.jsp");
      modelAndView.addObject("book",new Book(1,"Java","老张",2.22));
      return modelAndView;
  }
  ```

##### 5.3 响应异步请求

> 异步请求：ajax请求（前后端分离项目，所有请求都是异步请求）

**方式1：使用response中的输出流进行响应**

- 控制器方法的返回类型为**void**
- 在方法参数中增加 **HttpServletResponse resp** 参数
- 在方法中通过resp获取输出流，使用流响应ajax请求

```java
/**
 * 响应异步请求
 * 接收请求体数据
 */
@RequestMapping("/update")
public void update(@RequestBody Book book, HttpServletResponse resp) throws IOException {
    System.out.println("update book......");
    System.out.println(book);

    //使用jackson的转换工具，将java对象转成JSON字符串
    String s = new ObjectMapper().writeValueAsString(book);
    resp.setCharacterEncoding("utf-8");
    resp.setContentType("application/json");
    PrintWriter out = resp.getWriter();
    out.print(s);
    out.flush();
    out.close();
}
```

**方式2：直接在控制器方法返回响应的对象**

- 控制器方法的返回类型设置为，响应给ajax请求的对象类型（或List集合）
- 在控制器方法前添加注解：==@ResponseBody==，将返回的对象转换成JSON格式响应给ajax请求
- 如果一个控制器类中所有方法都是响应ajax请求，则直接在类上添加==@ResponseBody==注解即可

```java
/**
 * 响应异步请求
 * 接收请求体数据
 */
@RequestMapping("/update")
@ResponseBody
public List<Book> update(@RequestBody Book book) throws IOException {
    System.out.println("update book......");
    System.out.println(book);
    
    //方式二：SpringMVC特别提供的，直接将返回类型定义为返回的对象（或List集合）
    List<Book> books = new ArrayList<Book>();
    books.add(new Book(1,"Java","老张",2.22));
    books.add(new Book(2,"C++","老李",3.33));
    return books;
}
```

#### 6. 解决中文乱码问题

##### 6.1 前端编码

- JSP页面（同步交互）

  统一设置前端编码：

  ```jsp
  <%@ page contentType="text/html;charset=UTF-8" language="java" pageEncoding="utf-8" %>
  ```

- HTML页面（异步交互）

  统一设置前端编码：

  ```html
  <meta charset="UTF-8">
  ```

##### 6.2 服务器编码

- tomcat/conf/server.xml 中配置：

  ```xml
  <Connector port="8080" protocol="HTTP/1.1"
                 connectionTimeout="20000"
                 redirectPort="8443" URIEncoding="UTF-8"/>
  ```

##### 6.3 SpringMVC编码

- 在web.xml中配置SpringMVC编码过滤器的编码方式

  ```xml
  <!--设置SpringMVC编码（配置编码过滤器）-->
  <filter>
      <filter-name>EncodingFilter</filter-name>
      <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
      <init-param>
          <param-name>encoding</param-name>
          <param-value>utf-8</param-value>
      </init-param>
      <init-param>
          <param-name>forceEncoding</param-name>
          <param-value>true</param-value>
      </init-param>
  </filter>
  <filter-mapping>
      <filter-name>EncodingFilter</filter-name>
      <url-pattern>/*</url-pattern>
  </filter-mapping>
  ```

### 四、SpringMVC请求处理原理

#### 1. 请求处理流程

> SpringMVC通过前端控制器（DispatcherServlet）拦截并处理用户请求的流程

![image-20220501211804789](..\image\image-20220501211804789.png)

① 前端发送请求被前端控制器DispatcherServlet拦截

② 前端控制器调用处理器映射器HandlerMapping对请求URL进行解析，解析之后返回调用链给前端控制器

③ 前端控制器调用处理器适配器处理调用链

④ 处理器适配器==基于反射==通过==适配器设计模式==完成处理器Handler（也就是控制器：Controller类的方法）的调用处理用户请求

⑤ 处理器适配器将Handler返回的视图和数据信息封装成ModelAndView响应给前端控制器

⑥ 前端控制器调用视图解析器ViewResolver对ModelAndView进行解析，将解析结果（视图资源和数据）响应给前端控制器

⑦ 前端控制器调用视图view组件将数据进行渲染，将渲染结果（静态视图）响应给前端控制器

⑧ 前端控制器响应用户请求

#### 2. 核心组件介绍

> SpringMVC核心组件相互协同，完成请求的接收和响应

- **DispatcherServlet** ：前端控制器、总控制器、中央处理器
  - 作用：接收请求，协同各组件工作，响应请求
- **HandlerMapping** ：处理器映射
  - 作用：负责根据用户请求的URL找到对应的Handler（控制器，也就是Controller类）
  - ==可配置== SpringMVC提供多个处理器映射的实现，可以根据需要进行配置
- **HandlerAdapter** ：处理器适配器
  - 作用：按照处理器映射器解析的用户请求的调用链，通过适配器模式完成处理器Handler的调用
- **Handler** ：处理器/控制器的方法
  - Controller类方法，由工程师根据业务需求进行开发
  - 作用：处理请求
- **ModelAndView** ：视图模型
  - 作用：用于封装Handler返回的数据以及响应的视图
  - ModelAndView = Model + View
- **ViewResolver** ：视图解析器
  - 作用：对ModelAndView进行解析
  - ==可配置== SpringMVC提供多个视图解析器的实现，可以根据需要进行配置
- **View** ：视图
  - 作用：完成数据渲染

#### 3. 处理器映射配置

> 不同的处理器映射器对URL处理的方式也不相同，使用对应的处理器映射器之后，我们的**前端请求规则也需要发生相应的变化**
>
> SpringMVC提供的处理器映射器（官方推荐的两种）：
>
> - BeanNameUrlHandlerMapping ：根据控制器的ID访问控制器
>
>   - 需要将controller类名首字母改小写
>
>   - controller类可以不配置@RequestMapping，请求url为控制器id（首字母改小写）
>
> - SimpleUrlHandlerMapping ：根据控制器配置的URL访问（默认使用）
>
>   - 需要配置@RequestMapping，声明controller类的url

配置处理器映射器：

**在SpringMVC的配置文件中通过bean标签声明处理器映射器**

- 配置BeanNameUrlHandlerMapping：

  ```xml
  <!--声明处理器映射器，使用BeanNameUrlHandlerMapping-->
  <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
  ```

- 配置SimpleUrlHandlerMapping：

  ```xml
  <!--使用默认的处理器映射器：SimpleUrlHandlerMapping（不配置默认使用这个）-->
  <bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
  ```

#### 4. 视图解析器配置

> Spring提供了多个视图解析器：
>
> - UrlBasedViewResolver：
>   - 配置后可以为视图自动添加前缀和后缀
>     (本来controller是传回"/tips.jsp"字符串，配置后只需要传"tips")
>   - 需要依赖于jstl
>   - 还需要配置属性viewClass，依赖这个类完成解析
> - InternalResourceViewResolver：
>   - 是UrlBasedViewResolver的子类
>   - 不需要依赖jstl，不需要配置属性viewClass，前后缀配置方式和父类一样
>   - 比父类配置便捷一点，如果需要配置视图解析器，一般就配这个

- UrlBasedViewResolver

  添加依赖

  ```xml
  <dependency>
    <groupId>jstl</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
  </dependency>
  ```

  配置视图解析器

  ```xml
  <!--声明使用视图解析器，使用-->
  <bean id="viewResolver" class="org.springframework.web.servlet.view.UrlBasedViewResolver">
      <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
      <!--自动为视图添加前缀和后缀（本来返回的"/tips.jsp"变成了"tips"）-->
      <property name="prefix" value="/"/>
      <property name="suffix" value=".jsp"/>
  </bean>
  ```

- InternalResourceViewResolver

  ```xml
  <!--声明使用视图解析器，使用InternalResourceViewResolver-->
  <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
      <!--自动为视图添加前缀和后缀（本来返回的"/tips.jsp"变成了"tips"）-->
      <property name="prefix" value="/"/>
      <property name="suffix" value=".jsp"/>
  </bean>
  ```

### 五、日期格式处理

#### 1. 默认处理方式

> 如果前端需要输入日期数据，在控制器中转换成Date对象，SpringMVC要求前端输入的日期格式必须为：
> yyyy/MM/dd

#### 2. 自定义日期转换器

> 如果甲方要求日期格式必须为指定的格式，而这个指定格式SpringMVC不接受，该如何处理？
>
> - 自定义日期转换器
>   - 创建一个类实现Converter接口，泛型指定从什么类型转换到什么类型
>   - 实现covert方法

```java
/**
 * 自定义日期转换类
 */
public class MyDateConverter implements Converter<String, Date> {

    public Date convert(String dateString) {
        //定义日期格式
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日");

        //获取日期字符串的日期分隔符
        char c = dateString.charAt(4);
        try {
            //如果第一个分隔符是'年'，说明符合格式，返回date
            if (c == '年'){
                return sdf.parse(dateString);
            }
            //否则是其他分隔符类型（例如："/" "." "-"）
            //将原字符串根据分隔符切割
            String separator = String.valueOf(c);
            String[] s;
            if(".".equals(separator)){
                //如果分隔符是特殊符号："." ，则需要特殊处理
                s = dateString.split("\\.");
            }else {
                s = dateString.split(separator);
            }
            //如果分隔出正好三条字符串，说明格式正确，返回date
            if(s.length == 3){
                String convert = s[0] + "年" + s[1] + "月" + s[2] + "日";
                //返回正确格式Date
                return sdf.parse(convert);
            }
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

#### 3. 配置自定义转换器

> 在SpringMVC核心配置文件spring-servlet.xml中完成配置

```xml
<!--声明mvc使用注解驱动 并配置转换器工厂-->
<mvc:annotation-driven conversion-service="converterFactory"/>

<!--配置自定义日期转换器-->
<bean id="converterFactory"
      class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
    <property name="converters">
        <set>
            <bean class="com.feng.utils.MyDateConverter"/>
        </set>
    </property>
</bean>
```

### 六、文件上传与下载

> 案例：添加图书，同时提交图书的封面图片

#### 1. 文件上传实现

##### 1.1 前端提交文件

- 表单提交方式必须为post
- 表单enctype属性设置为 multipart/form-data

```jsp
<form action="book/add" method="post" enctype="multipart/form-data">
    <p>图书名称<input type="text" name="bookName"/></p>
    <p>图书作者<input type="text" name="bookAuthor"/></p>
    <p>图书价格<input type="text" name="bookPrice"/></p>
    <p>图书封面<input type="file" name="imgFile"/></p>
    <p><input type="submit" value="提交"/></p>
</form>
```

##### 1.2 控制器接收文件

> SpringMVC处理上传文件需要借助于 CommonsMultipartResolver 文件解析器

- 添加依赖：commons-io、commons-fileupload

  ```xml
  <!--文件上传依赖-->
  <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.2</version>
  </dependency>
  <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.4</version>
  </dependency>
  ```

- 在spring-servlet.xml中配置文件解析器

  ```xml
  <!--配置文件解析器-->
  <bean id="multipartResolver"
        class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
      <!--最大上传文件大小-->
      <property name="maxUploadSize" value="10240000"/>
      <!--服务器最大缓存大小-->
      <property name="maxInMemorySize" value="102400"/>
      <!--默认文件编码-->
      <property name="defaultEncoding" value="utf-8"/>
  </bean>
  ```

- 控制器接收文件

  ```java
  @RequestMapping("/add")
  public String addBook(Book book, MultipartFile imgFile, HttpServletRequest request) throws IOException {
      System.out.println("add book......");
  
      //imgFile就表示要上传的文件
      //1. 截取上传文件的后缀名，生成新的文件名
      String originalFilename = imgFile.getOriginalFilename();
      String ext = originalFilename.substring(originalFilename.lastIndexOf("."));//.jpg
      String fileName = System.currentTimeMillis() + ext;
  
      //2. 获取imgs目录在服务器的绝对路径
      String dir = request.getServletContext().getRealPath("imgs");
      String savePath = dir + "/" + fileName;
      System.out.println(savePath);
  
      //3. 保存文件到imgs目录中
      imgFile.transferTo(new File(savePath));
  
      //4. 将文件访问路径赋值到book对象（注意，访问路径是服务器的相对路径，而不能是绝对路径savePath）
      book.setBookImg("imgs/"+fileName);
  
      System.out.println(book);
  
      //5. 调用service将book保存到数据库（此处省略）
      return "tips";
  }
  ```

#### 2. 文件下载实现

##### 2.1 显示文件列表

> 前端代码省略，后端需要将文件目录下的所有文件名返回到前端，交由前端进行展示

```java
@RequestMapping("/list")
@ResponseBody
public String[] listImages(HttpServletRequest request){
    //从imgs目录下获取所有的文件信息
    String dir = request.getServletContext().getRealPath("imgs");
    File imgDir = new File(dir);
    String[] fileNames = imgDir.list();

    //响应异步请求
    return fileNames;
}
```

##### 2.2 下载文件

> 前端代码省略，后端需要将文件拷贝到前端，提供下载

```java
@RequestMapping("/downLoad")
public void downLoadImage(String imageName,
                          HttpServletRequest request, HttpServletResponse response) throws IOException {
    //根据文件名参数，获取该文件的绝对路径
    String dir = request.getServletContext().getRealPath("imgs");
    String filePath = dir + "/" +imageName;

    //输入流指向文件
    FileInputStream is = new FileInputStream(filePath);

    //设置响应类型
    response.setContentType("image/jpg");
    //设置响应头，下载时显示文件名
    response.addHeader("Content-Disposition","attachment;filename="+imageName);

    //将文件响应到客户端（输出流即从response获取）
    IOUtils.copy(is,response.getOutputStream());
}
```

### 七、统一异常处理

> 在我们的应用系统运行过程中，可能由于运行环境、用户操作、资源不足等各方面的原因导致系统出现异常（HTTP状态异常、Java代码异常Exception）；如果系统出现了异常，这些异常将会通过浏览器呈现给用户
> ，而这种异常的显示是没有必要的，因此我们可以在服务器进行特定的处理——当系统出现异常后，呈现给用户一个统一的、可读的异常提示页面。

#### 1. HTTP异常统一处理

> 示例：HTTP Status 404

- 创建一个用于进行异常提示的页面：404.jsp

  ```jsp
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
  <h1 style="color: pink">哎呀，页面消失了......o(╥﹏╥)o......</h1>
  </body>
  </html>
  ```

- 在web.xml中进行配置

  ```xml
  <!--配置HTTP异常统一处理（该配置不依赖于SpringMVC）-->
  <error-page>
      <error-code>404</error-code>
      <location>/404.jsp</location>
  </error-page>
  ```

#### 2. Java代码异常处理

##### 2.1 基于Servlet-api处理

- 创建异常提示页面：err.jsp

- 在web.xml中进行配置

  ```xml
  <error-page>
      <exception-type>java.lang.NumberFormatException</exception-type>
      <location>/err.jsp</location>
  </error-page>
  ```

##### 2.2 基于SpringMVC处理

- 使用异常处理类统一处理

  ```java
  @ControllerAdvice
  public class MyExceptionHandler {
  
      @ExceptionHandler(NumberFormatException.class)
      public String formatHandler(){
          return "err1";
      }
  
      @ExceptionHandler(NullPointerException.class)
      public String nullHandler(){
          return "err2";
      }
  }
  ```

### 八、SpringMVC拦截器

#### 1. 拦截器介绍

> SpringMVC提供的拦截器类似于Servlet-api中的过滤器，可以对控制器的请求进行拦截实现相关的预处理和后处理。**拦截器并不能取代过滤器**

- 过滤器
  - 是Servlet规范的一部分，所有的web项目都可以使用
  - 过滤器在web.xml配置（可以使用注解），能够拦截所有web请求
- 拦截器
  - 是SpringMVC框架的实现，只有在SpringMVC框架中才能使用
  - 拦截器在SpringMVC配置文件中配置，不会拦截SpringMVC放行的资源（jsp\html\css...）

#### 2. 自定义拦截器

**1. 创建拦截器**

> 创建一个类实现HandlerInterceptor接口，实现预处理和后处理方法

```java
public class MyInterceptor1 implements HandlerInterceptor {

    //预处理
    public boolean preHandle(HttpServletRequest request, 
                             HttpServletResponse response, 
                             Object handler) throws Exception {
        System.out.println("预处理...");
        Enumeration<String> parameterNames = request.getParameterNames();
        while (parameterNames.hasMoreElements()){
            String element = parameterNames.nextElement();
            if("bookName".equals(element)){
                return true;
            }
        }
        response.setStatus(400);
        return false;
    }

    //后处理
    public void postHandle(HttpServletRequest request, 
                           HttpServletResponse response, Object handler, 
                           ModelAndView modelAndView) throws Exception {
        System.out.println("后处理...");
        modelAndView.addObject("query","这是拦截器后处理添加的数据");
    }
}
```

**2. 配置拦截器**

> 一个拦截器可以配置多个拦截路径

```xml
<!--配置拦截器-->
<mvc:interceptors>
    <mvc:interceptor>
        <!--可以配置多个路径-->
        <mvc:mapping path="/book/query"/>
        <mvc:mapping path="/book/add"/>
        <mvc:mapping path="/student/**"/>
        <bean class="com.feng.utils.MyInterceptor1"/>
    </mvc:interceptor>
</mvc:interceptors>
```

#### 3. 拦截器链

> 将多个拦截器按照一定顺序构成一条执行链

![image-20220508195646787](..\image\image-20220508195646787.png)

**1. 编写多个拦截器类**

**2. 配置同一路径下多个拦截**

```xml
<!--配置拦截器链-->
<mvc:interceptors>
    <mvc:interceptor>
        <!--拦截器1-->
        <mvc:mapping path="/book/query"/>
        <bean class="com.feng.utils.MyInterceptor1"/>
    </mvc:interceptor>
    <mvc:interceptor>
        <!--拦截器2-->
        <mvc:mapping path="/book/query"/>
        <bean class="com.feng.utils.MyInterceptor2"/>
    </mvc:interceptor>
</mvc:interceptors>
```

### 九、笔记与代码解释

> 对应关系：
>
> - 第一章至第五章笔记 对应代码：SpringMVCTemplate
> - 第六章至第八章笔记 对应代码：SpringMVCTemplate2

