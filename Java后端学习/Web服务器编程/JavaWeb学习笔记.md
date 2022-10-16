# JavaWeb学习笔记

### 一、HTTP协议

#### 1. 概念

​    HTTP：Hyper Text Transfer Protocol 超文本传输协议

​	传输协议：定义了，客户端和服务器端通信时，发送数据的格式

​	 特点：

1. 基于TCP/IP的高级协议
2. 默认端口号：80
3. 基于请求/响应模型的：一次请求对应一次响应
4. ==无状态的：每次请求之间相互独立，不能交互数据==

​      历史版本：

* 1.0版本：每次请求响应都会建立新的连接
* 1.1版本：连接在一段时间内可以复用，提高效率，节省资源

#### 2. 请求消息数据格式

1. **请求行：**

​         格式：请求方式   请求url   请求协议/版本

​         示例：Get  /login.html  HTTP/1.1

​         请求方式：HTTP协议中有7种请求方式，常用的有2种：

* Get：
  1. ==请求参数在请求行中，在url后==
  2. 请求的url长度有限制
  3. 不太安全

* Post：
  1. ==请求参数在请求体中==
  2. 请求的url长度没有限制
  3. 相对安全

2. **请求头：**

​		 格式（键值对形式）：请求头名称：求情头值

​         **几个常见的请求头**：

​	      **Host**：表示请求的主机名；

​          **User- Agent**：浏览器版本，例如Chrome浏览器的标识类似Moilla/5.0 ；

