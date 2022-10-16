# MyBatis学习笔记

### 一、 MyBatis简介

#### 1. 介绍

1. MyBatis是一款优秀的==持久层==**框架**，用于**简化 JDBC 开发**

2. MyBatis本是Apache的一个开源项目iBatis，2010年这个项目由apache software foundation 迁移到了 google code，并且改名为MyBatis。2013年11月迁移到Github

3. 官网：https://mybatis.org/mybatis-3/zh/index.html

4. 官网阐述：

   1. ==MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作！==
   2. MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

5. **持久层：**

   1. 负责将数据保存到**数据库**的那一层代码
   2. JavaEE三层架构：表现层、业务层、**持久层**

6. **框架：**

   1. 框架就是一个**半成品软件**，是一套可重用的、通用的、软件**基础代码模型**

   2. 在框架的基础之上构建软件==编写更加高效、规范、通用、可扩展==

      （通俗来讲，就是别人已经帮忙写好了一部分基础代码，只需要在其写上自己的代码来进行软件开发）

#### 2. **JDBC原生开发的缺点**

1. 硬编码：
   1. 注册驱动，获取连接
   2. SQL语句
2. 操作繁琐：
   1. 手动设置sql参数
   2. 手动封装结果集（虽然使用DBUtils能简化一定程度）

### 二、MyBatis快速使用

#### 1. 环境与配置

1. ==创建Maven工程，导入坐标：==

   ```xml
   <dependencies>
       <!--MyBatis-->
       <dependency>
         <groupId>org.mybatis</groupId>
         <artifactId>mybatis</artifactId>
         <version>3.5.5</version>
       </dependency>
   
       <!--Servlet-->
       <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>javax.servlet-api</artifactId>
         <version>3.1.0</version>
         <scope>provided</scope>
       </dependency>
   
       <!--mysql驱动-->
       <dependency>
         <groupId>mysql</groupId>
         <artifactId>mysql-connector-java</artifactId>
         <version>5.1.37</version>
       </dependency>
   
       <!--Junit-->
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.12</version>
         <scope>test</scope>
       </dependency>
   
       <!--添加slf4j日志api-->
       <dependency>
         <groupId>org.slf4j</groupId>
         <artifactId>slf4j-api</artifactId>
         <version>1.7.20</version>
       </dependency>
       <!--logback-classic依赖-->
       <dependency>
         <groupId>ch.qos.logback</groupId>
         <artifactId>logback-classic</artifactId>
         <version>1.2.3</version>
       </dependency>
       <!--logback-core依赖-->
       <dependency>
         <groupId>ch.qos.logback</groupId>
         <artifactId>logback-core</artifactId>
         <version>1.2.3</version>
       </dependency>
     </dependencies>
   ```

2. ==在main/resources目录下，创建三个xml文件：==

![image-20220404233908757](..\image\image-20220404233908757.png)

3. ==编写MyBatis核心配置文件 mybatis-config.xml（mysql连接信息）：==

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
       
       <environments default="development">
           <environment id="development">
               <transactionManager type="JDBC"/>
               <dataSource type="POOLED">
                   <!--数据库连接信息-->
                   <property name="driver" value="com.mysql.jdbc.Driver"/>
                   <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
                   <property name="username" value="root"/>
                   <property name="password" value="Socialbigfeng321"/>
               </dataSource>
           </environment>
       </environments>
       
       <mappers>
           <!--加载sql映射文件-->
           <mapper resource="UserMapper.xml"/>
       </mappers>
   
   </configuration>
   ```

4. ==编写SQL映射文件 UserMapper.xml（统一管理sql语句）：==

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   
   <!--
       namespace：名称空间
   -->
   <mapper namespace="test">
   
       <select id="selectAll" resultType="com.feng.pojo.User">
           select * from tb_user
       </select>
   
   </mapper>
   ```

5. ==编写logback配置文件 logback.xml （日志工具）：==

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
       <!--
           CONSOLE ：表示当前的日志信息是可以输出到控制台的。
       -->
       <appender name="Console" class="ch.qos.logback.core.ConsoleAppender">
           <encoder>
               <pattern>【%level】 %blue(%d{HH:mm:ss.SSS}) %cyan(【%thread】) %boldGreen(%logger{15}) - %msg %n</pattern>
           </encoder>
       </appender>
   
       <logger name="com.feng" level="DEBUG" additivity="false">
           <appender-ref ref="Console"/>
       </logger>
   
       <root level="DEBUG">
           <appender-ref ref="Console"/>
       </root>
   </configuration>
   ```

 6. ==解决SQL映射文件的警告提示：==

    1. 产生原因：idea和数据库没有建立连接，不识别表信息

    2. 解决方式：在idea中配置MySQL数据库连接
       图解：

       ![image-20220405000409253](..\image\image-20220405000409253.png)

       ![image-20220405001847754](..\image\image-20220405001847754.png)

#### 2. 编写Java类

1. 在pojo包下，编写pojo类User：

   ```java
   public class User {
   
       private Integer id;
       private String username;
       private String password;
       private String gender;
       private String addr;
       
       //getter()...
       //setter()...
       //toString()...
   }
   ```

2. 编写类MyBatisDemo1，查询数据库User表：
   流程描述：

   1. 加载mybatis的核心配置文件，获取SqlSessionFactory对象
   2. 用SqlSessionFactory对象，获取SqlSession对象
   3. 用SqlSession对象，执行sql语句
   4. 释放资源

   代码：

   ```java
   /**
    * MyBatis快速入门
    */
   public class MyBatisDemo1 {
       public static void main(String[] args) throws IOException {
   
           //1.加载mybatis的核心配置文件，获取SqlSessionFactory
           String resource = "mybatis-config.xml";
           InputStream inputStream = Resources.getResourceAsStream(resource);
           SqlSessionFactory sqlSessionFactory = 
               new SqlSessionFactoryBuilder().build(inputStream);
   
           //2.获取SqlSession对象，用来执行sql
           SqlSession sqlSession = sqlSessionFactory.openSession();
   
           //3.执行sql，传入sql语句在UserMapper配置文件中的id（这是唯一的）
           List<User> users = sqlSession.selectList("test.selectAll");
   
           System.out.println(users);
   
           //4.释放资源
           sqlSession.close();
       }
   }
   ```

### 三、Mapper代理开发

#### 1. 简介

1. Mapper代理开发，是**创建和SQL映射文件同名对应的接口**，这样在执行SQL映射文件中的sql语句时，通过**使用接口代理的方式**执行，解决了==硬编码==的问题，也简化了后期执行sql

   ==在以后的开发中，一个mapper接口就对应一个表的操作==

2. 目的：

   1. 解决原生方式中的硬编码
   2. 简化后期执行SQL

3. 例如，原生的方式：
   ![image-20220405150009468](..\image\image-20220405150009468.png)

   执行的sql语句**写死在字符串**中，不灵活，并且编写时**idea没有提示**，需要反复查看SQL映射文件，很麻烦，这就是==硬编码==的问题。

   而**Mapper代理开发**的方式：
   ![image-20220405150258690](..\image\image-20220405150258690.png)

   能够调用接口中与SQL映射文件==壹壹对应==的方法，更灵活，而且**idea能提示编写**，效率高

4. 官网的阐述：

   Mapper代理的方法有很多优势，首先它不依赖于字符串字面值，会更安全一点；其次，如果你的 IDE 有代码补全功能，那么代码补全可以帮你快速选择到映射好的 SQL 语句。（和上面的例子说的一样）

#### 2. 实现流程

1. **Step1：** 定义与SQL映射文件**同名**的Mapper接口，并且将Mapper接口和SQL映射文件==放置在同一目录下==

   1. 放置在同一目录的方法：

      1. main/resources目录下的xml文件，在编译之后会自动放到**classes目录**下
      2. 所以，在resources目录下创建和==Mapper接口所在包==一样的**层级目录**，并放入对应SQL映射文件即可

   2. 需要注意的是：

      ​	**在resources目录下创建层级目录，不能像定义包一样用 "." 分隔，而是应该用 "/" 分隔**

      ​	例如：
      ​	![image-20220405154029763](..\image\image-20220405154029763.png)
      ​	![image-20220405154405636](..\image\image-20220405154405636.png)

2. **Step2：**设置SQL映射文件的**namespace属性**为Mapper接口==全限定名==（相当于全类名）

   例子：

   ```xml
   <mapper namespace="com.feng.mapper.UserMapper">
       <select id="selectAll" resultType="com.feng.pojo.User">
           select * from tb_user
       </select>
   </mapper>
   ```

3. **Step3：**在**Mapper接口**中定义方法

   1. 方法名就是SQL映射文件中sql语句的id

   2. 并保持参数类型和返回值类型一致

   3. 例子：

      ```xml
      <mapper namespace="com.feng.mapper.UserMapper">
          <select id="selectAll" resultType="com.feng.pojo.User">
              select * from tb_user;
          </select>
      </mapper>
      ```

      ```java
      public interface UserMapper {
      
          List<User> selectAll(); //List集合中的类型，与SQL返回的类型一致
      
      }
      ```

4. **Step4：**编写Java类：

   1. 通过SqlSession 的 getMapper()方法获取 Mapper接口代理对象

   2. 调用对应方法完成sql的执行

   3. 例子：

      ```java
      /**
       * Mapper代理开发快速入门
       */
      public class MyBatisDemo2 {
          public static void main(String[] args) throws IOException {
      
              //1.加载mybatis的核心配置文件，获取SqlSessionFactory
              String resource = "mybatis-config.xml";
              InputStream inputStream = Resources.getResourceAsStream(resource);
              SqlSessionFactory sqlSessionFactory = 
                  new SqlSessionFactoryBuilder().build(inputStream);
      
              //2.获取SqlSession对象，用来执行sql
              SqlSession sqlSession = sqlSessionFactory.openSession();
      
              //原生方法，已舍弃
              //List<User> users = sqlSession.selectList("test.selectAll");
      
              //3.获取Mapper代理对象（底层一定实现了接口方法，这里获取的正是实现类对象的引用）
              UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
      
              //4.执行sql
              List<User> users = userMapper.selectAll();
      
              System.out.println(users);
      
              //4.释放资源
              sqlSession.close();
          }
      }
      ```

5. **细节：**

   **如果Mapper接口名称和SQL映射文件名相同（使用了Mapper代理开发），并在同一目录下，则可以使用包扫描的方式简化SQL映射文件在mybatis核心配置文件中的加载**

   例子：

   ```xml
   <configuration>
       .....其他内容省略
       <mappers>
           <!--旧的加载sql映射文件方式-->
           <!--每个配置文件都要写一行，非常麻烦-->
           <!--<mapper resource="com\feng\mapper\UserMapper.xml"/>-->
           
           <!--包扫描的方式加载sql映射文件-->
           <!--使用该方法，这个目录下的所有映射文件都会被加载，而只需要一行代码，非常方便-->
           <package name = "com.feng.mapper"/>
       </mappers>
       
   </configuration>
   ```

#### 3. 图解

| MyBatis初始化                                                |
| ------------------------------------------------------------ |
| ![image-20220424200254871](..\image\image-20220424200254871.png) |

#### 4. Mapper.xml

> 注意：mapper.xml中的sql语句末尾不要加分号，可能会报莫名其妙的异常

1. **mapper根标签**

   > Mapper文件相当于DAO接口的“实现类”，namespace属性指定实现DAO接口的全限定名

2. **insert标签**

   > 声明添加操作
   >
   > 常用属性
   >
   > - id属性：绑定对应DAO接口中的方法
   >
   > - parameterType属性：用于指定接口中对应方法的参数类型，**该参数可省略**
   >
   > - useGeneratedKeys属性：设置添加操作是否需要回填生成的主键
   >
   > - keyProperty属性：指定回填的id设置到参数对象中的哪个属性
   >
   > - timeout属性：设置此操作的超时时间，如果不设置则一直等待

3. **delete标签**

   > 声明删除操作

4. **update标签**

   > 声明修改操作

5. **select标签**

   > 声明查询操作
   >
   > 常用属性
   >
   > - id属性：绑定对应DAO接口中的方法
   > - parameterType属性：用于指定接口中对应方法的参数类型，**该参数可省略**
   > - resultType属性：指定当前sql返回数据封装的对象类型（实体类）
   > - resultMap属性：指定从数据表到实体类的字段和属性的对应关系，值为resultMap标签id
   > - useCache属性：指定此查询操作是否需要缓存
   > - timeout属性：设置此操作的超时时间，如果不设置则一直等待

6. **resultMap标签**

   > 用于定义 数据库键 与 Java属性 的映射关系（当两者命名不一致时使用）
   >
   > - id：唯一标识
   > - type：映射的类型，支持别名（不区分大小写）
   >
   > ```xml
   > <resultMap id="brandResultMap" type="brand">
   >     <!--
   >         <id>标签：完成主键映射
   >         <result>标签：完成普通键映射
   > 
   >         column属性：表的列名
   >         property属性：Java属性
   >     -->
   >     <result column="brand_name" property="brandName"/>
   >     <result column="company_name" property="companyName"/>
   > </resultMap>
   > ```

7. **cache标签**

   > 设置当前DAO进行数据库操作时的缓存属性设置
   >
   > ```xml
   > <cache type="" size="" readOnly="false"/>
   > ```

8. **sql和include标签**

   > sql片段的定义 和 sql片段的引用
   >
   > ```xml
   > <sql id="ibc">id , brand_name, company_name</sql>
   > <select id="selectIdAndNames" resultMap="brandResultMap">
   >     select <include refid="ibc"/> from tb_brand
   > </select>
   > ```

### 四、MyBatis核心配置文件

#### 1. 简介

1. MyBatis 的配置文件**包含**了会**深深影响** ==MyBatis 行为的设置和属性信息==。 
2. 配置文档的顶层结构如下：
   - configuration（配置）
     - [properties（属性）](https://mybatis.org/mybatis-3/zh/configuration.html#properties)
     - [settings（设置）](https://mybatis.org/mybatis-3/zh/configuration.html#settings)
     - [typeAliases（类型别名）](https://mybatis.org/mybatis-3/zh/configuration.html#typeAliases)
     - [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)
     - [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)
     - [plugins（插件）](https://mybatis.org/mybatis-3/zh/configuration.html#plugins)
     - environments（环境配置）
       - environment（环境变量）
         - transactionManager（事务管理器）
         - dataSource（数据源）
     - [databaseIdProvider（数据库厂商标识）](https://mybatis.org/mybatis-3/zh/configuration.html#databaseIdProvider)
     - [mappers（映射器）](https://mybatis.org/mybatis-3/zh/configuration.html#mappers)

#### 2. 常用配置的解释

1. 环境配置（environments标签）：

   1. MyBatis 可以配置成==适应多种环境==，这种机制有助于**将 SQL 映射应用于多种数据库之中**， 现实情况下有多种理由需要这么做。例如，开发、测试和生产环境需要有不同的配置

   2. **尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境**

   3. 例如，可以配置多个数据库：

      ```xml
      <configuration>
          <environments default="development">
              <!--
      			配置多个数据库环境，通过default属性进行切换
      		-->
              <environment id="development"> <!--用于开发-->
                  <transactionManager type="JDBC"/> <!--事务管理方式，以后由Spring接管-->
                  <dataSource type="POOLED"> <!--数据库连接池，以后由Spring接管-->
                      <!--数据库连接信息，以后由Spring接管-->
                      <property name="driver" value="com.mysql.jdbc.Driver"/>
                      <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
                      <property name="username" value="root"/>
                      <property name="password" value="Socialbigfeng321"/>
                  </dataSource>
              </environment>
              
              <environment id="test">  <!--用于测试-->
                  <transactionManager type="JDBC"/>
                  <dataSource type="POOLED">
                      <!--数据库连接信息-->
                      <property name="driver" value="com.mysql.jdbc.Driver"/>
                      <property name="url" value="测试数据库的url"/>
                      <property name="username" value="root"/>
                      <property name="password" value="密码"/>
                  </dataSource>
              </environment>
              
          </environments>
      </configuration>
      ```

2. 类型别名（typeAliases标签）

   1. 配置==typeAliases标签==，在标签中==定义pojo包的package标签==，这样在SQL映射文件中，定义sql语句时，就不用在==resultType属性==下写完整的全类名了，只需要写**类名即可**（而且**不区分大小写**）

   2. 例子：

      ```xml
      <configuration>
          
          <!--类型别名，该包下的类起别名，不区分大小写-->
          <!--在SQL映射文件中，resultType属性可以不写该包下的全类名，只写类名即可-->
          <typeAliases>
              <package name="com.feng.pojo"/>
          </typeAliases>
          
      </configuration>
      ```

      ```xml
      <mapper namespace="com.feng.mapper.UserMapper">
          <!--
          <select id="selectAll" resultType="com.feng.pojo.User">
              select * from tb_user;
          </select>
          -->
      
          <!--com.feng.pojo包下的类在核心配置文件中起了别名-->
          <!--resultType可以直接写类名，不用写全类名，而且不区分大小写-->
          <select id="selectAll" resultType="user">
              select * from tb_user
          </select>
      </mapper>
      ```

   3. 也可以像这样一个一个地去配：

      ```xml
      <typeAliases>
        <typeAlias alias="Author" type="domain.blog.Author"/>
        <typeAlias alias="Blog" type="domain.blog.Blog"/>
        <typeAlias alias="Comment" type="domain.blog.Comment"/>
        <typeAlias alias="Post" type="domain.blog.Post"/>
        <typeAlias alias="Section" type="domain.blog.Section"/>
        <typeAlias alias="Tag" type="domain.blog.Tag"/>
      </typeAliases>
      ```

3. 属性（properties标签）

   1. 用于设置键值对，或者加载属性文件
   
   2. 在resource目录下创建jdbc.properties文件，可以使用properties标签引用它
   
   3. 例如
   
      ```properties
      mysql_driver=com.mysql.jdbc.Driver
      mysql_url=jdbc:mysql://localhost:3306/mybatis
      mysql_username=root
      mysql_password=Socialbigfeng321
      ```
   
      ```xml
      <!--引入配置文件-->
      <properties resource="jdbc.properties"/>
      
      <!--可以配置多个数据库环境<environment>，通过default属性进行切换-->
      <environments default="development">
          <environment id="development">
              <transactionManager type="JDBC"/> <!--事务管理方式，以后由Spring接管-->
              <dataSource type="POOLED"> <!--数据库连接池，以后由Spring接管-->
                  <!--数据库连接信息，以后由Spring接管-->
                  <property name="driver" value="${mysql_driver}"/>
                  <property name="url" value="${mysql_url}"/>
                  <property name="username" value="${mysql_username}"/>
                  <property name="password" value="${mysql_password}"/>
              </dataSource>
          </environment>
      </environments>
      ```
   
3. 设置（settings标签）

   1. 启动二级缓存
   2. 启动延迟加载
   
   ```xml
   <!--设置MyBatis属性-->
   <settings>
       <!--开启二级缓存-->
       <setting name="cacheEnabled" value="true"/>
       <!--开启延迟加载-->
       <setting name="lazyLoadingEnabled" value="true"/>
   </settings>
   ```
   
3. 插件（plugins标签）

   用于配置MyBatis插件（例如：分页插件）
   
   ```xml
   <!--plugins标签：用于配置MyBatis插件（例如：分页插件）-->
   <plugins>
       <plugin interceptor=""></plugin>
   </plugins>
   ```
   
3. **注意：**

   1. **配置各个标签时，需要遵守前后顺序**
   2. 顺序就是按照简介中的==配置文档的顶层结构的顺序==即可

### 五、CRUD的两种方式

#### 1. 配置文件方式

> 所谓配置文件完成增删改查，就是==将SQL语句配置到SQL映射文件中去==

##### 1.1 简单查询

1. ==数据库键名称与pojo类属性**命名不一致**的问题：==

   解决办法：

   在对应SQL映射文件中，添加**`<resultMap>`标签**配置，如下：

   ```xml
   <mapper namespace="com.feng.mapper.BrandMapper">
   
       <!--
           数据库键与Java属性的映射（当两者命名不一致时使用）
           id：唯一表示
           type：映射的类型，支持别名（不区分大小写）
       -->
       <resultMap id="brandResultMap" type="brand">
           <!--
               <id>标签：完成主键映射
               <result>标签：完成普通键映射
   
               column属性：表的列名
               property属性：Java属性
           -->
           <result column="brand_name" property="brandName"/>
           <result column="company_name" property="companyName"/>
       </resultMap>
   
       <!--Statement-->
       <!--为了映射，使用resultMap属性替换resultType属性-->
       <select id="selectAll" resultMap="brandResultMap">
           select * from tb_brand
       </select>
       <select id="selectById" resultMap="brandResultMap">
           select * from tb_brand where id = #{id}
       </select>
       
   </mapper>
   ```

2. ==表示参数占位符的两种方法：==

   1. **#{}：在#{}中填写sql参数名，进行预编译，编译时会将参数名替换为 ？ ，这是为了防止SQL注入**

   2. ${}：在${}中填写sql参数名，不会预编译，采用的是字符串拼接，会存在SQL注入问题

   3. 例子：

      ```xml
      <select id="selectById" resultMap="brandResultMap">
              select * from tb_brand where id = #{id}
      </select>
      ```

3. ==特殊字符处理：==

   1. 转义字符：例如：小于号 "<" 的转义字符为 `&lt;` （其他的上网查）

   2. CDATA区：用 `<![CDATA[ ]]>` 将特殊字符包裹，例如：`<![CDATA[ < ]]>`

   3. 例子：

      ```xml
      <select id="selectById" resultMap="brandResultMap">
              select * from tb_brand where id &lt; #{id}
      </select>
      
      <select id="selectById" resultMap="brandResultMap">
              select * from tb_brand where id 
          	<![CDATA[ 
      			< 
      		]]>
          	#{id}
      </select>
      ```

4. ==parameterType属性：==

   select标签parameterType属性，用于设置参数类型，**该参数可省略**

##### 1.2 多条件查询

1. SQL语句**设置多个参数**的方式：

   SQL语句：

   ```xml
   <select id="selectByCondition" resultMap="brandResultMap">
           select * from tb_brand
           where status = #{status}
           and company_name like #{companyName}
           and brand_name like #{brandName};
   </select>
   ```

2. Mapper接口方法==参数设置==的三种方式：

   1. **散装参数**：使用@Param("SQL中的参数占位符名称")注解表示，例如：

      ```java
      List<Brand> selectByCondition(
                  @Param("status")int status,
                  @Param("companyName")String companyName,
                  @Param("brandName")String brandName);
      ```

   2. **实体类封装参数**，保证SQL中参数名和实体类属性名对应上即可，例如：

      ```java
      List<Brand> selectByCondition(Brand brand);
      ```

   3. **Map集合封装参数**，保证SQL中参数名和Map集合的键名对应上即可，例如：

      ```java
      List<Brand> selectByCondition(Map map);
      ```

3. ==模糊查询==的**参数处理**：

   获取到参数后，要对使用模糊查询的参数做处理，例如：

   ```java
   companyName = "%" + companyName + "%";
   brandName = "%" + brandName + "%";
   ```

4. **使用以上方法完成多条件查询的弊端：**
   1. ==SQL语句是静态的==，需要填写规定的参数，但用户未必将所有的条件查询参数都填写上
   2. 一旦有参数为空，就无法查询出数据（因为sql中是 and 连接）
   3. 解决方式：使用==动态SQL==

##### 1.3 动态条件查询

1. 介绍：

   1. **SQL语句会随着用户的输入或外部条件的变化而变化**，我们称为 ==动态SQL==
   2. 动态 SQL 是 MyBatis 的强大特性之一

2. **多条件动态SQL的使用：**（和JSTL的感觉很像，只是用标签将sql进行包裹）

   1. `<if></if>`标签：

      1. **语法：**

         ```xml
         <if test="条件表达式1 and 条件表达式2 ">
             sql语句
         </if>
         ```

         例子：

         ```xml
         <select id="selectByCondition" resultMap="brandResultMap">
             select * from tb_brand
             where 
               <if test="status != null">
                   status = #{status}
               </if>
               <if test="companyName != null and companyName != '' ">
                   and company_name like #{companyName}
               </if>
               <if test="brandName != null and brandName != '' ">
                   and brand_name like #{brandName}
               </if>
         </select>
         ```

      2. **存在问题：**

         1. 如果没有输入第一个参数就无法成功查询：

            SQL语句变成：select * from tb_brand where and company_name like ? ... ;

         2. 如果没有输入任何参数，也无法完成查询：

            SQL语句变成：select * from tb_brand where;

      3. **问题解决(笨方法)：**

         在 where 后面加上一个恒等式，例如：

         ```xml
         <select id="selectByCondition" resultMap="brandResultMap">
             select * from tb_brand
             where 1 = 1
               <if test="status != null">
                   and status = #{status}
               </if>
               <if test="companyName != null and companyName != '' ">
                   and company_name like #{companyName}
               </if>
               <if test="brandName != null and brandName != '' ">
                   and brand_name like #{brandName}
               </if>
         </select>
         ```

   2. `<where></where>`标签：

      1. 用`<where></where>`标签来替换sql中的where关键字

      2. 可以解决使用`<if></if>`标签存在的问题

      3. 例子：

         ```xml
         <select id="selectByCondition" resultMap="brandResultMap">
             select * from tb_brand
             <where>
                 <if test="status != null">
                     and status = #{status}
                 </if>
                 <if test="companyName != null and companyName != '' ">
                     and company_name like #{companyName}
                 </if>
                 <if test="brandName != null and brandName != '' ">
                     and brand_name like #{brandName}
                 </if>
             </where>
         </select>
         ```

3. **单条件动态SQL的使用：**

   1. 单条件查询也就是用户只能从几个参数中选择一个进行填写并查询

   2. `choose(when, otherwise)`标签：选择，类似于Java中的switch

      1. 语法：

         ```xml
         <choose> <!--类似于switch-->
             <when test="条件表达式"> <!--类似于case-->
                sql语句(单条件，前面不加and)
             </when>
             <when test="条件表达式1 and 条件表达式2 ">
                 sql语句(单条件，前面不加and)
             </when>
             ...
             <otherwise> <!--类似于defuse-->
                 sql语句（一般这里写恒等式：1 = 1）
             </otherwise>
         </choose>
         ```

         **一般在otherwise里写恒等式：1 = 1，或者用where标签包裹，防止用户没写任何参数，**

         **导致sql变成：select * from AAA where;**

      2. 例子：

         ```xml
         <select id="selectByConditionSingle" resultMap="brandResultMap">
                 select * from tb_brand
                 where
                 <choose> <!--类似于switch-->
                     <when test="status != null"> <!--类似于case-->
                         status = #{status}
                     </when>
                     <when test="companyName != null and companyName != '' ">
                         company_name like #{companyName}
                     </when>
                     <when test="brandName != null and brandName != '' ">
                         brand_name like #{brandName}
                     </when>
                     <otherwise> <!--类似于defuse-->
                         1 = 1
                     </otherwise>
                 </choose>
         </select>
         ```

         ```xml
         <select id="selectByConditionSingle" resultMap="brandResultMap">
                 select * from tb_brand
                 <where>
                 <choose> <!--类似于switch-->
                     <when test="status != null"> <!--类似于case-->
                         status = #{status}
                     </when>
                     <when test="companyName != null and companyName != '' ">
                         company_name like #{companyName}
                     </when>
                     <when test="brandName != null and brandName != '' ">
                         brand_name like #{brandName}
                     </when>
                 </choose>
                 </where>
         </select>
         ```

##### 1.4 分页查询

1. SQL映射文件

   ```xml
   <!--分页查询-->
   <select id="selectAllByPage" resultMap="brandResultMap">
       select * from tb_brand
       limit #{start}, #{pageSize};
   </select>
   ```

2. Mapper

   ```java
   /**
    * 分页查询
    * 散装参数
    * @param start
    * @param pageSize
    * @return
    */
   List<Brand> selectAllByPage(@Param("start") int start, 
                               @Param("pageSize") int pageSize);
   ```

##### 1.5 简单添加

1. 编写Mapper接口不需要返回值

2. 参数：==除了主键id==之外的所有数据（主键自增长，不需要输入）

3. 在SQL映射文件中编写sql，例子：

   ```xml
   <insert id="add">
           insert into tb_brand (brand_name, company_name, ordered, description, status)
           values(#{brandName},#{companyName},#{ordered},#{description},#{status})</insert>
   ```

4. **执行方法时，注意两点：**

   MyBatis事务中：

   1. openSession()方法：**默认开启事务**，增删改操作后，需要调用sqlSession().commit(); 手动提交事务
   2. openSession(true)方法：调用时传入true，设置为自动提交事务（也就是关闭事务）

##### 1.6 主键返回添加

1. 在数据添加成功后，需要获取插入数据库数据的主键的值来干些事情。比如：添加订单和订单项

   1. 添加订单
   2. 添加订单项，订单项中需要设置所属订单的id

2. 实现方法：

   1. 在SQL映射文件中，

      设置 insert标签属性 **useGeneratedKeys**为==true==，设置属性**keyProperty**为==主键名称==

      ```xml
      <insert id="add" useGeneratedKeys="true" keyProperty="id">
              insert into tb_brand 
          	(brand_name, company_name, ordered, description, status)
              values(#{brandName},#{companyName},#{ordered},#{description},#{status})
      </insert>
      ```

   2. 通过封装这条数据的对象，使用封装对象对应的get方法获取主键值，例如：

      ```java
      Integer id = brand.getId();
      ```

##### 1.7 全部字段修改

1. Mapper接口方法：

   1. 可以不返回值
   2. 可以返回int值，受影响行数

2. SQL映射文件：

   ```xml
   <update id="update">
           update tb_brand
           set brand_name = #{brandName},
               company_name = #{companyName},
               ordered = #{ordered},
               description = #{description},
               status = #{status}
           where id = #{id}
   </update>
   ```

##### 1.8 动态字段修改

1. Mapper接口方法：

   1. 返回值：void 或者 int
   2. 参数：pojo类（也就是将来的部分数据，封装到对象中）

2. SQL映射：**用`<set></set>`标签 替代set关键字，解决逗号连接问题**

   ```xml
   <update id="update">
           update tb_brand
           <set>
               <if test="status != null">
                   status = #{status},
               </if>
               <if test="companyName != null and companyName != '' ">
                   company_name = #{companyName},
               </if>
               <if test="brandName != null and brandName != '' ">
                   brand_name = #{brandName},
               </if>
               <if test="ordered != null and ordered != '' ">
                   ordered = #{ordered},
               </if>
               <if test="description != null and description != '' ">
                   description = #{description}
               </if>
           </set>
           where id = #{id}
   </update>
   ```

##### 1.9 单个删除

1. Mapper接口方法：

   1. 返回值：void 或者 int
   2. 参数：主键id

2. SQL映射：

   ```xml
   <delete id="deleteById">
           delete from tb_brand where id = #{id}
   </delete>
   ```

##### 1.10 批量删除

1. Mapper接口方法：

   ```java
   int deleteByIds(@Param("ids")int[] ids)
   ```

   1. 返回值：void 或者 int
   2. 参数：**主键id数组**

2. SQL映射：

   sql语句：

   ```sql
   delete from tb_brand where id in (?,?,?);
   ```

   由于不知道传入的id有几个，所以不能确定写多少个占位符。

   **使用`<foreach></foreach>`标签，遍历id数组：**

   1. 语法：

      ```xml
      <foreach collection="遍历的数组" item="遍历的元素" separator="," open="(" close=")">
              #{遍历的元素}
      </foreach>
      ```

   2. 例子：

      ```xml
      <delete id="deleteByIds">
              delete from tb_brand where id
              in
              <foreach collection="ids" item="id" separator="," open="(" close=")">
                  #{id}
              </foreach>
      </delete>
      ```

3. 细节：

   **在SQL映射中，MyBatis会将数组参数，封装为一个Map集合**，默认key名称为array，value就是数组，

   因此：

   1. foreach标签中的collection属性值要写==默认的 array==
   2. 或者在Mapper接口方法中使用==@Param("数组名称")注解==，这样在collection属性值中就可以写==数组名称==
   3. foreach标签中的separator属性值为"," 也就是用逗号将参数值隔开
   4. open="("  和 close=")"  表示 in 关键字后的括号，以左括号开始，右括号结束

#### 2. MyBatis参数传递

1. MyBatis mapper接口方法中可以接收**各种各样的参数**，MyBatis底层对于这些参数进行==不同的封装处理方式==

2. 原理：

   1. MyBatis提供了 ParamNameResolver类来进行参数封装
   2. 其中的一个方法 getNamedParams() 执行参数的封装过程

3. **结论：**

   1. 单个参数：
      1. POJO类型：直接使用，实体类属性名 和 参数占位符名称一致
      2. Map集合：直接使用，键名 和 参数占位符名称一致
      3. Collection：封装为Map集合，使用@Param注解来修改Map集合中默认的键名
      4. List：封装为Map集合，使用@Param注解来修改Map集合中默认的键名
      5. Array：封装为Map集合，使用@Param注解来修改Map集合中默认的键名
      6. 其他类型：直接使用
   2. 多个参数：封装为Map集合，使用@Param注解来修改Map集合中默认的键名

4. **建议：**

   ==将来都使用@Param注解来修改Map集合中默认的键名，并使用修改后的名称来获取值，这样可读性更高==

#### 3. 注解开发方式

##### 3.1 介绍

1. 所谓使用注解开发完成增删改查，就是==把sql语句写到注解中去==

2. 在Mapper接口方法上定义SQL注解，并写入对应sql语句即完成注解开发

3. 例子：

   ```java
   @Select("select * from tb_user where id = #{id}")
   public User selectById(int id)
   ```

##### 3.2 注解完成增删改查

1. 分类：
   1. 查询：==@Select==
   2. 添加：==@Insert==
   3. 修改：==@Update==
   4. 删除：==@Delete==
2. 建议：
   1. 注解方式完成简单功能
   2. 配置文件方式完成复杂功能

### 六、CRUD事务管理

> SqlSession 对象
>
> - getMapper(DAO.class)：获取Mapper（DAO接口的实例）
> - 事务管理

#### 1. 手动提交

- sqlSession.commit(); 提交事务
- sqlSession.rollback(); 回滚事务

```java
@Test
public void testUpdate() throws IOException {

    //假如用户传入了数据：
    int ordered = 1500;
    int id = 5;
    //封装数据
    Brand brand = new Brand();
    brand.setOrdered(ordered);
    brand.setId(id);

    String resource = "mybatis-config.xml";
    InputStream inputStream = Resources.getResourceAsStream(resource);
    SqlSessionFactory sqlSessionFactory =
        new SqlSessionFactoryBuilder().build(inputStream);
    SqlSession sqlSession = sqlSessionFactory.openSession();

    try {
        //通过会话获取DAO对象
        BrandMapper brandMapper = sqlSession.getMapper(BrandMapper.class);
        //测试DAO中方法
        int update = brandMapper.update(brand);
        //操作完成，手动提交事务
        this.sqlSession.commit();
        System.out.println(update);
    } catch (Exception e) {
		//操作出现异常，调用rollback进行回滚
        sqlSession.rollback();
    }
    this.sqlSession.close();
}
```

#### 2. 自动提交

> 通过SqlSessionFactory调用openSession方法获取SqlSession对象时，可以通过参数设置事务是否自动提交:
>
> - 如果参数设置为true，表示自动提交事务：factory.openSession(true);
> - 如果参数设置为false，或者不设置，表示手动提交：factory.openSession(false);factory.openSession();

**注意：如果数据库操作不止一项，自动提交存在风险**（上个操作完成自动提交，下个操作失败也无法回滚）

### 七、MyBatis工具类封装

> 在获取Mapper对象的步骤中，重复性高，将 会话工厂SqlSessionFactory、会话SqlSession、Mapper对象 的获取操作进行封装，之后只需要通过该工具类进行Mapper对象获取即可

```java
/**
 * 会话工厂SqlSessionFactory、会话SqlSession、Mapper对象的获取工具类
 */
public class MyBatisUtil {

    private static SqlSessionFactory factory;
    
    /**
     * 使用ThreadLocal存放sqlSession对象，
     * 可以保证同一线程访问ThreadLocal获取的sqlSession对象是同一个
     * ThreadLocal为每个线程保存仅属于自己的对象
     * 不同线程对同一个ThreadLocal对象的访问，
     * 获取的都是线程本地的对象（底层是map实现）
     */
    private static final ThreadLocal<SqlSession> local = new ThreadLocal<SqlSession>();

    static {
        try {
            InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
            factory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取：SqlSessionFactory 会话工厂
     * @return SqlSessionFactory
     */
    public static SqlSessionFactory getFactory(){
        return factory;
    }

    /**
     * 获取：SqlSession 会话
     * 无参，默认手动提交事务
     * @return SqlSession
     */
    public static SqlSession getSqlSession(){
        return getSqlSession(false);
    }

    /**
     * 获取：SqlSession 会话
     * @param isAutoCommit: true，自动提交；false，手动提交
     * @return SqlSession
     */
    public static SqlSession getSqlSession(boolean isAutoCommit){
        SqlSession sqlSession = local.get();
        if(sqlSession == null){
            sqlSession = factory.openSession(isAutoCommit);
            local.set(sqlSession);
        }
        return sqlSession;
    }

    /**
     * 获取：Mapper
     * 默认手动提交事务
     * @param c Mapper的Class对象
     * @param <T> Mapper类型
     * @return Mapper
     */
    public static <T>T getMapper(Class<T> c){
        return getMapper(c,false);
    }

    /**
     * 获取：Mapper
     * @param c Mapper的Class对象
     * @param <T> Mapper类型
     * @param isAutoCommit: true，自动提交；false，手动提交
     * @return Mapper
     */
    public static <T>T getMapper(Class<T> c, boolean isAutoCommit){
        SqlSession sqlSession = getSqlSession(isAutoCommit);
        return sqlSession.getMapper(c);
    }
    
    /**
     * 提交事务
     */
    public static void commit(){
        SqlSession sqlSession = local.get();
        sqlSession.commit();
    }

    /**
     * 回滚事务
     */
    public static void rollback(){
        SqlSession sqlSession = local.get();
        sqlSession.rollback();
    }
    
    /**
     * 关闭SqlSession会话
     */
    public static void closeSqlSession(){
        SqlSession sqlSession = local.get();
        if(sqlSession != null){
            sqlSession.close();
            local.set(null);
        }
    }
}
```

### 八、分页插件

> 分页插件是一个独立于MyBatis框架之外的第三方插件

#### 1. 添加依赖

```xml
<!--pagehelper分页插件-->
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>5.3.0</version>
</dependency>
```

#### 2. 配置插件

> 在MyBatis的主配置文件中通过plugins标签进行配置

```xml
<!--plugins标签：用于配置MyBatis插件（例如：分页插件）-->
<plugins>
    <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
</plugins>
```

#### 3. 分页示例

> 对Brand全部信息进行分页查询

```java
@Test//使用pagehelper进行分页查询
public void testSelectByPagehelper(){

    //使用pagehelper可以将查询数据和页码、每页数据条数等分页信息封装起来
    //不需要再手动完成这些逻辑实现，十分方便，并且使得前端完成分页显示十分便捷
    
    BrandMapper brandMapper = MyBatisUtil.getMapper(BrandMapper.class);

    //设置分页（参数一：第几页；参数二：一页中有几条数据）
    PageHelper.startPage(1, 2);
    List<Brand> brands = brandMapper.selectAll();
    MyBatisUtil.closeSqlSession();

    //将查询数据封装到pageInfo中
    PageInfo<Brand> pageInfo = new PageInfo<Brand>(brands);

    //从pageInfo中可以获取分页的各种数据:
    //获取数据
    List<Brand> list = pageInfo.getList();
    for (Brand brand : list) {
        System.out.println(brand);
    }
    //获取当前页码
    int pageNum = pageInfo.getPageNum();
    System.out.println("第"+pageNum+"页");

}
```

### 九、关联映射

#### 1. 实体关系

> 实体——数据实体，实体关系指的是数据与数据之间的关系
>
> 例如：用户和角色、房屋和楼栋、订单和商品

实体关系分为一下四种：

- **一对一关联**：

  实例：人和身份证、学生和学生证、用户基本信息表和用户详情表

  数据表关系：

  - 主键关联（用户表主键 和 详情表主键 相同时，表示是匹配的数据）

    ![image-20220424222228968](..\image\image-20220424222228968.png)

  - 唯一外键关联

    ![image-20220424222613170](..\image\image-20220424222613170.png)

- **一对多关联、多对一关联**

  实例：

  - 一对多：班级和学生、类别和商品
  - 多对一：学生和班级、商品和类别

  数据表关系：

  - 在“多”的一端添加外键 和 “一”的一端进行关联

- **多对多关联**

  实例：用户和角色、角色和权限、房屋和业主、学生和社团、订单和商品

  数据表关系：建立第三张关系表，添加两个外键，分别与两张表主键进行关联

#### 2. 一对一关联

> 用户信息表和用户详情表，一对一关联，查询用户信息表时 关联查询 出用户详情

**数据表：**

> details中唯一外键 uid 关联 users中的主键user_id

```sql
-- 用户信息表
create table users(
	user_id int primary key auto_increment,
	user_name varchar(20) not null unique,
	user_pwd varchar(20) not null,
	user_realname varchar(20) not null,
	user_img varchar(100) not null
);

-- 用户详情表
create table details(
	detail_id int primary key auto_increment,
	user_addr varchar(50) not null,
	user_tel char(11) not null,
	user_desc varchar(200),
	uid int not null unique
	-- constraint FK_USER foreign key(uid) references users(user_id)
);
```

**实体：**

> 一对一关联的两个实体，其中一个实体包含另一个实体类对象属性，作为关联

| User                                                         | Detail                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![image-20220425211039452](..\image\image-20220425211039452.png) | ![image-20220425211140770](..\image\image-20220425211140770.png) |

**映射文件：**

==连接查询==

> 在resultMap标签中，增加关联映射对象的属性，property属性值用 对象.属性名 赋值

| SQL映射文件                                                  |
| ------------------------------------------------------------ |
| ![image-20220425211407184](..\image\image-20220425211407184.png) |

==子查询==

> 在resultMap标签中，使用association标签，调用子查询
>
> association标签：
>
> - select：要执行的子查询，值为某个mapper中的方法
> - property：赋值为子查询结果的属性
> - column：执行子查询所携带的参数（注意是表中的列名）

| SQL映射文件                                                  |
| ------------------------------------------------------------ |
| DetailMapper映射文件，提供根据uid查询所有信息的sql           |
| ![image-20220425215103032](..\image\image-20220425215103032.png) |
| UserMapper映射文件，关联查询一个detail对象，然后赋值到这个user的detail属性中 |
| ![image-20220425215402368](..\image\image-20220425215402368.png) |

#### 3. 一对多关联

> 以班级的角度，查询这个班级时，查询出这个班级关联的所有学生

**数据表：**

> students中外键scid 关联 classes中的主键cid

```sql
-- 创建班级信息表
create table classes(
	cid int primary key auto_increment,
	cname varchar(30) not null unique,
	cdesc varchar(100)
);

-- 创建学生信息表
create table students(
    sid char(5) primary key,
    sname varchar(20) not null,
    sage int not null,
    scid int not null
);
```

**实体：**

> Clazz类由于查询出来关联的student对象不止一个，因此设置一个List集合进行存储

| Class                                                        | Student                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![image-20220425224120480](..\image\image-20220425224120480.png) | ![image-20220425222726249](..\image\image-20220425222726249.png) |

**映射文件：**

==连接查询==

> 在resultMap标签中，使用collection标签
>
> collection标签：
>
> - property：查询到的集合所放到的属性
> - ofType：集合元素类型
>
> 在collection标签中用result子标签声明集合元素（student）中的属性

| SQL映射文件                                                  |
| ------------------------------------------------------------ |
| ![image-20220425232553616](..\image\image-20220425232553616.png) |

==子查询==

> 在resultMap标签中，使用collection标签
>
> collection标签：
>
> - property：查询到的集合所放到的属性
> - select：要执行的子查询，值为某个mapper中的方法
> - column：执行子查询所携带的参数（注意是表中的列名）

| SQL映射文件                                                  |
| ------------------------------------------------------------ |
| StudentMapper映射文件，提供根据cid查询所有信息的sql          |
| ![image-20220425234808104](..\image\image-20220425234808104.png) |
| ClassMapper映射文件，关联查询一个student集合，然后赋值到这个clazz的stus属性中 |
| ![image-20220425234958975](..\image\image-20220425234958975.png) |

#### 4. 多对一关联

> 以学生的角度，查询这个学生时，关联查询出这个学生所在的班级
> （实现方法 其实和 一对一关联 的实现方法一样；单独看一个学生时，就是一个学生和一个班级一对一关联）

**实体：**

> 以学生为角度，则在Student类中添加一个Clazz对象作为映射，而Clazz类中则不再需要关联的学生集合

| Student                                                      | Clazz                                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![image-20220426152531249](..\image\image-20220426152531249.png) | ![image-20220426152314502](..\image\image-20220426152314502.png) |

**映射文件：**

==连接查询==

> 在resultMap标签中，增加关联映射对象的属性，property属性值用 对象.属性名 赋值

| SQL映射文件                                                  |
| ------------------------------------------------------------ |
| ![image-20220426153432284](..\image\image-20220426153432284.png) |

==子查询==

> 在resultMap标签中，使用association标签，调用子查询
>
> association标签：
>
> - select：要执行的子查询，值为某个mapper中的方法
> - property：赋值为子查询结果的属性
> - column：执行子查询所携带的参数（注意是表中的列名）

| SQL映射文件                                                  |
| ------------------------------------------------------------ |
| ClassMapper映射文件，提供根据cid查询所有信息的sql            |
| ![image-20220426153959691](..\image\image-20220426153959691.png) |
| StudentMapper映射文件，关联查询一个clazz对象，然后赋值到这个student的clazz属性中 |
| ![image-20220426154155503](..\image\image-20220426154155503.png) |

#### 5. 多对多关联

> 多个学生 关联 多个课程

**数据表：**

```sql
-- 学生信息表（如上）
-- 课程信息表
create table courses(
	course_id int primary key auto_increment,
    course_name varchar(50) not null
);
-- 选课信息表/成绩表（第三方表）
create table grades(
	sid char(5) not null,
    cid int not null,
    score int not null
);
```

**实体（两种情况）**根据业务需求自行选择

> 查询学生时，同时查询学生选择的课程，成绩表（第三方表）不需要创建实体

| Student                                                      | Course                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![image-20220426164107132](..\image\image-20220426164107132.png) | ![image-20220426164201355](..\image\image-20220426164201355.png) |

> 根据课程编号查询课程时，同时查询选择了这门课程的学生，成绩表（第三方表）不需要创建实体

| Student                                                      | Course                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![image-20220426164312010](..\image\image-20220426164312010.png) | ![image-20220426164358252](..\image\image-20220426164358252.png) |

**SQL映射文件（从课程角度为例）**

==连接查询==

> sql中通过 第三方表（成绩表）连接查询，resultMap封装结果对象，使用collection标签(学生对象不止一个)

| SQL映射文件                                                  |
| ------------------------------------------------------------ |
| ![image-20220426172515186](..\image\image-20220426172515186.png) |

==子查询==

> StudentMapper映射文件中提供根据 课程id查询学生信息的sql， 通过成绩表连接查询完成；
>
> CourseMapper映射文件使用collection标签调用子查询，封装到students集合属性中(学生对象不止一个)

| SQL映射文件                                                  |
| ------------------------------------------------------------ |
| StudentMapper映射文件，提供根据 course_id 查询所有信息的sql  |
| ![image-20220426174302136](..\image\image-20220426174302136.png) |
| CourseMapper映射文件，关联查询一个student集合，然后赋值到这个course的students属性中 |
| ![image-20220426174615457](..\image\image-20220426174615457.png) |

#### 6. 秘诀小总结

1. 连接查询：

   - 一对一关联：resultMap中直接用result标签，声明映射对象的属性
   - 一对多关联：resultMap中用collection标签，声明映射对象的属性（因为存在多个对象用集合存储）

2. 子查询：

   - 一对一关联：resultMap中用association标签，调用子查询
   - 一对多关联：resultMap中用collection标签，调用子查询（因为存在多个对象用集合存储）

3. 多对一关联，从“多”的角度出发，就是一对一关联，例如从学生角度出发，一个学生和一个班级一对一关联。

   所以**多对一关联 实现方法 和 一对一关联 相同**。

4. 多对多关联，**建立第三张表，完成sql语句的书写**，resultMap封装结果，不管是连接查询还是子查询，都用collection标签完成，因为关联对象不止一个(多对多)

### 十、Druid连接池配置

> MyBatis作为一个ORM框架，在进行数据库操作时，是需要和数据库创建连接的，MyBatis支持基于数据库连接池的连接创建创建方式。
>
> 当配置MyBatis数据源时，只要配置了dataSource标签的type属性值为POOLED时，就可以使用MyBatis内置的连接池管理连接。
>
> 如果想要使用第三方的数据库连接池，则需要进行自定义配置

#### 1. 常见连接池

- DBCP
- C3P0
- Druid 性能也比较好，提供了比较便捷的监控系统
- Hikari 性能最好

| 功能            | dbcp                | druid              | c3p0                               | HikariCP                           |
| --------------- | ------------------- | ------------------ | ---------------------------------- | ---------------------------------- |
| 是否支持PSCache | 是                  | 是                 | 是                                 | 否                                 |
| 监控            | jmx                 | jmx/log/http       | jmx,log                            | jmx                                |
| 扩展性          | 弱                  | 好                 | 弱                                 | 弱                                 |
| sql拦截及解析   | 无                  | 支持               | 无                                 | 无                                 |
| 代码            | 简单                | 中等               | 复杂                               | 简单                               |
| 更新时间        | 2015.8.6            | 2015.10.10         | 2015.12.09                         | 2015.12.3                          |
| 特点            | 依赖于common-pool   | 阿里开源，功能全面 | 历史久远，代码逻辑复杂，且不易维护 | 优化力度大，功能简单，起源于boneCP |
| 连接池管理      | LinkedBlockingDeque | 数组               |                                    | threadlocal+CopyOnWriteArrayList   |

#### 2. 添加Druid依赖

```xml
<dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.2.4</version>
</dependency>
```

#### 3. 创建Druid连接池工厂

> 创建一个类继承PooledDataSourceFactory，构建无参构造，修改属性dataSource 为 DruidDataSource

```java
public class DruidDataSourceFactory extends PooledDataSourceFactory {
    public DruidDataSourceFactory() {
        //修改 父类属性dataSource 为DruidDataSource
        this.dataSource = new DruidDataSource();
    }
}
```

#### 4. 配置MyBatis数据源

> 修改dataSource标签信息，将Druid连接池配置到数据源中

```xml
<environments default="developmentByDruid">

    <!--使用Druid连接池的environment-->
    <environment id="developmentByDruid">
        <transactionManager type="JDBC"/>
        <!--
            dataSource：
            MyBatis需要一个连接池工厂，而且必须是PooledDataSourceFactory类型的
            MyBatis通过这个工厂获取数据库连接池，就是这个工厂中的dataSource属性
            所以必须创建PooledDataSourceFactory的子类，修改其中的dataSource为DruidDataSource
            而type属性的值，就是所创建的这个子类
        -->
        <dataSource type="com.feng.utils.DruidDataSourceFactory">
            <!--Druid数据源命名规则：-->
            <property name="driverClass" value="${mysql_driver}"/>
            <property name="jdbcUrl" value="${mysql_url}"/>
            <property name="username" value="${mysql_username}"/>
            <property name="password" value="${mysql_password}"/>
        </dataSource>
    </environment>

</environments>
```

> Druid连接池日志信息：
> 【INFO】 21:08:39.420 【main】 c.a.d.p.DruidDataSource - {dataSource-1} inited 

### 十一、缓存机制

> MyBatis是基于JDBC的封装，使数据库操作更加便捷；MyBatis除了对JDBC操作步骤进行封装之外，也对其性能进行了优化：
>
> - 在MyBatis引入了缓存机制，用于提升MyBatis的检索效率
> - 在MyBatis引入了延迟加载机制，用于减少对数据库不必要的访问

#### 1. 缓存工作原理

> 缓存，就是存储数据的内存

| 工作原理                                                     |
| ------------------------------------------------------------ |
| ![image-20220426220459532](..\image\image-20220426220459532.png) |

#### 2. MyBatis的缓存

> MyBatis缓存分为一级缓存和二级缓存

##### 2.1 一级缓存

> 一级缓存也叫作==SqlSession级缓存==，**为每个SqlSession单独分配的缓存内存**，无需手动开启可直接使用；多个SqlSession的缓存是不共享的。
>
> 特性：
>
> 1. 如果多次查询使用的是同一个SqlSession对象，则第一次查询之后数据会存放到缓存，后续的查询则直接访问缓存中存储的数据。
> 2. 如果第一次查询完成之后，对查询出的对象进行修改（此修改会影响到缓存），第二次查询会直接访问缓存，造成第二次查询的结果与数据库不一致。
> 3. 当再次查询时，若想要跳过缓存直接查询数据库，可通过sqlSession.clearCache();来清楚当前sqlSession的缓存。
> 4. 如果第一次查询之后第二次查询之前，使用当前sqlSession执行了数据库修改操作，此修改会使第一次查询缓存的数据失效，因此第二次查询会再次访问数据库。

测试代码

```java
public class CacheTest {

    /**
     * 一级缓存：
     * 如果多次查询使用的是同一个SqlSession对象，则第一次查询之后数据会存放到缓存，
     * 后续的查询则直接访问缓存中存储的数据
     */
    @Test
    public void testL1Cache01(){

        //这样获取mapper，两个mapper对应的是同一个sqlSession对象，那么一级缓存生效
        UserMapper mapper1 = MyBatisUtil.getMapper(UserMapper.class);
        UserMapper mapper2 = MyBatisUtil.getMapper(UserMapper.class);

        //这样获取mapper，两个mapper对应的不是同一个sqlSession对象，那么没有使用一级缓存
        UserMapper mapper3 = MyBatisUtil.getFactory().openSession().
            				 getMapper(UserMapper.class);
        UserMapper mapper4 = MyBatisUtil.getFactory().openSession().
            				 getMapper(UserMapper.class);

        //第一次查询王五
        User user1 = mapper1.queryUser("wangwu");
        System.out.println(user1);
        System.out.println("=======================================");

        //第二次查询王五的结果：从缓存中获取的，控制台上没有sql的打印
        User user2 = mapper2.queryUser("wangwu");
        System.out.println(user2);
        System.out.println("=======================================");

        //第一次查询张三
        User user3 = mapper3.queryUser("zhangsan");
        System.out.println(user3);
        System.out.println("=======================================");

        //第二次查询张三的结果：从数据库中获取的，控制台上有sql的打印
        User user4 = mapper4.queryUser("zhangsan");
        System.out.println(user4);
        System.out.println("=======================================");

    }

    /**
     * 一级缓存：
     * 如果第一次查询完成之后，对查询出的对象进行修改（此修改会影响到缓存），
     * 第二次查询会直接访问缓存，造成第二次查询的结果与数据库不一致
     */
    @Test
    public void testL1Cache02(){

        //两个mapper对应的是同一个sqlSession对象，一级缓存生效
        UserMapper mapper1 = MyBatisUtil.getMapper(UserMapper.class);
        UserMapper mapper2 = MyBatisUtil.getMapper(UserMapper.class);

        //第一次查询王五
        User user1 = mapper1.queryUser("wangwu");
        System.out.println(user1);
        System.out.println("=======================================");

        //修改了查询结果user1中的属性值
        user1.setUserName("JOJO");

        //为了使第二次查询结果和数据库一致，可以清空缓存，从数据库中查找，那么一级缓存也没有意义了
		//SqlSession sqlSession = MyBatisUtil.getSqlSession();
		//sqlSession.clearCache();

        //第二次查询王五的结果：和数据库不一致
        User user2 = mapper2.queryUser("wangwu");
        System.out.println(user2);
        System.out.println("=======================================");

    }
}
```

##### 2.2 一级缓存使用问题

> 当多个线程进行数据库操作时，由于sqlSession对象不是同一个，可能造成的数据不一致问题
>
> 问题：
> 	线程一，第一次查询之后，线程二进行了数据库修改操作，线程一第二次查询依然显示修改前的数据，导	致查询数据与数据库不一致
>
> 分析：
> 	修改操作和查询操作不是同一个线程，因此使用的不是同一个DAO对象（sqlSession不是同一个），因此	修改操作不会导致查询操作的缓存失效，第二次查询的时候依然访问的是缓存而不是数据库
>
> 解决方案：
> 	1. 让修改操作和查询操作使用同一个sqlSession（不合理）
> 	1. 每次进行查询操作之后，sqlSession.clearCache()，清空缓存，再次查询时绕过缓存，直接查询数据库

| 一级缓存使用存在的数据不一致问题                             |
| ------------------------------------------------------------ |
| ![image-20220427171114585](..\image\image-20220427171114585.png) |

##### 2.3 二级缓存

> 二级缓存也称为==SqlSessionFactory级缓存==，**通过同一个factory对象获取的sqlSession可以共享二级缓存**；在应用服务器中SqlSessionFactory是单例的，因此二级缓存可以实现全局共享。
>
> 特性：
>
> 1. 二级缓存默认没有开启，需要在mybatis-config.xml中通过settings标签开启
> 2. 二级缓存只能缓存实现序列化接口的对象
> 3. 查询操作后必须commit提交事务，才能将结果存到缓存中

实现步骤：

- 在mybatis-config.xml中开启使用二级缓存

  ```xml
  <settings>
      <setting name="cacheEnabled" value="true"/>
  </settings>
  ```

- 在需要使用二级缓存的Mapper文件中配置cache标签使用二级缓存

  > cache标签属性：
  >
  > - eviction="FIFO"	表示收回策略    
  >
  > - flushInterval="60000"    表示刷新间隔
  >
  > - size="512"    表示引用数目
  >
  > - readOnly="true"    只读，查询返回的对象不可修改
  >
  > 不填写参数，采用默认值

  ```xml
  <!--使用二级缓存-->
  <cache/>
  ```

- 使用二级缓存的POJO实现序列化接口

  ```java
  @Data
  @NoArgsConstructor
  @AllArgsConstructor
  @ToString
  public class Student implements Serializable {
      private String stuId;
      private String stuName;
      private int stuAge;
      private int stuCid; //学生班级
  }
  ```

- 测试

  ```java
  /**
   * 二级缓存：
   * 二级缓存默认没有开启，需要在mybatis-config.xml中通过settings标签开启
   * 二级缓存只能缓存实现序列化接口的对象
   * 查询操作后必须commit提交事务，才能将结果存到缓存中
   */
  @Test
  public void testL2Cache(){
  
      //通过同一个Factory获取到的不同sqlSession
      SqlSession sqlSession1 = MyBatisUtil.getFactory().openSession();
      SqlSession sqlSession2 = MyBatisUtil.getFactory().openSession();
  
      //第一次查询
      StudentMapper mapper1 = sqlSession1.getMapper(StudentMapper.class);
      List<Student> students1 = mapper1.listStudentsByCid(1);
      System.out.println(students1);
  
      //查询过后必须提交事务才能将结果放入缓存
      sqlSession1.commit();
  
      System.out.println("===============================================");
  
      //第二次查询
      StudentMapper mapper2 = sqlSession2.getMapper(StudentMapper.class);
      List<Student> students2 = mapper2.listStudentsByCid(1);
      System.out.println(students2);
  }
  ```

##### 2.4 查询操作的缓存开关

> SQL映射文件的sql中，通过属性：
>
> - useCache：本查询是否使用二级缓存，一级查询正常使用，默认为true
> - flushCache：本查询是否自动清除缓存，一二级均有效，默认为false

```xml
<!--根据cid查询所有-->
<select id="listStudentsByCid" resultMap="studentResultMap" useCache="true" flushCache="true">
    select * from students
    where scid=#{stuCid}
</select>
```

### 十二、延迟加载

> MyBatis引入了延迟加载机制，用于减少对数据库不必要的访问：
>
> 延迟加载：如果在MyBatis开启了延迟加载，在执行了**子查询**（至少查询两次及以上）时，默认只执行第一次查询，当用到子查询的查询结果时，才会触发子查询的执行；如果无需使用子查询结果，则子查询不会执行

#### 1. 延迟加载时机

> MyBatis延迟加载时机：
>
> - 执行对主加载对象的查询时，不会立即执行对关联对象的查询（子查询）。
> - 访问主加载对象的属性时也不会执行关联对象的select查询。
> - 只有当真正访问**关联对象**的属性时，才会执行对关联对象的select 查询。

#### 2. 延迟加载开启

1. 全局开启，配置mybatis-config.xml：

   > 配置settings标签

   ```xml
   <!--设置MyBatis属性-->
   <settings>
       <!--开启延迟加载-->
       <setting name="lazyLoadingEnabled" value="true"/>
   </settings>
   ```

2. 针对某条子查询的单独开启：

   > 在执行子查询的标签中，设置属性fetchType="lazy"

   ```xml
   <!--子查询Map-->
   <resultMap id="classResultMap2" type="Clazz">
       <id column="cid" property="classId"/>
       <result column="cname" property="className"/>
       <result column="cdesc" property="classDesc"/>
       <!--调用子查询，关联查询一个集合并赋值到property属性中，子查询参数为column-->
       <!--设置该子查询的延迟加载：fetchType="lazy"-->
       <collection property="stus"
                   select="com.feng.mapper.StudentMapper.listStudentsByCid"
                   column="cid" fetchType="lazy"/>
   </resultMap>
   ```

#### 3. 测试代码

```xml
<!--子查询Map-->
<resultMap id="classResultMap2" type="Clazz">
    <id column="cid" property="classId"/>
    <result column="cname" property="className"/>
    <result column="cdesc" property="classDesc"/>
    <!--调用子查询，关联查询一个集合并赋值到property属性中，子查询参数为column-->
    <!--设置该子查询的延迟加载：fetchType="lazy"-->
    <collection property="stus" select="com.feng.mapper.StudentMapper.listStudentsByCid"
                column="cid" fetchType="lazy"/>
</resultMap>

<!--一对多关联映射：子查询-->
<select id="queryClassWithStu2" resultMap="classResultMap2">
    select *
    from classes where cname=#{className}
</select>
```

```java
/**
 * 延迟加载：
 * 只有访问结果对象的关联对象属性时，
 * 才会执行子查询
 */
@Test
public void testLazyLoading(){

    ClassMapper mapper = MyBatisUtil.getMapper(ClassMapper.class);

    //执行查询（实际只执行了第一次查询，没有执行子查询，看日志信息）
    Clazz clazz = mapper.queryClassWithStu2("Java1班");

    //访问clazz中的非关联对象属性时，也不会执行子查询，看日志信息
    System.out.println(clazz.getClassDesc());

    //访问clazz中的关联对象属性stus(学生列表)时，执行子查询返回结果，看日志信息
    System.out.println(clazz.getStus());
}
```

### 十三、笔记与代码解释

> 对应关系：
>
> - 第一章至第八章笔记内容  与 代码MyBatisTemplate 对应
> - 第九章至第十二章笔记内容 与 代码MyBatisTemplate2 对应