​	  	**Accept**：表示浏览器能接收的资源类型，如：text/* , image/*;

​          **Accept-Language**：表示浏览器偏好的语言，服务器可以据此返回不同语言的网页;

​	  	**Accept-Encoding**：表示浏览器可以支持的压缩类型，例如gzip, deflate等。

​          **Referer**：告诉服务器，当前请求从哪来，如：http：//localhost/login.html 

​          **Referer的作用**：1.防盗链、2.统计工作

3. **请求空行：**

​		 空行（用于分开请求头和请求体）

4. **请求体（正文）：**

​		 封装POST请求消息的请求参数（GET请求没有请求体，请求参数放在请求行中）

#### 3. 响应消息数据格式

1. **响应行：**
   格式：协议版本 响应状态码 状态码描述
   例：HTTP/1.1 200 OK

   响应状态码分类：

   | 状态码分类 | 说明                                                         |
   | ---------- | ------------------------------------------------------------ |
   | 1XX        | 响应中——临时状态码，表示请求已经接受，告诉客户端应该继续请求或者如果它已经完成则忽略它 |
   | 2XX        | 成功―—表示请求已经被成功接收，处理已完成                     |
   | 3XX        | 重定向—―重定向到其它地方:它让客户端再发起—个请求以完成整个处理 |
   | 4XX        | 客户端错误——处理发生错误，责任在客户端，如:客户端的请求一个不存在的资源，客户端未被授权，禁止访问等 |
   | 5XX        | 服务器端错误——处理发生错误，责任在服务端，如:服务端抛出异常，路由出错，HTTP版本不支持等 |

   常见响应状态码：

   | 响应状态码 | 状态码英文描述                  | 解释                                                         |
   | ---------- | ------------------------------- | ------------------------------------------------------------ |
   | 200        | OK                              | 客户端请求成功，即处理成功，这是我们最想看到的状态码         |
   | 302        | Found                           | 指示所请求的资源已移动到由Locati on响应头给定的URL,浏览器会自动重新访问到这个页面 |
   | 304        | Not Modified                    | 告诉客户端，你请求的资源至上次取得后，服务端并未更改,你直接用你本地缓存吧。隐式重定向 |
   | 400        | Bad Request                     | 客户端请求有语法错误，不能被服务器所理解                     |
   | 403        | Forbidden                       | 服务器收到请求，但是拒绝提供服务,比如:没有权限访问相关资源   |
   | 404        | Not Found                       | 请求资源不存在，一般是URL输入有误，或者网站资源被删除了      |
   | 428        | Precondition Required           | 服务器要求有条件的请求，告诉客户端要想访问该资源，必须携带特定的请求头 |
   | 429        | TOO Many Requests               | 太多请求，可以限制客厂端请求某个资源的数量，配合Retry-After(多长时间后可以请求)响应头一起使用 |
   | 431        | Request Header Fields TOO Large | 请求头太大，服务器不愿意处理请求,因为它的头部字段太大。请求可以在减少请求头域的大小后重新提交。 |
   | 405        | Method Not Allowed              | 请求方式有误，比如应该用GET请求方式的资源，用了POST          |
   | 500        | Interna1 server Error           | 服务器发生不可预期的错误。服务器出异常了，赶紧看日志去吧     |
   | 503        | service Unavailable             | 服务器尚未准备好处理请求，服务器刚刚启动，还未初始化好       |
   | 511        | Network Authentication          | 客户端需要进行身份验证才能获得网络访问权限                   |

2. **响应头**：
   以键值对的形式，常见响应头：
   Content-Type：表示该响应内容的类型，例如：text/html,image/jpeg;
   Content-Length：表示该响应内容的长度(字节数) ;
   Content-Encoding：表示该响应压缩算法，例如：gzip;
   Cache-Control：指示客户端应如何缓存，例如：max-age=3od表示可以最多缓存300秒

3. **响应体**：
   存放响应数据内容，例如：html

### 二、Tomcat服务器

#### 1. 目录结构

1. bin：

   **bin目录主要是用来存放tomcat的命令，**主要有两大类，一类是以.sh结尾的（[linux](https://so.csdn.net/so/search?from=pc_blog_highlight&q=linux)命令），另一类是以.bat结尾的（windows命令）。很多环境变量的设置都在此处，例如可以设置JDK路径、tomcat路径 

   **startup 用来启动tomcat** 
   **shutdown 用来关闭tomcat** 
   **修改catalina可以设置tomcat的内存**

2. conf：

   **conf**：目录主要是用来存放tomcat的一些配置文件。

   **server.xml**：可以设置端口号、设置域名或IP、默认加载的项目、请求编码

   **web.xml**：可以设置tomcat支持的文件类型

   **注意**：conf文件夹下的web.xml是webapps中所有项目的web.xml的父亲，所有web应用的web.xml都会继承此文件中的内容。如果自己项目里的web.xml配置了一样的，会覆盖父亲。**所以不要在conf文件夹下的web.xml为某个特定的web应用进行特定的配置！**

   **context.xml**：可以用来配置数据源之类的
   **tomcat-users.xml**：用来配置管理tomcat的用户与权限
   在**Catalina目录**下：可以设置默认加载的项目

3. lib：

   **lib目录主要用来存放tomcat运行需要加载的jar包。** 
   例如，像连接数据库的jdbc的包我们可以加入到lib目录中来。

4. logs：

   logs目录用来存放tomcat在运行过程中产生的日志文件，非常重要的是在控制台输出的日志。（清空不会对tomcat运行带来影响） 
   在windows环境中，控制台的输出日志在catalina.xxxx-xx-xx.log文件中 
   在linux环境中，控制台的输出日志在catalina.out文件中

5. temp：

   temp目录用户存放tomcat在运行过程中产生的临时文件。（清空不会对tomcat运行带来影响） 

6. webapps：

   **webapps目录用来存放应用程序，**当tomcat启动时会去加载webapps目录下的应用程序。**可以以文件夹、war包、jar包的形式发布应用。** 
   **也可以把应用程序放置在磁盘的任意位置，在配置文件中映射好就行。**

7. work:

   **work目录用来存放tomcat在运行时的编译后文件，例如JSP编译后的文件。** 
   **清空work目录，然后重启tomcat，可以达到清除缓存的作用。**

#### 2. 项目结构

​		java动态项目目录结构：

​		--- 项目根目录：

​			 --- WEB-INF目录：

​                      --- web.xml：web项目的核心配置文件

​					  --- classes目录：放置java字节码文件的目录

​                      --- lib目录：放置依赖的jar包

​	         --- 静态资源

#### 3. 项目部署

1. 直接将项目放到webapps目录下即可。

   **简化部署**：将项目打成一 个war包，再将war包放置到webapps目录下。

   war包会自动解压缩

   最常用的方法

2. 配置conf/server. xm1文件

   在`<Host>`标签体中配置

   `<Context docBase="D: \hello" path=" /hehe" />`

   docBase：项目存放的路径

   path：虚拟目录

   不建议使用的方法

3. 在conf\Catalina\localhost 创建任意名称的xml文件。在文件中编写

   `<Context docBase="D: \hello" />` 

   虛拟目录：xml文件的名称

   这是一种热部署

#### 4. idea与Tomcat的相关配置

 1. IDEA会为每一个tomcat部署的项目单独建立一份配置文件：

    看控制台的log：Using CATALINA_BASE:   "C:\Users\26574\AppData\Local\JetBrains\IntelliJIdea2020.1\tomcat\Unnamed_MyWebApps""

    在idea中启动项目时，会根据这个配置文件运行Tomcat服务器，真正的Tomcat服务器的配置并不会改变，例如，在idea的Edit configurations设置中，将默认的8080端口改为80端口，真正的Tomcat服务器的配置的端口号仍然是8080

2. Tomcat部署的web项目对应着-->工作空间中web目录下的所有资源，发布web项目时，应将工作空间中web目录编译生成好的文件打包部署到Tomcat部署的web项目中。

#### 5. Maven项目集成tomcat

1. 方法一：在idea中点击ADD Configuration集成本地Tomcat

   ![image-20220327165645946](..\image\image-20220327165645946.png)

2. 方法二：在pom.xml中添加tomcat插件(目前只支持tomcat7)：

   ```xml
   <!--tomcat插件 集成tomcat-->
   <plugin>
     <groupId>org.apache.tomcat.maven</groupId>
     <artifactId>tomcat7-maven-plugin</artifactId>
     <version>2.2</version>
     <!--参数配置-->
     <configuration>
       <port>80</port><!--端口-->
       <path>/</path><!--访问路径-->
     </configuration>
   </plugin> 
   ```

   然后右键项目，使用MavenHelper插件启动tomcat：

   <img src="..\image\image-20220327170327952.png" alt="image-20220327170327952" style="zoom:80%;" />

#### 6. Maven依赖管理范围

1. 通过设置坐标的依赖范围(scope)，可以设置对应jar包的作用范围：编译环境、测试环境、运行环境。编译环境：jar包在编译环境有效，也就是在java目录下有效；测试环境：jar包在测试环境有效，也就是在test目录下有效；运行环境：jar包在运行环境有效，也就是在tomcat服务器中（打包发布到服务器运行的时候）有效

2. 依赖范围scope参数：

   | 依赖范围 | 编译classpath | 测试classpath | 运行classpath |           例子           |
   | -------- | :-----------: | :-----------: | :-----------: | :----------------------: |
   | compile  |       Y       |       Y       |       Y       |         logback          |
   | test     |       -       |       Y       |       -       |          Junit           |
   | provided |       Y       |       Y       |       -       |       servlet-api        |
   | runtime  |       -       |       Y       |       Y       |         jdbc驱动         |
   | system   |       Y       |       Y       |       -       |    存储在本地的jar包     |
   | import   |       -       |       -       |       -       | 引入DependencyManagement |

   scope默认值为：compile
   例子：
   由于tomcat服务器中已经自带servlet的jar包，所以设置scope参数为provided，使得打出的war包中不包含servlet的jar包，避免冲突

   ```xml
   <dependency>
     <groupId>javax.servlet</groupId>
     <artifactId>javax.servlet-api</artifactId>
     <version>3.1.0</version>
     <scope>provided</scope>
   </dependency>
   ```

### 三、Servlet

#### 1. 概念

Servlet（Server Applet），全称Java Servlet。**是用Java编写的服务器端程序**。其主要功能在于交互式地浏览和修改数据，生成动态Web内容。狭义的Servlet指的是JavaEE的规范之一，也就是Java语言实现的一个接口，广义的Servlet是指任何实现了这个Servlet接口的类，一般情况下，人们将Servlet理解为后者。

**Servlet运行于支持Java的应用服务器中**。从实现上讲，Servlet可以响应任何类型的请求，但绝大多数情况下Servlet只用来扩展基于HTTP协议的Web服务器。

**Servlet对象是单例的。**

#### 2. 实现步骤

1. 方法一：实现Servlet接口，实现其所有方法：
   五大方法：

   **init()** ：初始化

   **destroy()**：被销毁

   getServletConfig()：获取servletconfig对象

   getServletInfo()：获取Servlet的一些信息：版本，作者等

   **service()**：提供服务

2. 方法二：继承GenericServlet类：GenericServlet将Servlet接口中其他的方法做了默认空实现，只将service( )方法作为抽象，将来定义Servlet类时，可以继承GenericServlet，实现service( )方法即可

3. **方法三**：**继承HttpServlet类**：HttpServlet是对**http协议**的一种封装，简化操作。HttpServlet实现了service( )方法，并且根据请求的不同调用对应请求的方法，将来定义Servlet类时只需要继承HttpServlet类并实现请求方法，如：**doGet()**、**doPost()**即可，此方法最常用

#### 3. 执行原理

1. 当服务器接受到客户端浏览器的请求后，会解析请求URL路径，获取访问的Servlet的资源路径

2. 查找web. xml文件，是否有对应的`<url-pattern>`标签体内容。

3. 如果有，则再找到对应的`<servlet-class>`全类名

4. tomcat会将对应的字节码文件加载进内存，并且创建其对象（反射）

5. 调用其方法

   <img src="..\image\SI$TDSK7PLBUJVERZU_%D}0.png" alt="SI$TDSK7PLBUJVERZU_%D}0"  />

6. 在Servlet3.0后，支持注解配置，不需要再在web. xml文件中配置servlet标签和servlet-mapping标签来配置url-pattern以及servlet-class全类名；

   注解配置，例：@WebServlet("/demo1")    引号内写资源虚拟路径url-pattern

#### 4. 生命周期

1. ==被创建==：执行==init()==方法，只执行一次

   + Servlet什么时候被创建？

     默认情况下，第一次被访问时Servlet被创建；

     **可以配置执行Servlet的创建时机**：

     在web.xml配置文件中在`<servlet>`标签下配置：

     1.第一次被访问时，创建：`<load-on-startup>`的值为负数

     2.在服务器启动时，创建：`<load-on-startup>`的值为0或正整数

     **也可以在注解中进行配置：**

     例如：@WebServlet(urlPatterns = "/demo1",loadOnStartup = 0)

   + Servlet的init()方法，**只执行一次**，说明一个==Servlet在内存中只存在一个对象==， **Servlet是单例的**

     多个用户同时访问时，**可能存在线程安全问题**。

     **解决：尽量不要在Servlet中定义成员变量。即使定义了成员变量，也不要对修改值**

2. ==提供服务==：执行==service()==方法，执行多次

   每次访问Servlet时，service()方法都会被调用一次

3. ==被销毁==：执行==destroy()==方法，只执行一次

   Servlet被销毁时执行，服务器关闭时，Servlet被销毁。

   只有服务器正常关闭时，才会执行destroy()方法（直接关闭黑窗口属于不正常关闭）

   destroy()方法在Servlet被销毁之前被执行，一般用于释放资源

#### 5. web.xml配置和注解配置

1. 在web.xml配置文件中的配置：

   配置`<servlet>`标签和`<servlet-mapping>`标签：

   例如：

   ```xml
   <!-- 配置Servlet -->
   <servlet>
   	<servlet-name>demo1</servlet-name>
   	<servlet-class>com.feng . ServletDemo1</servlet-class>
   </servlet>
   
   <servlet-mapping>
   	<servlet-name>demo1</servlet-name>
   	<ur1-pattern>/demo1</ur1-pattern>
   </servlet-mapping>
   ```

2. Servlet3.0后支持注解配置，可以不需要web.xml配置文件了，

   直接在Servlet类上使用@WebServlet注解进行配置，如下：

   urlpartten：Servlet访问路径

   **一个Servlet可以定义多个访问路径:**

   例如： @WebServlet({"/d4" , "/dd4" ,"/ddd4" })

   **urlPattern路径配置规则:**

   1. 精确匹配：

      配置路径：@WebServlet("/aaa")（同时可以：/aaa/bbb 多层路径，目录结构）

      访问路径：localhost:8080/web-demo/aaa
   
   2. 目录匹配：
   
      配置路径：@WebServlet("/user/*")
   
      访问路径：localhost:8080/web-demo/user/aaa 
                    或 localhost:8080/web-demo/user/bbb 等
   
   3. 扩展名匹配：
   
      配置路径：@WebServlet("*.do")
   
      访问路径：localhost:8080/web-demo/aaa.do
                    或 localhost:8080/web-demo/bbb.do 等
   
   4. 任意匹配：
   
      配置路径：@WebServlet("/") 或 @WebServlet("/*")
   
      访问路径：localhost:8080/web-demo/hehe 
                    或 localhost:8080/web-demo/haha 等
   
      **“/” 和 “/*” 的区别：**
   
      当我们的项目中的Servlet配置了“/"，会覆盖掉tomcat中web.xml的DefaultServlet，当其他的url-pattern都匹配不上时都会走这个Servlet，导致静态资源无法访问；
      当我们的项目中配置了“/*”，意味着匹配任意访问路径；
      **因此，尽量不要使用任意匹配！**

#### 6. 体系结构

GenericServlet类实现了Servlet接口：GenericServlet将Servlet接口中其他的方法做了默认空实现，只将service( )方法作为抽象

HttpServlet类继承了GenericServlet类：HttpServlet是对**http协议**的一种封装，简化操作。

#### 7. 请求响应对象

了解响应过程：

第一步：tomcat服务器会根据请求url中的资源路径，创建对应的Servlet类的对象。

第二步：tomcat服务器，会创建request和response两个对象，request对象中封装请求消息数据。

第三步：tomcat将request和response两个对象传递给service方法，并且调用service方法。

第四步：程序员，可以通过request对象获取请求消息数据，通过response对象设置响应消息数据。

第五步：服务器在给浏览器做出响应之前，会从response对象中拿程序员设置的响应消息数据。

##### 1. Request对象

1. ==Request 继承体系：==

   **ServletRequest接口**是Java提供的请求对象根接口；**HttpServletRequest接口**是java提供的对Http协议封装的请求对象接口，它继承了ServletRequest接口；**RequestFacade类**是Tomcat定义的实现类，它实现了HttpServletRequest接口。

   **结论：**

   1. Tomcat需要解析请求数据，封装为request对象，并且创建request对象传递到service方法中。这个过程是Tomcat自身完成的
   2. 使用request对象，只需查阅JavaEE API文档中的HttpServletRequest接口即可；因为Tomcat自身定义了该接口的实现类，并且自动地创建实现类对象传入到service、doGet、doPost方法中

2. ==Request 获取请求数据：==

   1. **获取请求行：**例：GET /request-demo/req1?username=zhangsan HTTP/1.1

      | 方法                          | 解释                         | 例子                                    |
      | ----------------------------- | ---------------------------- | --------------------------------------- |
      | String getMethod():           | 获取请求方式                 | GET                                     |
      | String getContextPath():      | 获取虚拟目录（项目访问路径） | /request-demo                           |
      | StringBuffer getRequestURL(): | 获取URL（统一资源定位符）    | http://localhost:8080/request-demo/req1 |
      | String getRequestURI():       | 获取URI（统一资源标识符）    | /request-demo/req1                      |
      | String getQueryString():      | 获取请求参数（Get方式）      | username=zhangsan&password=123          |

   2. **获取请求头：**

      方法：**String getHeader(String name)：**根据请求头名称，获取值

   3. **获取请求体（Post方式）：**

      调用以下方法获取请求体的输入流，根据输入流对象获取请求体信息：

      **ServletInputStream getInputStream()：**获取字节输入流

      **BufferedReader getReader()：**获取字符输入流（==常用于接收JSON数据==）

   4. **Get、Post方式统一获取请求参数的方法：**

      由于请求方式不同，导致获取请求参数的方式也不同，这样当业务逻辑相同的时候，还需要在两个方法中以不同的获取方式重复写逻辑相同的代码，很麻烦
      于是Request对象提供了通用的方式，获取请求参数

      原理：Request根据请求方式获取到请求参数后，将参数（字符串：username=zhangsan&hobby=1&hobby=2）分隔后，放入Map集合中，由于请求参数键值对存在一个键对应多个键值的情况，因此该Map集合键值是String[]的形式

      **相应方法：**

      **Map<String,String[]> getParameterMap()：**获取所有参数Map集合

      **String[] getParameterValues()：**根据名称获取参数值（数组）

      **String getParameter(String name)：**根据名称获取参数值（单个值）

      (这些方法的底层仍然是调用doGet、doPost各自的获取请求参数的方法)

   5. 请求参数中文乱码问题的解决

      1. POST请求解决方案：

         调用以下方法即可：

         req.setCharacterEncoding("utf-8"); 请求参数中含中文字符

      2. GET请求解决方案：

         tomcat7对浏览器发送的请求URL是以ISO-8859-1的编码方式解码的，而tomcat8之后则是以utf-8的方式解码，所以请求参数获取不会出现乱码
         
         对tomcat7的Get请求乱码的解决：
         
         方式一，通用方式：
         
         1. 获取请求参数后先以ISO-8859-1的方式编码为纯字节数组：
            String username = req.getParameter("username");
            byte[] bytes = username.getBytes("ISO-8859-1");
         
         2. 再以utf-8的方式将纯字节数组解码为字符串：
         
            username = new String(bytes,"utf-8")；
         
         方式二，URL编码方式（使用以下两个工具类的方法）：
         
         1. 编码：
            URLEncoder.encode(str,"utf-8");
         2. 解码：
            URLDecoder.decode("ISO-8859-1");

3. ==Request 请求转发：==

   1. 解释：请求转发(forward)：一种在服务器内部的资源跳转方式
      当一个资源(Servlet)获取请求并对请求数据做了一些处理后，可转发给另一个资源(Servlet)接着处理。
   
   2. **实现方式：
      req.getRequestDispatcher("资源B路径").forward(req,resp);**
   
   3. **请求转发资源间共享数据：**
   
      使用Request对象：
      **req.setAttribute(String name, Object o)：**以键值对的方式存储数据到request域中
      **req.getAttribute(String name)：**根据key，获取值
      **req.removeAttribute(String name)：**根据key，删除该键值对
   
   4. **请求转发的特点：**
   
      浏览器地址栏路径不发生变化；
   
      只能转发到当前服务器的内部资源；
   
      一次请求，可以在转发的资源间使用request共享数据。

##### 2. Response对象

1. ==Response继承体系：==

   **ServletResponse接口**是Java提供的请求对象根接口；**HttpServletResponse接口**是java提供的对Http协议封装的请求对象接口，它继承了ServletResponse接口；**ResponseFacade类**是Tomcat定义的实现类，它实现了HttpServletResponse接口。

   **结论：**

   1. Tomcat创建response对象传递到service方法中。这个过程是Tomcat自身完成的
   2. 使用Response对象，只需查阅JavaEE API文档中的HttpServletResponse接口即可；因为Tomcat自身定义了该接口的实现类，并且自动地创建实现类对象传入到service、doGet、doPost方法中

2. ==Response设置响应数据：==

   1. 设置响应行：HTTP/1.1 200 OK
      void setStatus(int sc)：设置响应状态码

   2. 设置响应头：Content-Type: text/html

      void setHeader(String name, String value)：设置响应头键值对

   3. 设置响应体：`<html><head></head><body></body></html>`

      1. 响应字符数据：

         1. 获取字符输出流：
            printWriter getWriter()；

            设置响应头（响应体格式）：
            resp.setHeader("content-type","text/html;charset=utf-8");
            也可以不设置响应头，调用下面这个方法，统一设置流和请求体的编码：
            resp.setContentType("text/html;charset=utf-8");

         2. 写数据：
            writer.write("<h1>欢迎您</h1>");
         3. 细节：该流不需要关闭，响应结束，response对象销毁，服务器自动关闭流

      2. 响应字节数据：

         1. 获取字节输出流：
            ServletOutputStream getOutputStream();

         2. 写数据：
            outputStream().write(字节数据);

         3. 响应字节数据常用于**图片、音频、视频**等文件的响应，可以使用IO流读取本地文件然后用ServletOutputStream响应到浏览器；

            也可以使用工具类：**IOUtils** , 其中方法 **IOUtils.copy(fis,os);** 即可完成操作
            使用方法：添加maven依赖：

            ```xml
            <dependency>
              <groupId>commons-io</groupId>
              <artifactId>commons-io</artifactId>
              <version>2.6</version>
            </dependency>
            ```

3. ==Response重定向：==

   1. 解释：重定向(Redirect)：一种资源跳转的方式（和请求转发有所区别）
      当一个资源(Servlet)获取到一个请求，发现处理不了的时候，会响应给浏览器，告知该请求无法处理，并且提供一个可以处理该请求的另一个资源(Servlet)的路径，浏览器则会根据该路径访问这个资源，完成重定向。

   2. **实现方式：**

      resp.setStatus(302)；设置响应状态码302

      resp.setHeader("location","资源B的路径")；设置响应头location

      **简化后更常用的方法（推荐使用）：**

      resp.sendRedirect("资源B的路径");  //底层仍是上面的两个方法

      **注意：资源B的路径需要加上项目的虚拟目录！！！如：/maven-tomcat1/demo3**
      **（如果在idea中或者在pom文件的tomcat插件里，配置了虚拟目录为 "/" ,则相当于不用加）**

   3. **重定向特点（与请求转发不同）：**

      浏览器地址栏发生变化；

      可以重定向到任意位置的资源（服务器内部、外部均可）；

      两次请求，不能在多个资源使用request共享数据

4. ==资源路径的相关问题：==

   1. 到底什么时候需要加项目的虚拟目录？
      明确路径谁使用：
      浏览器使用：需要加虚拟目录（项目访问路径）
      服务端使用：不需要加虚拟目录

   2. 例子：
      `<a href = '路径'>`     加虚拟目录
      `<form action = '路径'>`  加虚拟目录
      req.getRequestDispatcher("路径")  不加虚拟目录
      resp.sendRedirect("路径")  加虚拟目录

   3. **动态加上虚拟目录：**

      用request对象将虚拟目录拿到，然后与资源路径进行拼接

      例子：
      String contextPath = request.getContextPath();
      resp.sendRedirect( contextPath + "资源路径" )

### 四、JSP技术

#### 1. 概述与原理

1. 概念：Java Server Pages, Java服务端页面

   一种动态的网页技术，其中既可以定义HTML、JS、CSS等静态内容，还可以定义Java代码的动态内容，可以说，**JSP = HTML + Java**

   例子：

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <h1>hello world!</h1>
   <%
       System.out.println("hello world!");
   %>
   </body>
   </html>
   ```

2. JSP的作用：简化开发，避免了在Servlet中直接输出HTML标签

3. JSP Maven依赖坐标：

   ```xml
   <dependency>
     <groupId>javax.servlet.jsp</groupId>
     <artifactId>jsp-api</artifactId>
     <version>2.2</version>
     <scope>provided</scope>
   </dependency>
   ```

4. JSP原理：

   JSP本质上就是一个Servlet，浏览器请求Jsp文件时，tomcat会将该Jsp文件转换成Servlet的java文件，再通过编译生成class字节码文件，通过该字节码文件进行服务。

#### 2. JSP脚本

1. JSP脚本用于在JSP页面内定义 Java代码
2. JSP脚本分类：
   1. <%......%> ：内容会直接放到 jspService()方法中
   2. <%=......%> ：内容会放到out.print()中，作为out.print()的参数，打印到浏览器页面
   3. <%!......%> ：内容会放到 jspService()方法之外，被类直接包含（相当于成员的位置）

#### 3. JSP缺点

1. 书写麻烦，特别是一些复杂的页面

2. 阅读麻烦

3. 复杂度高，运行需要依赖于各种环境，JRE，JSP容器，JavaEE...

4. 占内存和磁盘，JSP会自动生成 .java和 .class文件占磁盘，运行的是 .class文件占内存

5. 调试困难，出错后，需要找到自动生成的 .java文件进行调试

6. 不利于团队协作，前端人员不会Java，后端人员不精HTML

7. ...

   **JSP已逐渐退出历史舞台...**

#### 4. 技术发展史

1. 开始只使用Servlet，html页面写在Servlet里，很不方便
2. 随后变为只使用JSP，在JSP中直接写java代码，很难阅读
3. 随后再变为 JSP + Servlet 一起使用，先在Servlet处理数据后放入request的Attribute中，转发到 JSP 进行展示，**不直接在JSP中写Java代码**（使用**EL表达式、JSTL标签**）
4. **之后 Servlet+html+ajax 成为 JavaWeb技术的主流**

#### 5. EL表达式

1. Expression Language 表达式语言，用于**简化JSP页面的Java代码**

2. 主要功能：==获取数据==

3. 语法：${expression}
   ==${brands}：==**获取域中key为brands的数据**

   ==${brand.name}：==**自动调用brand的getName()方法，获取name值**

4. JavaWeb中的四大域对象：

   1. page：当前页面有效
   2. request：当前请求有效（常用）
   3. session：当前会话有效（常用）
   4. application：当前应用有效

   **EL表达式获取数据，会依次从这4个域中寻找，直到找到为止**

#### 6. JSTL标签

1. Jsp Standarded Tag Library：JSP标准标签库。使用标签**取代JSP页面上的Java代码**

2. 导入坐标和引入JSTL标签库

   导入maven依赖坐标：

   ```xml
   <dependency>
     <groupId>jstl</groupId>
     <artifactId>jstl</artifactId>
     <version>1.2</version>
   </dependency>
   <dependency>
     <groupId>taglibs</groupId>
     <artifactId>standard</artifactId>
     <version>1.1.2</version>
   </dependency>
   ```

   在JSP页面上引入JSTL标签库：

   ```jsp
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
   ```

3. 两个重要的标签

   **<c: if>标签：**相当于if

   1. 语法：

      <c: if test="逻辑表达式"> 

      ​       具体操作

      </c: if>

   2. 例子：

      ```jsp
      <c:if test="${status ==1}">
          启用
      </c:if>
      
      <c:if test="${status ==0}">
          禁用
      </c:if>
      ```

   **<c: forEach>标签：**相当于for循环

   1. 语法：

      **第一种用法（增强for循环）：**

      <c: forEach items="被遍历的容器users" var="用于遍历的临时变量user">

      ​		具体操作

      </c: forEach>

      相当于：

      for(User user ：users){
      		具体操作
      }

      还可以加入一个varStatus属性，varStatus="属性名称"，该属性可以产生一个序号，在循环中创造表格时可以用这个属性添加序号，

      格式：${serial.count} ：count  （count从1开始，index从0开始）

      例子：
      <c:forEach items="${actors}" var="actor" varStatus="serial">

      ​		<td>${serial.count}</td>

      </c: forEach>

      **第二种用法（普通for循环）：**

      <c:forEach begin="0" end="10" step="1" var="i">
        	${i}
      </c: forEach>
      begin：开始数    end：结束数    step：步长    var：循环变量

      相当于：

      for(int i = 0; i <= 10; i++){

      ​	System.out.println(i);

      }

   2. 例子：
      第一种用法：打印表格

      ```jsp
      <c:forEach items="${actors}" var="actor" varStatus="serial">
          <tr align="center">
              <td>${serial.count}</td>
              <td>${actor.name}</td>
              <td>${actor.sex}</td>
              <td>${actor.breath}</td>
              <td>${actor.level}</td>
              <c:if test="${actor.survive == 1}">
                  <td>存活</td>
              </c:if>
              <c:if test="${actor.survive == 0}">
                  <td>牺牲</td>
              </c:if>
          </tr>
      </c:forEach>
      ```

      第二种用法：分页工具条

      ```jsp
      <c:forEach begin="1" end="10" step="1" var="i">
          <a href="#">${i}</a>
      </c:forEach>
      ```


### 五、MVC模式与三层架构

#### 1. MVC模式

1. 概述：

   MVC是一种分层开发的模式，其中：
   	M：Model，业务模式，处理业务
   	 V：View，视图，界面展示
   	 C：Controller，控制器，处理请求，调用模型和视图

2. MVC好处：

   职责单一，互不影响

   有利于分工协作

   有利于组件重用

   ![image-20220330210859263](..\image\image-20220330210859263.png)

#### 2. 三层架构

1. **概述：**

   数据访问层（dao/mapper）：对数据库的==CRUD基本操作==

   业务逻辑层（service）：对==业务逻辑==进行封装，**组合数据访问层中基本功能**，形成复杂的业务逻辑功能

   表现层（controller）：接收请求，封装数据，==调用业务逻辑层==，响应数据

2. **三大框架：SSM**

   分别对应三层架构：

   ![image-20220330211501994](..\image\image-20220330211501994.png)



### 六、会话跟踪技术

#### 1. 概述

1. 会话：用户打开浏览器，访问web服务器的资源，会话建立，直到有一方断开连接，会话结束。在一次会话中可以包含==多次==请求和响应
2. 会话跟踪：一种维护浏览器状态的方法，服务器需要识别多次请求是否来自于同一浏览器，以便在同一次会话的多次请求间==共享数据==
3. HTTP协议是==无状态==的，每次浏览器向服务器发送请求时，服务器都会将该请求视为新的请求，因此我们需要==会话跟踪技术==来实现会话内数据共享
4. 实现方式：
   1. 客户端会话跟踪技术：Cookie
   2. 服务端会话跟踪技术：Session

#### 2. Cookie会话

1. **介绍**
   Cookie：客户端会话技术，将数据保存到客户端，以后每次请求都携带Cookie数据进行访问

2. **基本使用**
   ==发送Cookie：==

   1. 创建Cookie对象，设置数据：
      Cookie cookie = new Cookie("key","value");
   2. 发送Cookie到客户端：使用response对象：
      response.addCookie(cookie);

   ==获取Cookie：==

   1. 获取客户端携带的所有Cookie，使用request对象：
      Cookie[] cookies = request.getCookies();
   2. 遍历数组，获取每一个Cookie对象：for
   3. 使用Cookie对象方法获取数据：
      cookie.getName();
      cookie.getValue();

3. **原理**

   Cookie的实现是基于HTTP协议的，底层实际上是：

   服务端设置cookie，相当于设置了一个cookie响应头响应回浏览器，响应头的值就是cookie保存的数据（键值对)；

   服务端获取cookie，相当于客户端设置了cookie请求头向服务端发送请求，请求头的值就是保存在浏览器的cookie数据（键值对）

   1. 响应头：set-cookie
   2. 请求头：cookie

   ![image-20220331155621854](..\image\image-20220331155621854.png)

4. **细节**
   ==Cookie存活时间：==

   1. 默认情况下，Cookie存储在浏览器内存中，当浏览器关闭，内存释放，则Cookie销毁
   2. setMaxAge(int seconds)：cookie对象方法，设置Cookie存活时间（单位：秒）
      1. 正数：将Cookie写入浏览器所在电脑的硬盘，持久化存储。到时间自动删除
      2. 负数：默认值，Cookie在当前浏览器内存中，当浏览器关闭，则Cookie被销毁
      3. 零：删除对应Cookie

   ==Cookie存储中文：==

   1. Cookie 不能直接存储中文
   2. 如果需要存储，则需要进行转码：URL编码（使用工具类：URLEncoder、URLDecoder）
   3. tomcat9 已经支持直接存储中文

#### 3. Session会话

1. **介绍**
   Session：服务端会话跟踪技术：将数据保存到服务端中
   JavaEE提供 HttpSession接口，来实现一次会话的多次请求间数据共享功能

2. **基本使用**

   1. 获取Session对象：
      HttpSession session = request.getSession();

   2. Session对象功能：

      void setAttribute(String name, Object o)：存储数据到session域中

      Object getAttribute(String name)：根据key，获取值

      void removeAttribute(String name)：根据key，删除该键值对

3. **原理**

   ==Session是基于Cookie实现的==：

   如果服务端创建了Session对象，那么在响应浏览器时，会设置一个cookie响应头：set-cookie，并且将它的值设置为该会话Session对象的id

   当下一个请求发送时，会设置一个cookie请求头：cookie，值为Session对象的id。

   所以不同的浏览器访问服务端，各自在服务端的Session对象id是不一样的。

   ==注意：==同一个浏览器重启后，访问的Session对象是新的，和重启前的Session对象id不同

   ![image-20220331171709590](..\image\image-20220331171709590.png)

4. **细节**

   1. ==Session的钝化、活化：==

      服务器重启后，Session中的数据是否还在？==> 在
      钝化：在服务器正常关闭后，Tomcat会自动将Session数据写入硬盘的文件中
      活化：再次启动服务器后，Tomcat会自动从文件中加载数据到Session中

   2. ==Session的销毁：==

      1. 默认情况下，无操作，30分钟自动销毁，可以在web.xml中配置对应时间（单位：分钟）：

         ```xml
         <session-config>
             <session-timeout>100</session-timeout>
         </session-config>
         ```

      2. 调用Session对象的 invalidate() 方法，自己销毁掉

#### 4. 小结

1. Cookie 和 Session 都是来完成一次会话内多次请求间==共享数据==的
2. 区别：
   1. 存储位置：Cookie将数据存储在客户端，Session将数据存储在服务端
   2. 安全性：Cookie不安全（请求可能被截断），Session安全（攻破服务器较难）
   3. 数据大小：Cookie最大3KB，Session无大小限制
   4. 存储时间：Cookie可以长期存储，Session默认30分钟
   5. 服务器性能：Cookie不占用服务器资源，Session占用服务器资源

### 七、Filter

#### 1. 概述

1. Filter表示==过滤器==，是JavaWeb三大组件（Servlet、Filter、Listener）之一
2. 过滤器可以把对资源的请求拦截下来，从而实现一些特殊的功能
3. 过滤器一般完成一些通用的操作，比如：权限控制、统一编码处理、敏感字符处理等等...

#### 2. 实现步骤

1. 定义类，==实现Filter接口==，重写其所有方法：

   init()：初始化、destroy()：销毁、doFilter()：放行

   ![image-20220331222009590](..\image\image-20220331222009590.png)

2. 配置Filter拦截的资源的路径：在类上定义@WebFilter注解：
   ![image-20220331222225269](..\image\image-20220331222225269.png)

3. 在doFilter方法中：

   1. 写放行前的业务逻辑，主要操作request对象，例如：权限判定（是否登录）、请求数据统一编码等
   2. 调用==chain.doFilter()==方法放行，真正访问目的资源
   3. 写放行后的业务逻辑，主要操作response对象，目的资源访问完后，仍会回到过滤器中

   ![image-20220331222953427](..\image\image-20220331222953427.png)

#### 3. 执行过程

1. 过滤器放行后访问对应资源，资源访问完成后，会回到Filter中
2. 回到Filter中，不重头执行，而是执行放行之后的逻辑（代码）

![image-20220331223339627](..\image\image-20220331223339627.png)

#### 4. 拦截路径配置

1. 拦截具体的资源：/index.jsp：只有访问index.jsp时才会被拦截
2. 目录拦截：/user/*：访问/user下的所有资源，都会被拦截
3. 后缀名拦截：*.jsp：访问后缀名为jsp的资源，都会被拦截
4. 拦截所有：/*：访问所有资源，都会被拦截

#### 5. 过滤器链

1. 一个Web应用，可以配置多个过滤器来拦截同一个资源的访问，这些过滤器称为过滤器链
   ![image-20220331224522997](..\image\image-20220331224522997.png)
2. 注解配置的Filter，优先级（执行顺序）按照过滤器类名（字符串）的自然排序

### 八、Listener

#### 1. 概述

1. Listener表示监听器，是JavaWeb三大组件（Servlet、Filter、Listener）之一
2. 监听器可以监听的意思是，在application（代表这个web项目），session，request三个对象创建、销毁或者往其中添加、修改、删除属性时==自动==执行代码的功能组件。其中，对application对象的监听，相当于对ServletContext对象的监听

#### 2. 分类

JavaWeb中提供了8个监听器：

| 监听器分类         | 监听器名称（接口）              | 作用                                           |
| ------------------ | ------------------------------- | ---------------------------------------------- |
| ServletContext监听 | ServletContextListener          | 用于对ServletContext对象进行监听（创建、销毁） |
|                    | ServletContextAttributeListener | 对ServletContext对象中属性的监听（增删改属性） |
| Session监听        | HttpSessionListener             | 对Session对象的整体状态的监听（创建、销毁）    |
|                    | HttpSessionAttributeListener    | 对Session对象中的属性监听（增删改属性)         |
|                    | HttpSessionBindingListener      | 监听对象于Session的绑定和解除                  |
|                    | HttpSessionActivationListener   | 对Session数据的钝化和活化的监听                |
| Request监听        | ServletRequestListener          | 对Request对象进行监听(创建、销毁)              |
|                    | ServletRequestAttributeListener | 对Request对象中属性的监听〔增删改属性)         |

#### 3. 实现步骤

以ServletContextListener的使用为例：

1. 定义类，实现ServletContextListener接口，重写方法：

   void contextInitialized()方法：当web项目创建后，这个方法中的代码就会执行；

   void contextDestroyed()方法：当web项目卸载后，这个方法中的代码就会执行。

2. 在类上添加==@WebListener==注解（不用配置参数，自动的）

3. 例子：

   ![image-20220331234231608](..\image\image-20220331234231608.png)



### 九、AJAX

#### 1. 概述

1. 概念：AJAX(**A**synchronous **J**avaScript **A**nd **X**ML)：==异步==的 JavaScript 和 XML

2. **AJAX作用：**

   1. 与服务器进行数据交换：通过AJAX可以给服务器发送请求，并**获取服务器响应的数据**，这样：

      使用了AJAX和服务器进行通信，就可以使用**HTML+AJAX**来**替换 JSP 页面**了，从而做到前后端人员开发**分工协作**（前后端分离）

      图解：
      ==以前：==

      ![image-20220402204100092](..\image\image-20220402204100092.png)

      ==现在：==

      ![image-20220402204231725](..\image\image-20220402204231725.png)

   2. 异步交互：可以在**不重新加载整个页面**的情况下，与服务器交换数据并**更新部分网页**的技术，如：搜索联想、用户名是否可用校验，等等...

      异步同步解释：
      图解：

      ![image-20220402205041664](..\image\image-20220402205041664.png)

#### 2. 原生AJAX基本使用

1. **Ajax 的核心是 XMLHttpRequest 对象(XHR)。**所有现代浏览器都支持 XMLHttpRequest 对象。

   XMLHttpRequest 对象用于**同幕后服务器交换数据**。这意味着可以更新网页的部分，而不需要重新加载整个页面。

   **操纵XMLHttpRequest 对象在JS脚本中进行**

2. **基本使用步骤：**

   1. ==创建XMLHttpRequest对象：==
      方法：**variable = new XMLHttpRequest();**

      例子：

      ```javascript
      var xhttp; 
      if (window.XMLHttpRequest) {
          xhttp = new XMLHttpRequest(); //创建XMLHttpRequest对象
          } else {
          // code for IE6, IE5
           xhttp = new ActiveXObject("Microsoft.XMLHTTP");//老版本浏览器创建方法
      }
      ```

   2. ==向服务器发送请求：==
      方法：

      1. **xhttp.open("GET", "ajax_info.txt", true); ** 规定请求的类型
         (参数1：请求类型 ； 参数2：资源路径 ； 参数3：true(异步)、false(同步)  默认是true
         注意：==参数2要填写全路径==，因为以后的**前后端是分开的两个项目**，部署在不同的服务器上，不能再使用相对路径，例如：http:/ /localhost:8080/ajax-demo/ajaxServlet)
      2. **xhttp.send();** 向服务器发送请求（用于 GET）
      3. **xhttp.send(string);**  向服务器发送请求（用于 POST）

      例子：

      ```javascript
      //Get
      xhttp.open("GET", "demo_get2.asp?fname=Bill&lname=Gates", true);
      xhttp.send();
      //Post
      xhttp.open("POST", "ajax_test.asp", true);
      xhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
      xhttp.send("fname=Bill&lname=Gates");
      ```

   3. ==获取响应：==
      属性：

      1. **readyState 属性：**存留 XMLHttpRequest 的状态：

         0: 请求未初始化

         1: 服务器连接已建立

         2: 请求已接收

         3: 正在处理请求

         4: 请求已完成且响应已就绪

      2. **onreadystatechange 属性：** 用来定义当 readyState 发生变化时执行的函数。

      3. **status 属性：**存有 XMLHttpRequest 对象的状态码：

         200: "OK"

         403: "Forbidden"

         404: "Page not found"

      4. **statusText 属性：**返回状态文本（例如 "OK" 或 "Not Found"）

      5. **responseText 属性：**获取字符串形式的响应数据

      6. **responseXML 属性：**获取 XML 数据形式的响应数据

      例子：

      ```javascript
      function loadDoc() {
          var xhttp = new XMLHttpRequest();
          xhttp.onreadystatechange = function(){//定义当 readyState 发生变化时执行的函数
              if (this.readyState == 4 && this.status == 200) {
                  document.getElementById("demo").innerHTML =
                  this.responseText; //修改id是"demo"的html元素的内容，为响应数据
             }
          };
          xhttp.open("GET", "ajax_info.txt", true); //规定Get请求
          xhttp.send(); //发送请求
      } 
      ```

   4. 查阅文档，了解更多原生AJAX使用：
      https://www.w3school.com.cn/js/js_ajax_http.asp

#### 3. Axios异步框架

1. **Axios 对原生AJAX进行封装，**简化书写。官网：https://www.axios-http.cn

2. **基本使用方式：**

   1. ==引入 axios 的 js 文件：==

      ```html
      <script src="js/axios-0.18.0.js"></script>
      ```

   2. ==使用 axios发送请求，并获取响应结果：==

      语法和例子：

      ```javascript
      //Get请求
      axios({ //使用axios，写完method、url后，请求就自动发送（第一对括号的内容）
          method:"get",
          url:"http://localhost:8080/ajax-demo1/aJAXDemo1?username=zhangsan"
          }).then(function (resp){//.then是一个回调函数，响应收到的时候调用
          	alert(resp.data);//resp.data就是响应内容
          });
      ```

      ```javascript
      //Post请求
      axios({ //Post请求体内容写到data中
          method:""post"",
          url:"http://localhost:8080/ajax-demo1/aJAXDemo1",
          data:"username=zhangsan”
      }).then(function(resp){
          alert(resp.data);
      });
      ```

3. **Axios 请求方式别名使用方式**

   1. 概述：

      为了方便起见，Axios已经为所有支持的请求方式提供了别名：

      1. axios.request(config)
      2. axios.get(url[, config])
      3. axios.delete(url[, config])
      4. axios.head(url[, config])
      5. axios.options(url[, config])
      6. axios.post(url[, data[, config]])
      7. axios.put(url[, data[, config]])
      8. axios.patch(url[, data[, config]])

      **在使用别名方法时， url、method、data 这些属性都不必在配置中指定，**例如：

      | 方法名             | 作用             |
      | ------------------ | ---------------- |
      | get(url)           | 发起GET方式请求  |
      | post(url,请求参数) | 发起POST方式请求 |

   2. ==使用方法示例：==

      ```javascript
      //Get请求
      axios.get("url")
        .then(function(resp)){
           alert(resp,data);   
      });
      ```

      ```javascript
      //Post请求
      axios.post("url","username=zhangsan")
        .then(function(resp)){
           alert(resp,data);   
      });
      ```

### 十、JSON

#### 1. 概述

1. 概念：JSON：**J**avaScript **O**bject **N**otation。 ==JavaScript 对象表示法==
2. 优势：由于其语法简单，层次结构鲜明，现多用于作为**数据载体**，在网络中进行数据传输
   图解：
   ![image-20220402225259545](..\image\image-20220402225259545.png)

#### 2. JSON 基础语法

1. ==定义：==

   ```javascript
   var 变量名 = {"key1":value1,
              	 "key2":value2,
       		 ...	
   			};
   ```

   value的数据类型有：

   1. 数字（整数或浮点数）
   2. 字符串（在双引号中）
   3. 逻辑值（true 或 false）
   4. 数组（在方括号中）
   5. 对象（在花括号中）
   6. null

   例子：

   ```javascript
   var json = {"name":"zhangsan",
               "age":23,
               "addr":["北京","上海","西安"]
              };
   ```

2. ==获取数据：==

   语法：变量名.key
   例子：

   ```javascript
   json.name
   ```

#### 3. JSON、Java对象的转换 

1. ==图解：==
   ![image-20220402230636728](..\image\image-20220402230636728.png)

2. ==服务端对 JSON数据、Java对象的转换：==

   1. **Fastjson**是**阿里巴巴**提供的一个Java语言编写的高性能功能完善的JSON库，是目前Java语言中最快的JSON库，可以实现**Java对象**和**JSON字符串**的相互转换。

   2. **Fastjson的使用：**

      1. 导入maven依赖坐标：

         ```xml
         <dependency>
             <groupId>com.alibaba</groupId>
             <artifactId>fastjson</artifactId>
             <version>1.2.62</version>
         </dependency>
         ```

      2. Java对象转JSON：

         ```java
         String jsonStr = JSON.toJSONString(obj);
         ```

      3. JSON字符串转Java对象：

         ```java
         User user = JSON.parseObject(jsonStr,User.class);
         ```

3. ==客户端与服务端通过JSON传输数据：==

   **案例一，查询数据：**

   ![image-20220403003836341](..\image\image-20220403003836341.png)

   客户端相关代码：

   ```javascript
   window.onload = function(){
       axios({
           method:"get",
           url:"http://localhost:8080/ajax-demo1/jsonDemo1Servlet"
       }).then(function(resp)){
               //获取数据
               let brands = resp.data;
               let tableData = "<tr>\n"+
               				"<th>序号</th>\n"+
               				"<th>品牌名称</th>\n"+
               				"<th>企业名称</th>\n"+
               				"<th>排序</th>\n"+
               				"<th>品牌介绍</th>\n"+
               				"<th>状态</th>\n"+
               				"<th>操作</th>\n"+
               				"</tr>";
               
               for(let i = 0; i < brands.length; i++){
           		let brand = brands[i];
           		
           		tableData += "\n" + 
                       		 "<tr align=\"center\">\n" +
                       		 "<td>"+(i+1)+"</td>\n" +
                       		 "<td>"+brand.brandName+"</td>\n" +
                       		 "<td>"+brand.companyName+"</td>\n" +
                       		 "<td>"+brand.ordered+"</td>\n" +
                       		 "<td>"+brand.description+"</td>\n" +
                       		 "<td>"+brand.status+"</td>\n" +
                       		 "\n" +
                       "<td><a href=\"#\">修改</a> <a href=\"#\">删除</a></td>\n" +
                       		 "</tr>"
       		}
       
       		//设置表格数据
       		document.getElementById("brandTable").innerHTML = tableData;
               
           });
   }
   ```

   服务端相关代码：

   ```java
   @WebServlet("/jsonDemo1Servlet")
   public class jsonDemo1Servlet extends HttpServlet{
       private BrandSerVice brandService = new BrandService();
       
       @Override
       protected void doGet(HttpRequest req, HttpResponse resp) throws ...{
           //1. 调用Service查询
           List<Brand> brands = brandService.selectAll();
           
           //2. 将集合转换为JSON数据，序列化
           String jsonString = JSON.toJSONString(brands);
           
           //3.响应数据
           resp.setContentType("text/json;charset=utf-8");
           resp.getWriter().write(jsonString);
       }
       
       @Override
       protected void doPost(HttpRequest req, HttpResponse resp) throws ...{
           this.doGet(req,resp);
       }
   }
   ```

   **案例二，新增数据：**

   ![image-20220403003711089](..\image\image-20220403003711089.png)

   客户端相关代码：

   ```javascript
   //1. 给按钮绑定点击事件
   document.getElementById("btn").onclick = function(){
       //将表单数据转为json
       var formData = {
           brandName:"",
           companyName:"",
           ordered:"",
           description:"",
           status:"",
       };
       //获取表单数据
       let brandName = document.getElementById("brandName").value;
       //设置数据
       formData.brandName = brandName;
       
       //获取表单数据
       let companyName = document.getElementById("companyName").value;
       //设置数据
       formData.companyName = companyName;
       
       //...以此类推
       
       let status = document.getElementByName("status");
       for(let i = 0; i < status.length; i++){
           if(status[i].checked){
               formData.status = status[i].value;
           }
       }
       
       //2. 发送ajax请求
       axios({
           method:"post",
           url:"http://localhost:8080/ajax-demo1/addServlet"
           data:formData
        }).then(function (resp){
           // 判断响应数据是否为 success
           if(resp.data == "success"){
               location.href = "http://localhost:8080/brand-demo/brand.html";
           }
       });
   }
   ```

   服务端相关代码：

   ```java
   @WebServlet("/addServlet")
   public class add1Servlet extends HttpServlet{
       private BrandSerVice brandService = new BrandService();
       
       @Override
       protected void doGet(HttpRequest req, HttpResponse resp) throws ...{
           //1. 接收数据,req.getParameter不能接收json的数据
           /*
             req.getParameter brandName = request.getParameter("brandName")；
             System.out.println(brandName);
           */
           
           //1. 获取请求体数据
           BufferedReader br = request.getReader();
           String params = br.readLine();
           
           //2. 将JSON字符串转为Java对象
           Brand brand = JSON.parseObject(params,Brand.class);
           
           //3. 调用service添加
           brandService.add(brand);
           
           //4. 响应成功标识
           response.getWriter().write("success");
       }
       
       @Override
       protected void doPost(HttpRequest req, HttpResponse resp) throws ...{
           this.doGet(req,resp);
       }
   }
   ```

   

















