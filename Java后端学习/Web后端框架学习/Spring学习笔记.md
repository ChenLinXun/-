# Spring学习笔记

### 一、Spring介绍

#### 1. 面向接口编程

1. Web项目开发中的==耦合度==问题

   1. 在Servlet中需要调用service层中的方法，则需要在Servlet类中通过**new关键字**创建service实现类对象
   2. 在service实现类中需要调用Dao层中的方法，也需要在service实现类中通过**new关键字**创建Dao实现类对象（MyBatis管理Dao层可以解决这个耦合问题）
   3. 如果**使用new关键字**创建对象：
      1. 失去了面向接口编程的灵活性
      2. 代码的侵入性增强（**增加了耦合度**）、降低了代码的灵活性

2. 解决方案（==面向接口编程==）：

   1. 在Servlet中定义service接口对象引用，**不使用new关键字**创建实现类对象，在servlet的实例化时，**通过反射**==动态==的给Service对象引用赋值
   2. 如何实现：**Spring可以做到！！！**

3. 图解：

   | 面向接口编程                                                 |
   | ------------------------------------------------------------ |
   | ![image-20220416221224333](..\image\image-20220416221224333.png) |

#### 2. Spring简介

1. Spring是**分层的** Java SE/EE应用 full-stack**轻量级开源框架**，以==Ioc==（Inverse Of Control：==控制反转==）和
   ==AOP==（Aspect Oriented Programming：==面向切面编程==）为内核，来解决企业项目开发的复杂度问题，解耦
2. 提供了**展现层**==SpringMVC== 和 **持久层**==Spring JDBCTemplate== 以及 ==业务层事务管理== 等众多的企业级应用技术，还能**整合**开源世界众多著名的**第三方框架和类库**，逐渐成为使用最多的Java EE企业应用开源框架
3. 2017年9月 发布了Spring的最新版本 Spring5.0 通用版（GA），这也是如今学习Spring的版本
4. 官网：https://spring.io/

#### 3. Spring优势

1. **轻量级：**体积小，对代码没有侵入性

2. **方便解耦，简化开发：**

   通过Spring提供的 **IoC**容器，可以**将对象间的依赖关系交由Spring进行控制**，==避免硬编码==所造成的==过度耦合==。
   用户也不必再为单例模式类、属性文件解析等这些很底层的需求编写代码，可以更专注于上层的应用

2. **AOP 编程的支持：**

   通过Spring的AOP功能，方便进行==面向切面编程==，许多不容易用传统OOP实现的功能可以通过AOP轻松实现，可以在**不改变原有业务逻辑的情况下实现对业务的增强**

3. **声明式事务的支持：**

   可以将我们从单调烦闷的事务管理代码中解脱出来，通过声明方式灵活的进行事务管理，提高开发效率和质量

4. **方便程序的测试：**

   可以用非容器依赖的变成方式进行几乎所有的测试工作，测试不再是昂贵的操作，而是随手可做的事情

5. **方便集成各种优秀框架**：

   Spring对各种优秀框架（Struts、Hibemate、Hessian、Quartz、**MyBatis**等）的支持

6. **降低 JavaEE API的使用难度**：

   Spring 对 JavaEE API（如 JDBC、JavaMail、远程调用等）进行了薄薄的封装层，使这些API的使用难度大为降低

7. **Java 源码的经典学习范例：**

   Spring的源代码==设计精妙、结构清晰、匠心独用==，处处体现着大师对Java设计模式灵活运用以及对Java技术的高深造诣。它的源码无疑是Java技术的最佳实践的范例

#### 4. Spring体系结构

1. 官网介绍：

   1. Spring发展多年，已经基于Spirng核心（IoC、AOP）开发了许多组件，我们称之为Spring全家桶

   2. Spring全家桶：

      Spring Framework（Spring(IoC、AOP)）

      Spring Cloud（微服务架构）

      Spring Boot（工具框架，快速完成项目整合）

      Spring Data（Spring提供的数据访问的客户端）

      Spring Security（安全框架）

2. 体系结构：

   1. 图解

      | Spring体系结构图解                                           |
      | ------------------------------------------------------------ |
      | ![image-20220407153728815](..\image\image-20220407153728815.png) |

   1. Core Container：
   
      > Spring容器组件，用于完成实例的创建和管理（每个组件就是一个jar包，根据需要进行引入使用）
      >
      > Core Container是Spirng提供一切功能的基础，任何时候都需要引入
      >
      > 组件（jar包）：
      >
      > - core 工作核心，必须引用
      > - beans 实例管理，依赖于core
      > - context 容器上下文
      > - SpEL 表达式语言
      
   1. AOP、Aspects
   
      > Spring AOP组件，实现面向切面编程
      >
      > 组件（jar包）：
      >
      > - aop
      > - aspects
      
   1. web
   
      > Spring web组件实际上指的是 SpringMVC框架（SpringMVC就是Spring的一个组件，基于核心），
      >
      > 实现web项目的MVC控制
      >
      > 组件（jar包）：
      >
      > - web （Spring对web项目的支持）
      > - webmvc（SpringMVC组件）
      
   4. Data Access
   
      > Spring数据访问组件，也是一个基于JDBC封装的持久层框架（即使不用MyBatis，Spring也可以完成持久化操作），但实际应用中MyBatis更加灵活，进行数据库操作用MyBatis，事务管理用Spring中的事务管理组件
      >
      > 组件（jar包）：
      >
      > - tx 实现事务管理
   
   5. Test
   
      > Spring的单元测试组件，提供了Spring环境下的单元测试支持
      >
      > 组件（jar包）：
      >
      > - test
   
3. 学习内容概述：

   1. 学习完Core Container和AOP、Aspects，相当于学完Spring（核心IoC、AOP）
   2. 学习完web，相当于学完SpringMVC
   3. 学习完Data Access和Test，相当于学完和MyBatis的整合
   4. 学习SSM，实际上是学习两个框架：Spring和MyBatis，只不过我们常把SpringMVC组件看成是一个框架

### 二、IoC控制反转

> Spring IoC 容器组件，可以完成：对象的创建、对象属性赋值、对象管理

#### 1. 基于XML

##### 1.1 框架部署

1. 创建Maven工程	
   - Java
   - Web
   
2. 添加SpringIoC依赖
   - core
   - beans
   - **context**
   - aop
   - expression
   
   SpringIoC的jar包是依赖导入的，导入context，就会导入全部五个
   
   ```xml
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-context</artifactId>
       <version>5.2.13.RELEASE</version>
   </dependency>
   ```
   
3. 创建Spring配置文件

   > 通过配置文件 “告诉” Spring容器 创建什么对象，给对象属性赋什么值

   在resource目录下创建名为==applicationContext.xml==的文件（约定好的文件名，建议使用）

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
   <!-- 对于一个xml文件，如果作为框架的配置文件，需要遵守框架的配置规则 -->
   <!-- 通常一个框架为了让开发者能够正确的配置，都会提供xml的规范文件（dtd\xsd） -->
   <!-- xsd形式引入规范文件的好处：可以按需扩展引入，也就是可以引入多个规范文件 -->
   
   </beans>
   ```

##### 1.2 IoC的使用

> 使用 SpringIoC组件 创建并管理对象，需要通过XML将类声明给Spring容器进行管理，从而通过Spring工厂完成对象的创建及属性值的注入

1. 创建一个实体类，例如：

   ```java
   public class Actor {
       private String name;
       private String sex;
       private String skill;
       private String level;
       //getter and setter
   }
   ```

2. 在Spring配置文件中配置实体类

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--通过bean标签将实体类配置给Spring进行管理
   		id：实体类唯一标识
   	-->
       <bean id="actor" class="com.feng.ioc.bean.Actor"></bean>
   
   </beans>
   ```

3. 使用Spring容器管理对象：

   > 1. 初始化Spring对象工厂，获取Spring容器对象：
   >
   >    ```java
   >    //1. 初始化Spring容器，加载Spring配置文件
   >    ClassPathXmlApplicationContext context =
   >        new ClassPathXmlApplicationContext("applicationContext.xml");
   >    ```
   >
   > 2. 通过Spring容器创建获取对象：
   >
   >    ```java
   >    //2. 通过Spring容器创建Actor类对象
   >    Actor actor = (Actor) context.getBean("actor");
   >    ```
   >
   > 3. 在Spring配置文件中的配置：
   >
   >    ```xml
   >    <!--id：类唯一标识
   >        class：类的全限定名
   >        property标签：为类实例赋值
   >    -->
   >    <bean id="actor" class="com.feng.ioc.bean.Actor">
   >        <property name="name" value="炼狱杏寿郎"/>
   >        <property name="sex" value="男"/>
   >        <property name="skill" value="炎之呼吸"/>
   >        <property name="level" value="柱"/>
   >    </bean>
   >    ```

##### 1.3 IoC 和 DI

1. 解释：

   - IoC（Inverse of Control）控制反转，通过Spring对象工厂**完成对象的创建**
   - DI（Dependency Injection）依赖注入，在Spring完成对象创建的同时依赖Spring容器**完成对象属性赋值**

2. IoC

   > 当我们需要通过Spring对象工厂创建某类对象时，需要将这个类交给Spring管理——通过bean标签配置

   ```xml
   <bean id="actor" class="com.feng.ioc.bean.Actor"></bean>
   <bean id="book" class="com.feng.ioc.bean.Book"></bean>
   ```

3. DI

   > 通过Spring容器给创建的对象属性赋值（共有三种方法，其中有两种常用方法）

   ```xml
   <bean id="actor" class="com.feng.ioc.bean.Actor">
           <property name="name" value="炼狱杏寿郎"/>
           <property name="sex" value="男"/>
           <property name="skill" value="炎之呼吸"/>
           <property name="level" value="柱"/>
   </bean>
   ```

##### 1.4 DI依赖注入

###### 1.4.1 DI三种方式

> Spring容器加载配置文件之后，通过反射创建类的对象，并给属性赋值；
>
> Spring容器通过反射实现属性==注入有三种方式==：
>
> - ==set()方法注入==
> - ==构造器注入==
> - 接口注入（不常用）

###### 1.4.2 set注入

1. 解释：

   在bean标签中通过配置**property标签**给属性赋值，Spring底层实际上就是**通过反射调用set()方法**完成注入

2. 各种属性类型的注入：

   1. > **基本数据类型和String类型：**
      > 直接通过property标签的value属性进行赋值
      >
      > 例如：
      >
      > ```xml
      > <bean id="actor" class="com.feng.ioc.bean.Actor">
      >         <property name="name" value="炼狱杏寿郎"/> <!--name是String类型-->
      >         <property name="age" value="23"/>  <!--age是int类型-->
      > </bean>
      > ```
      >
      > 底层：会检查属性的类型，并且和value中的值进行匹配

   2. > **日期类型：**
      >
      > - 方式一：在property标签中通过 ref 引用Spring容器中的一个对象
      >
      >   ```xml
      >   <bean id="date" class="java.util.Date"></bean>
      >   <bean id="actor" class="com.feng.ioc.bean.Actor">
      >       <property name="date" ref="date"/>
      >   </bean>
      >   ```
      >
      > - 方式二：在property标签中添加子标签bean来指定对象
      >
      >   ```xml
      >   <bean id="actor" class="com.feng.ioc.bean.Actor">
      >       <property name="date">
      >           <bean class="java.util.Date"/>
      >       </property>
      >   </bean>
      >   ```

   3. > **自定义类对象类型：**（和日期类型的方法一样）
      >
      > - 方式一：在property标签中通过 ref 引用Spring容器中的一个对象
      >
      >   ```xml
      >   <bean id="teacher" class="com.feng.ioc.bean.Teacher">
      >       <property name="name" value="炼狱槙寿郎"/>
      >       <property name="age" value="45"/>
      >   </bean>
      >   
      >   <bean id="actor" class="com.feng.ioc.bean.Actor">
      >       <property name="name" value="炼狱杏寿郎"/>
      >       <property name="teacher" ref="teacher"/>
      >   </bean>
      >   ```
      >
      > - 方式二：在property标签中添加子标签bean来指定对象
      >
      >   ```xml
      >   <bean id="actor" class="com.feng.ioc.bean.Actor">
      >       <property name="name" value="炼狱杏寿郎"/>
      >       <property name="teacher">
      >           <bean class="com.feng.ioc.bean.Teacher">
      >               <property name="name" value="炼狱槙寿郎"/>
      >               <property name="age" value="45"/>
      >           </bean>
      >       </property>
      >   </bean>
      >   ```

   4. > **集合类型：**
      >
      > - ==List==
      >
      >   1. List<String>  List中的元素是字符串或者基本类型的封装类
      >      直接使用value属性赋值：
      >
      >      ```xml
      >      <property name="hobbies" value="旅游，电影"/>
      >      ```
      >
      >   2. List<Object> List中的元素是对象类型
      >      方式一：添加子标签<list> ，在list标签中添加bean标签，即为集合元素
      >
      >      ```xml
      >      <property name="hobbies">
      >          <list>
      >              <bean class="com.feng.ioc.bean.Book">
      >              	<property name="name" value="圆圈正义"/>
      >                  <property name="author" value="罗翔"/>
      >              </bean>
      >          </list>
      >      </property>
      >      ```
      >
      >      方式二：先定义集合元素，然后在list标签中添加<ref>子标签引入
      >
      >      ```xml
      >      <bean id="book01" class="com.feng.ioc.bean.Book">
      >          <property name="name" value="圆圈正义"/>
      >          <property name="author" value="罗翔"/>
      >      </bean>
      >                              
      >      <property name="hobbies">
      >          <list>
      >              <ref bean="book01"></ref> <!--引入容器中的bean-->
      >          </list>
      >      </property>
      >      ```
      >
      > - ==Set==
      >
      >   1. 注入方式和List相同
      >
      >   2. 将list标签换成set标签即可
      >
      >   3. 例如：
      >
      >      ```xml
      >      <property name="hobbies">
      >          <set>
      >              <!--和list元素注入方式相同-->
      >          </set>
      >      </property>
      >      ```
      >
      > - ==Map==
      >
      >   1. 使用map标签中的entry（节点，键值对）标签中的 key标签 和 value标签
      >
      >      ```xml
      >      <property name="book">
      >          <map>
      >          	<entry>
      >         			<key>
      >          			<value></value> <!--在value标签中添加键key的值-->
      >          		</key>
      >          		<value></value> <!--在value标签中添加键值value的值-->
      >          	</entry>
      >          </map>
      >      </property>
      >      ```
      >
      >   2. 例子：
      >
      >      ```xml
      >      <property name="book">
      >          <map>
      >          	<entry>
      >         			<key>
      >          			<value>圆圈正义</value>
      >          		</key>
      >          		<value>罗翔</value>
      >          	</entry>
      >          </map>
      >      </property>
      >      ```
      >
      > - ==Properties==
      >
      >   1. 使用props标签中的prop标签
      >
      >   2. 因为规定Properties集合中的key和value都是String类型，所以prop标签的key属性的值就是Properties集合中的键key的值
      >
      >   3. 示例：
      >
      >      ```xml
      >      <property name="book">
      >          <props>
      >          	<prop key="圆圈正义">罗翔</prop>
      >              <prop key="活着">余华</prop>
      >          </props>
      >      </property>
      >      ```

###### 1.4.3 构造器注入

1. 解释：

   在bean标签中通过配置**constructor-arg标签**给属性赋值，Spring底层实际上就是**通过反射调用有参构造器**完成注入
   当类中没有声明无参构造器时，使用构造器注入就必须给属性赋值

2. 各种属性类型的注入：

   1. > **基本数据类型、字符串、自定义类对象：**
      >
      > 配置constructor-arg标签，可以给标签index属性赋值，说明是第几个参数，否则必须按参数顺序
      >
      > ```xml
      > <bean id="date" class="java.util.Date"/>
      > <bean id="stu" class="com.feng.ioc.bean.Student">
      >     <constructor-arg index="0" value="1001"/>
      >     <constructor-arg index="2" value="女"/>
      >     <constructor-arg index="1" value="张三"/>
      >     <constructor-arg index="3" value="21"/>
      >     <constructor-arg index="4" value="50.5"/>
      >     <constructor-arg index="5" ref="date"/>
      >     <constructor-arg index="6">
      >         <bean class="com.feng.ioc.bean.Clazz">
      >             <property name="clazzNum" value="03"/>
      >         </bean>
      >     </constructor-arg>
      > </bean>
      > ```

   2. > **集合类型属性：**
      >
      > 和set注入方式逻辑一样，只不过把property标签换成constructor-arg标签
      >
      > 例如：Student类有参构造中参数依次为 List、Set、Map、Properties
      >
      > ```xml
      > <bean id="stu" class="com.feng.ioc.bean.Student">
      >     
      >     <!--List-->
      > <constructor-arg index="0">
      >     <list>
      >         <bean class="com.feng.ioc.bean.Book">
      >         	<property name="name" value="圆圈正义"/>
      >             <property name="author" value="罗翔"/>
      >         </bean>
      >     </list>
      > </constructor-arg>
      >     
      > 	<!--Set-->
      > <constructor-arg index="1">
      >     <set>
      >         <!--和list元素注入方式相同-->
      >     </set>
      > </constructor-arg>
      > 
      >     <!--Map-->
      > <constructor-arg index="2">
      >     <map>
      >     	<entry>
      >    			<key>
      >     			<value>圆圈正义</value>
      >     		</key>
      >     		<value>罗翔</value>
      >     	</entry>
      >     </map>
      > </constructor-arg>
      > 
      >     <!--properties-->
      > <constructor-arg index="3">
      >     <props>
      >     	<prop key="圆圈正义">罗翔</prop>
      >         <prop key="活着">余华</prop>
      >     </props>
      > </constructor-arg>
      > </bean>
      > ```

##### 1.5 Bean的作用域

解释：

> 在bean标签中可以通过scope属性指定对象的作用域
>
> - scope="singleton" 表示当前bean是单例模式（默认饿汉模式，Spring容器初始化阶段就会完成此对象的创建；当在bean标签中设置lazy-init="true"变为懒汉模式）
> - scope="prototype" 表示当前bean为非单例模式，每次通过Spring容器获取此bean对象时都会创建一个新的对象

例子：

```xml
<!--bean的作用域-->
<bean id="book1" class="com.feng.ioc.bean.Book"/> <!--饿汉模式-->
<bean id="book2" class="com.feng.ioc.bean.Book" lazy-init="true"/> <!--懒汉模式-->
<bean id="book3" class="com.feng.ioc.bean.Book" scope="prototype"/> <!--非单例模式-->
```

##### 1.6 Bean的声明周期方法

> 在bean标签中通过init-method属性指定当前bean的初始化方法，初始化方法在构造器执行之后执行，通过
> destroy-method属性指定当前bean的销毁方法，销毁方法在对象销毁之前执行

- Bean类

  ```java
  public class Book {
  
      private String bookName;
      private String author;
  
      public void init(){
          //初始化方法，当前类对象被创建时，进行一些资源准备工作
          bookName  = "《活着》";
          author = "余华";
          System.out.println("~~~~~~~~~init~~~~~~~~~~~");
      }
  
      public Book(){
          System.out.println("---book已创建---");
      }
  
      public void destroy(){
          //销毁方法，当Spring容器销毁对象时调用，进行一些资源释放的工作
          System.out.println("~~~~~~~~~destroy~~~~~~~~~~~");
      }
  }
  ```

- Spring配置文件

  ```xml
  <!--bean的声明周期方法-->
  <bean id="book4" class="com.feng.ioc.bean.Book" scope="prototype"
        init-method="init" destroy-method="destroy"/>
  ```

##### 1.7 自动装配

> 自动装配：Spring在实例化当前bean的时候从Spring容器中找到匹配的实例赋值给当前bean中的一个属性
>
> 使用：bean标签中的autowire属性，值为"byName" 或 "byType"
>
> 自动装配策略有两种：
>
> - byName：根据当前bean中属性的属性名在Spring容器中寻找匹配的对象，如果根据name找到了bean但是类型不一致时抛出异常
> - byType：根据当前bean中属性的类型在Spring容器中寻找匹配的对象，如果根据类型找到了多个bean则抛出异常

- byName

  ```XML
  <bean id="teacher" class="com.feng.ioc.bean.Teacher"/>
  <bean id="act" class="com.feng.ioc.bean.Actor" autowire="byName"/>
  ```

- byType

  ```xml
  <bean id="clazz" class="com.feng.ioc.bean.Clazz"/>
  <bean id="student" class="com.feng.ioc.bean.Student" autowire="byType"/>
  ```

##### 1.8 IoC工作原理

| 工作原理图                                                   |
| ------------------------------------------------------------ |
| ![image-20220420204152886](..\image\image-20220420204152886.png) |

#### 2. 基于注解

> Spring除了提供基于XML的配置方式，同时提供了基于注解的配置：直接在实体类中添加注解声明给Spring容器管理，以简化开发步骤

##### 2.1 框架部署

1. 创建Maven工程	

   - Java
   - Web

2. 添加Spring IoC依赖

   ```xml
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-context</artifactId>
       <version>5.2.13.RELEASE</version>
   </dependency>
   ```

3. 创建Spring配置文件

   > 因为Spring容器初始化时，只会加载applicationContext.xml文件，那么在实体类中添加的注解就不会被Spring扫描，所以==需要在applicationContext.xml声明Spring的扫描范围==，以达到Spring初始化时扫描带有注解的实体类并完成初始化工作

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          http://www.springframework.org/schema/context/spring-context.xsd">
   
       <!--声明使用注解配置-->
       <context:annotation-config/>
   
       <!--声明Spring工厂注解的扫描范围-->
       <context:component-scan base-package="com.feng.beans"/>
   
   </beans>
   ```

##### 2.2 IoC常用注解

1. ==@Component==

   - 类注解，声明此类被Spring容器管理，相当于bean标签的作用
   - @Component(value="stu") value属性用于指定当前bean的id，相当于bean标签的id属性；value属性也可以省略，如果省略，当前类的id默认为类名首字母改小写
   - @Service、@Controller、@Repository 这三个注解也可以将声明给Spring管理，他们和@Component的区别主要是语义上的区别：
     - @Controller 注解主要声明 **控制器类** 配置给Spring管理，例如Servlet
     - @Service 注解主要声明 **业务处理类** 配置给Spring管理，例如Service接口的实现类
     - @Repository 注解主要声明 **持久化类** 配置给Spring管理，例如DAO接口
     - @Component 除了控制器、service和Dao**之外的类**一律使用此注解声明
   - **注解配置不存在注入问题**，属性值的初始化**直接在Java类中赋值**

2. ==@Scope==

   - 类注解，用于声明当前类是 单例模式 还是 非单例模式，相当于bean标签的scope属性
   - @Scope("prototype") 表示声明当前类为非单例模式（默认单例模式）

3. ==@Lazy==

   - 类注解，用于声明当前类是 懒汉模式 还是 饿汉模式，相当于bean标签的lazy-init属性
   - @Lazy(true) 表示声明为懒汉模式，默认为饿汉模式

4. ==@PostConstruct==

   - 方法注解，声明一个方法为当前类的初始化方法（在构造器调用后执行），相当于bean标签的init-method属性

5. ==@PreDestroy==

   - 方法注解，声明一个方法为当前类的销毁方法（在对象从容器中释放之前执行），相当于bean标签的destroy-method属性

6. ==@Autowired==

   - 属性注解、方法注解（set方法），声明一个属性自动装配，**默认byType**，默认必须（如果没有找到与属性类型匹配的bean则抛出异常），**没有byName**

   - @Autowired(required = false) 通过required属性设置当前自动装配是否为必须（默认必须——如果没有找到类型与属性类型匹配的bean则抛出异常），如果为false则没找到时为null，不抛异常

     - byType（默认）

     - ref引用

       在set方法参数中使用 @Qualifier("clz") 注解，表示找到 id 为clz的类

       ```java
       @Component(value = "clz")
       public class Clazz {
       	......
       }
       
       @Autowired
       public void setClazz(@Qualifier("clz") Clazz clazz) {
           this.clazz = clazz;
       }
       ```

7. ==@Resource==

   - 属性注解，也用于声明属性自动装配
   - 默认装配方式为byName，如果根据byName没有找到对应的bean，则继续根据byType寻找对应的bean，根据byType如果依然没找到或找到多个类型匹配的bean，则抛出异常

### 三、AOP面向切面编程

#### 1. 代理模式

##### 1.1 静态代理

> 静态代理，代理类只能够为特定的类生产代理对象，不能代理任意类

![image-20220422163116265](..\image\image-20220422163116265.png)

**使用代理的好处：**

1. 被代理类中只用关注核心业务的实现，将通用的管理型逻辑（事务管理、日志管理）和业务逻辑分离

2. 将通用的代码放在代理类中实现，提供了代码的复用性

3. 通过在代理类添加业务逻辑，实现对原有业务逻辑的扩展（增强）

##### 1.2 动态代理

> 动态代理，几乎可以为所有的类产生代理对象
>
> 动态代理的实现方式有2种：
>
> - JDK动态代理
> - CGLib动态代理

- JDK动态代理实现：

  > 由于JDK动态代理是通过被代理类实现的接口来构建代理对象的，因此JDK动态代理只能代理实现了接口的类对象。如果一个类没有实现任何接口，就不能使用JDK动态代理

  代理类

  ```java
  /**
   * JDK动态代理，是通过 被代理对象实现的接口 来产生 其代理对象
   * 1.创建一个类，实现InvocationHandler接口，重写invoke方法
   * 2.在类中定义一个Object类型的变量，并提供它的有参构造器，用于将被代理对象传递进来
   * 3.定义getProxy方法，用于创建并返回代理对象
   */
  public class JDKDynamicProxy implements InvocationHandler {
  
      //被代理对象
      Object proxiedObj;
  
      public JDKDynamicProxy(Object proxiedObj){
          this.proxiedObj = proxiedObj;
      }
  
      //构建代理对象，并返回代理对象
      public Object getProxy(){
          //1. 获取被代理对象的类加载器
          ClassLoader classLoader = proxiedObj.getClass().getClassLoader();
          //2. 获取被代理对象实现的接口
          Class<?>[] interfaces = proxiedObj.getClass().getInterfaces();
          //3. 生产代理对象（通过被代理对象的 类加载器 和 所实现的接口）
          //第一个参数：被代理对象的类加载器
          //第二个参数：被代理对象实现的接口
          //第三个参数：使用代理对象调用方法时，用于拦截方法执行的处理器
          Object proxy = Proxy.newProxyInstance(classLoader, interfaces, this);
          return proxy;
      }
  
      @Override
      public Object invoke(Object proxy, Method method, Object[] args) throws Throwable{
          begin();
          Object returnValue = method.invoke(proxiedObj); //执行method方法
          commit();
          return returnValue;
      }
  
      public void begin(){
          System.out.println("开启事务...");
      }
  
      public void commit(){
          System.out.println("提交事务...");
      }
  
  }
  ```

  测试

  ```java
  public class JDKDynamicProxyTest {
      public static void main(String[] args) {
  
          JDKDynamicProxy jdkDynamicProxy1 = new JDKDynamicProxy(new BookDaoImpl());
          BookDao proxy1 = (BookDao) jdkDynamicProxy1.getProxy();
          proxy1.insert();
  
          JDKDynamicProxy jdkDynamicProxy2 = new JDKDynamicProxy(new StudentDaoImpl());
          StudentDao proxy2 = (StudentDao) jdkDynamicProxy2.getProxy();
          proxy2.insert();
  
      }
  }
  ```

- CGLib动态代理实现：

  > 如果一个类没有实现任何接口，该如何产生代理对象呢？
  >
  > CGLib动态代理，是通过创建被代理类的子类来创建代理对象的，因此即使没有实现任何接口的类也可以通过CGLib产生代理对象
  >
  > CGLib动态代理，不能为final类创建代理对象
  
  添加依赖
  
  ```xml
  <dependency>
      <groupId>cglib</groupId>
      <artifactId>cglib</artifactId>
      <version>3.3.0</version>
  </dependency>
  ```
  
  代理类
  
  ```java
  /**
   * 1.添加cglib依赖
   * 2.创建一个类，实现MethodInterceptor接口，同时实现接口中的intercept方法
   * 3.在类中定义一个Object类型的变量，并提供这个变量的有参构造，用于传递被代理对象
   * 4.定义getProxy方法创建并返回代理对象（代理对象是通过创建被代理类的子类来创建的）
   */
  public class CGLibDynamicProxy implements MethodInterceptor {
  
      //被代理对象
      private Object proxiedObj;
  
      public CGLibDynamicProxy(Object proxiedObj){
          this.proxiedObj = proxiedObj;
      }
  
      //构建代理对象，并返回代理对象
      public Object getProxy(){
          Enhancer enhancer = new Enhancer();
          enhancer.setSuperclass(proxiedObj.getClass());
          enhancer.setCallback(this);
          Object proxy = enhancer.create();
          return proxy;
      }
  
      @Override
      public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
          begin();
          //通过反射调用被代理类的方法
          Object returnValue = method.invoke(proxiedObj,objects);
          commit();
          return returnValue;
      }
  
      public void begin(){
          System.out.println("开启事务...");
      }
  
      public void commit(){
          System.out.println("提交事务...");
      }
  
  }
  ```
  
  测试
  
  ```java
  public class CGLibDynamicProxyTest {
      public static void main(String[] args) {
  
          //创建被代理对象
          StudentDao studentDao = new StudentDao();
          BookDao bookDao = new BookDao();
          
          //通过cglib动态代理类创建代理对象
          CGLibDynamicProxy cgLibDynamicProxy1 = new CGLibDynamicProxy(studentDao);
          //代理对象实际上是被代理对象的子类对象，因此代理对象可直接强转为被代理类型
          StudentDao proxy1 = (StudentDao) cgLibDynamicProxy1.getProxy();
          //使用对象调用方法，实际上并没有执行这个方法，而是执行了代理类中的intercept方法，		   		   //将当前调用的方法以及方法中的参数传递到intercept方法中
          proxy1.insert();
  
          CGLibDynamicProxy cgLibDynamicProxy2 = new CGLibDynamicProxy(bookDao);
          BookDao proxy2 = (BookDao) cgLibDynamicProxy2.getProxy();
          proxy2.delete();
  
      }
  }
  ```

#### 2. AOP介绍

> Aspect Oriented Programming：面向切面编程，是一种利用“横切”的技术（底层实现就是动态代理），对原有的业务逻辑进行拦截，并且可以在这个拦截的横切面上添加特定的业务逻辑，对原有的业务进行增强。
>
> 基于动态代理实现，在不改变原有业务的情况下，对业务逻辑进行增强

![image-20220423165759749](..\image\image-20220423165759749.png)

> 当程序执行到切入点时，被Spring拦截下来，原方法不再通过对象调用，而通过Spring为此生成的代理对象进行调用。因此，AOP的底层就是动态代理
>
> 要使用aop面向切面编程，调用切入点方法的对象必须通过Spring容器获取，不能使用关键字new

#### 3. 基于XML

##### 3.1 框架部署

1. 创建Maven项目

2. 添加依赖

   - context
   - aspects

   ```xml
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-context</artifactId>
       <version>5.2.13.RELEASE</version>
   </dependency>
   
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-aspects</artifactId>
       <version>5.2.13.RELEASE</version>
   </dependency>
   ```

3. 创建Spring配置文件

   > 通过配置文件，“告诉”Spring容器，将切面类中的哪些方法，添加到切点中的哪个位置

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/aop
          http://www.springframework.org/schema/aop/spring-aop.xsd">
       
   </beans>
   ```

##### 3.2 AOP的使用

> 例：在Dao的方法添加开启事务和提交事务的逻辑

1.创建切面类，定义要添加的业务逻辑

```java
public class txManager {

    public void begin(){
        System.out.println("~~~~~~开启事务~~~~~~");
    }

    public void commit(){
        System.out.println("~~~~~~提交事务~~~~~~");
    }

}
```

2.配置aop

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">

    <bean id="bookDao" class="com.feng.dao.BookDao"/>
    <bean id="txManager" class="com.feng.utils.txManager"/>

    <aop:config>
        <!--声明切入点-->
        <aop:pointcut id="book_update" 
                      expression="execution(* com.feng.dao.BookDao.update())"/>

        <!--声明txManager为切面类-->
        <aop:aspect ref="txManager">
            <!--通知-->
            <aop:before method="begin" pointcut-ref="book_update"/>
            <aop:after method="commit" pointcut-ref="book_update"/>
        </aop:aspect>
    </aop:config>

</beans>
```

##### 3.3 切入点的声明

各种切入点声明方式

> <aop:pointcut id=" 切入点唯一标识 " expression="execution(返回值  全类名.方法名(参数类型) )"/>

* " * " 代表所有
* "()" 代表无参
* " (..) " 代表任何参数（包括无参）

| 含义                                       | 例子                                                         |
| ------------------------------------------ | ------------------------------------------------------------ |
| BookDao中所有方法                          | <aop:pointcut id="cp1" expression="execution(* com.feng.dao.BookDao.*(..))"/> |
| BookDao中所有无参方法                      | <aop:pointcut id="cp1" expression="execution(* com.feng.dao.BookDao.*())"/> |
| BookDao中所有无返回值方法                  | <aop:pointcut id="cp1" expression="execution(void com.feng.dao.BookDao.*(..))"/> |
| dao包类中所有方法                          | <aop:pointcut id="cp1" expression="execution(* com.feng.dao.* .*(..))"/> |
| BookDao中所有参数为int类型的deleteById方法 | <aop:pointcut id="book_delete" expression="execution(* com.feng.dao.BookDao.deleteById(int))"/> |

##### 3.4 通知策略

> AOP通知策略：就是声明将切面类中的切点方法如何植入到切入点

五种通知策略：

```xml
<!--声明MyAspect为切面类-->
<aop:aspect ref="myAspect">
    <!--aop:before 前置通知，切入到指定切入点之前-->
    <aop:before method="method1" pointcut-ref="book_insert"/>

    <!--aop:after 后置通知，切入到指定切入点之后-->
    <aop:after method="method2" pointcut-ref="book_insert"/>

    <!--aop:after-throwing 异常通知，切入到抛出异常之后-->
    <aop:after-throwing method="method3" pointcut-ref="book_insert"/>

    <!--aop:after-returning 返回通知，切入到返回值返回之后
                            和后置通知是同一时间点，谁配置在前面，谁的方法先执行
    -->
    <aop:after-returning method="method4" pointcut-ref="book_insert"/>

    <!--aop:around 环绕通知，切点方法中的一部分切入到切入点前，另一部分切入到切入点后
                   采取环绕通知的切点方法，要遵守定义规则进行定义
    -->
    <aop:around method="method5" pointcut-ref="book_insert"/>
</aop:aspect>
```

环绕通知方法定义规则：

```java
//环绕通知的切点方法，必须遵守如下的定义规则：
//1.必须带有一个ProceedingJoinPoint类型的参数
//2.必须有Object类型的返回值
//3.在前后增强的业务逻辑之间执行 Object v = point.proceed();
//4.方法最后返回v
public Object method5(ProceedingJoinPoint point) throws Throwable {
    System.out.println("method5-before...");
    Object v = point.proceed();
    System.out.println("method5-after...");
    return v;
}
```

#### 4. 基于注解

##### 4.1 框架部署

1. 添加依赖，和基于XML方式一致

   ```xml
   <dependencies>
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-context</artifactId>
           <version>5.2.13.RELEASE</version>
       </dependency>
   
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-aspects</artifactId>
           <version>5.2.13.RELEASE</version>
       </dependency>
   </dependencies>
   ```

2. 创建Spring配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          http://www.springframework.org/schema/context/spring-context.xsd
          http://www.springframework.org/schema/aop
          http://www.springframework.org/schema/aop/spring-aop.xsd">
   
       <!--声明使用注解配置-->
       <context:annotation-config/>
   
       <!--声明Spring工厂注解的扫描范围-->
       <context:component-scan base-package="com.feng"/>
   
       <!--基于注解配置的aop代理-->
       <aop:aspectj-autoproxy/>
   
   </beans>
   ```

##### 4.2 AOP常用注解

1. ==@Aspect==
   - 类注解，表示一个类为切面类
   - @Aspect(value = "") value属性用于指定当前bean的id，相当于bean标签的id属性；value属性也可以省略，如果省略，当前类的id默认为类名首字母改小写
2. ==@Pointcut==
   - 方法注解，表示一个方法为切入点
   - @Pointcut("execution(* com.feng.dao.*.*(..))")
      public void pc1(){}
3. ==@Before==
   - 方法注解，定义方法为切点方法，前置通知
   - @Before("execution(* com.feng.dao.*.*(..))") 
   - 也可以 @Before("pc1()") 
4. ==@After==
   - 方法注解，定义方法为切点方法，后置通知
   - @After("pc1()")
5. ==@AfterThrowing==
   - 方法注解，定义方法为切点方法，异常通知
   - @AfterThrowing("pc1()")
6. ==@AfterReturning==
   - 方法注解，定义方法为切点方法，返回通知
   - @AfterReturning("pc1()")
7. ==@Around==
   - 方法注解，定义方法为切点方法，环绕通知
   - @Around("pc1()")

**注意：**注解使用虽然方便，但是只能在代码上添加注解，因此我们自定义的类提倡使用注解配置；
但是如果使用到第三方提供的类则需要通过xml配置形式完成

### 四、整合MyBatis

> Spring两大核心思想：IoC和AOP
>
> IoC：控制反转，Spring容器可以完成对象的创建、属性注入、对象管理等工作
>
> AOP：面向切面，在不修改原有业务逻辑的情况下，实现原有业务的增强

#### 1. Spring提供的支持

**1.1 Spring IoC的支持：**

> MyBatis需要创建的对象：
>
> - 需要创建数据源DataSource对象（例如：DruidDataSource）
> - 需要创建SqlSessionFactory对象
> - 需要创建SqlSession对象
> - 需要创建Mapper对象
>
> Spring IoC可以为MyBatis完成以上对象的创建和管理

**1.2 Spring AOP的支持：**

> 使用Spring提供的**事务管理切面类**，完成对MyBatis数据库操作中的事务管理

#### 2. 框架部署

##### 2.1 部署MyBatis

1. 添加依赖

   ```xml
   <!--MyBatis-->
   <dependency>
       <groupId>org.mybatis</groupId>
       <artifactId>mybatis</artifactId>
       <version>3.5.5</version>
   </dependency>
   
   <!--mysql驱动-->
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>5.1.37</version>
   </dependency>
   ```

2. 创建mybatis-config.xml核心配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
   
   </configuration>
   ```

##### 2.2 部署Spring

1. 添加依赖

   ```xml
   <!--Spring-->
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-context</artifactId>
       <version>5.2.13.RELEASE</version>
   </dependency>
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-aspects</artifactId>
       <version>5.2.13.RELEASE</version>
   </dependency>
   <dependency><!--spring对jdbc的支持-->
       <groupId>org.springframework</groupId>
       <artifactId>spring-jdbc</artifactId>
       <version>5.2.13.RELEASE</version>
   </dependency>
   ```

2. 创建applicationContext.xml核心配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/aop
          http://www.springframework.org/schema/aop/spring-aop.xsd">
   
   </beans>
   ```

##### 2.3 添加整合依赖

Spring整合MyBatis的依赖：

mybatis-spring （就是Mybatis提供的兼容Spring的补丁）

```xml
<!--Mybatis兼容Spring的补丁（整合依赖）-->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>2.0.6</version>
</dependency>
```

#### 3. 整合IoC配置

##### 3.1 **整合Druid连接池**

- 添加druid依赖

  ```xml
  <!--Druid连接池-->
  <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.2.4</version>
  </dependency>
  ```

- 创建druid.properties属性文件

  > init：初始化时建立物理连接的个数
  >
  > minIdle：最小连接池数量
  >
  > maxActive：最大连接池数量
  >
  > maxWait：等待获取连接时间（ms）

  ```properties
  ## 数据源参数
  druid.driver=com.mysql.jdbc.Driver
  druid.url=jdbc:mysql://localhost:3306/mybatis
  druid.username=root
  druid.password=Socialbigfeng321
  
  ## 连接池参数
  druid.pool.init=1
  druid.pool.minIdle=3
  druid.pool.maxActive=20
  druid.pool.maxWait=30000
  ```

- 在applicationContext.xml中配置DruidDataSource

  ```xml
  <!--加载druid.properties配置文件-->
  <context:property-placeholder location="classpath:druid.properties"/>
  
  <!--依赖Spring容器完成数据源DataSource的创建-->
  <bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
      <property name="driverClassName" value="${druid.driver}"/>
      <property name="url" value="${druid.url}"/>
      <property name="username" value="${druid.username}"/>
      <property name="password" value="${druid.password}"/>
  
      <property name="initialSize" value="${druid.pool.init}"/>
      <property name="minIdle" value="${druid.pool.minIdle}"/>
      <property name="maxActive" value="${druid.pool.maxActive}"/>
      <property name="maxWait" value="${druid.pool.maxWait}"/>
  </bean>
  ```

##### 3.2 SqlSessionFactory

> 依赖Spring容器创建MyBatis的SqlSessionFactory对象

```xml
<!--依赖Spring容器完成MyBatis的SqlSessionFactory对象的创建-->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <!--配置数据源-->
    <property name="dataSource" ref="druidDataSource"/>
    <!--配置mapper文件路径-->
    <property name="mapperLocations" value="classpath:com/feng/mapper/*Mapper.xml"/>
    <!--配置需要定义别名的POJO-->
    <property name="typeAliasesPackage" value="com.feng.pojo"/>
    <!--可选：配置MyBatis的主配置文件（mybatis-config.xml中可能存在额外配置）-->
    <property name="configLocation" value="classpath:mybatis-config.xml"/>
</bean>
```

##### 3.3 整合配置Mapper

> 配置MapperScannerConfigurer类，**创建所有的Mapper接口对象**，并存储到Spring容器

applicationContext：

```xml
<!--
    加载mapper包中的所有Mapper，通过sqlSessionFactory获取sqlSession，
    通过sqlSession创建所有的Mapper接口对象，并存储到Spring容器
-->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <!--配置sqlSessionFactory-->
    <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
    <!--配置mapper接口-->
    <property name="basePackage" value="com.feng.mapper"/>
</bean>
```

测试：

> 直接通过context获取Mapper，不需要用new关键字，**注意Mapper类id默认首字母小写**

```java
public class UserMapperTest extends TestCase {
    public void testQueryUsers() {

        ClassPathXmlApplicationContext context =
                new ClassPathXmlApplicationContext("applicationContext.xml");

        //获取Mapper，注意，Mapper的id默认是首字母小写，除非进行：@Component注解配置
        UserMapper userMapper = (UserMapper) context.getBean("userMapper");

        List<User> users = userMapper.queryUsers();
        System.out.println(users);
    }
}
```

##### 3.4 IoC整合配置示例

> 以编写service层中的ServiceImpl为例：
>
> - serviceImpl中的mapper对象不再需要通过new关键字获取
> - 只需要使用@Resource注解，完成自动装配即可，Spring容器已经帮助创建了mapper对象

使用注解声明：

```xml
!--声明使用注解配置-->
<context:annotation-config/>

<!--声明Spring工厂注解的扫描范围-->
<context:component-scan base-package="com.feng"/>
```

ServiceImpl编写：

```java
@Service
public class UserServiceImpl implements UserService {

    @Resource//自动装配，当Spring容器创建UserMapper对象时，自动装配给userMapper
    private UserMapper userMapper;

    /**
     * 查询全部用户信息服务
     * @return List<User>
     */
    public List<User> listUsers() {
        return userMapper.queryUsers();
    }
}
```

测试：

```java
public class UserServiceTest extends TestCase {

    public void testListUsers() {

        ClassPathXmlApplicationContext context =
                new ClassPathXmlApplicationContext("applicationContext.xml");

        UserService userService = (UserServiceImpl) context.getBean("userServiceImpl");
        List<User> users = userService.listUsers();
        System.out.println(users);
    }
}
```

#### 4. 整合AOP事务管理

> 使用Spring提供的事务管理切面类 完成Mapper中增删改操作的事务管理

##### 4.1 示例

```xml
<!--1.将Spring提供的 事务管理切面类对象 交给Spring容器管理-->
<bean id="transactionManager"
      class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <!--设置数据源-->
    <property name="dataSource" ref="druidDataSource"/>
</bean>

<!--2.通过Spring jdbc提供的 tx标签，声明事务管理策略-->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <tx:attributes>
        <!--isolation：事务隔离级别  propagation：事务传播机制-->
        <tx:method name="insert*" isolation="REPEATABLE_READ" propagation="REQUIRED"/>
        <tx:method name="query*" isolation="REPEATABLE_READ" propagation="SUPPORTS"/>
    </tx:attributes>
</tx:advice>
```

##### 4.2 事务隔离级别

> - SERIALIZABLE 串行化：
>
>   T1在执行过程中，T2既不能读也不能写
>
> - REPEATABLE_READ 可重复读：
>
>   T1在执行过程中，T2只能读但不能改，T2可以添加数据
>   （可能造成：幻读）
>
> - READ_COMMITTED 读已提交：
>
>   T1在执行过程中，T2可以读也可以写，但是T1只能读取到T2提交后的数据
>   （可能造成：不可重复读、幻读）
>
> - READ_UNCOMMITTED 读未提交：
>
>   T1在执行过程中，T2可以读也可以写，T1可以读取到T2未提交的数据
>   （可能造成：脏读、不可重复读、幻读）

==数据库操作百分之九十都是查询操作，综合安全性和效率，大多数情况使用 可重复读REPEATABLE_READ==

##### 4.3 事务传播机制

> 当一个数据库操作（方法），包含另一个操作（方法）的调用时：
>
> - **REQUIRED**：
>   如果上层方法没有事务，则创建一个新的事务；如果已经存在事务，则加入到事务中。
> - **SUPPORTS**：
>   如果上层方法没有事务，则以非事务方式执行；如果已经存在事务，则加入到事务中。
> - REQUIRES_NEW：
>   如果上层方法没有事务，则创建一个新的事务；如果已经存在事务，则将当前事务挂起。
> - NOT_SUPPORTED：
>   如果上层方法没有事务，则以非事务方式执行；如果已经存在事务，则将当前事务挂起。（不支持事务）
> - NEVER：
>   如果上层方法没有事务，则以非事务方式执行；如果已经存在事务，则抛出异常。（不支持事务）
> - MANDATORY：
>   如果上层方法已经存在事务，则加入到事务中执行；如果不存在事务则抛出异常。
> - NESTED：
>   如果上层方法没有事务，则创建一个新的事务；如果已经存在事务，则嵌套到当前事务中。

==增删改一般用REQUIRED，查询一般用SUPPORTS；根据业务再做定夺==

##### 4.4 AOP事务配置(xml)

> 三大步骤：
>
> - 第一步：将事务管理切面类交给Spring容器管理
> - 第二步：声明事务管理策略
> - 第三步：将事务管理策略以AOP配置应用于service操作方法（一个service包含多个mapper操作）

```xml
<!--1.将Spring提供的 事务管理切面类对象 交给Spring容器管理-->
<bean id="transactionManager"
      class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <!--设置数据源-->
    <property name="dataSource" ref="druidDataSource"/>
</bean>

<!--2.通过Spring jdbc提供的 tx标签，声明事务管理策略-->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <tx:attributes>
        <!--isolation：事务隔离级别  propagation：事务传播机制-->
        <tx:method name="insert*" isolation="REPEATABLE_READ" propagation="REQUIRED"/>
        <tx:method name="query*" isolation="REPEATABLE_READ" propagation="SUPPORTS"/>
    </tx:attributes>
</tx:advice>

<!--3.切点定义，策略引入-->
<aop:config>
    <aop:pointcut id="crud" expression="execution(* com.feng.service.*.*(..))"/>
    <aop:advisor advice-ref="txAdvice" pointcut-ref="crud"/>
</aop:config>
```

##### 4.5 AOP事务配置(注解)

> 使用tx标签声明事务管理策略时，可能方法较多，配置比较麻烦；使用注解配置比较便捷

在applicationContext.xml中声明：

```xml
<!--AOP事务管理配置(基于注解)：-->
<!--声明使用注解完成事务配置-->
<tx:annotation-driven transaction-manager="transactionManager"/>
```

==@Transactional==注解：

@Transactional(isolation =  ,propagation = )

方法注解，直接在service方法上配置，表示该方法由AOP事务管理配置

示例：

```java
@Service
public class UserServiceImpl implements UserService {

    @Resource//自动装配，当Spring容器创建UserMapper对象时，自动装配给userMapper
    private UserMapper userMapper;

    /**
     * 服务：
     * 添加一个用户，并返回全部用户信息
     * @return List<User>
     */
    @Transactional(isolation = Isolation.REPEATABLE_READ ,
                   propagation = Propagation.REQUIRED )
    public List<User> insertAndListUsers(User user){
        User query = userMapper.queryByName(user.getUserName());
        if(query == null){
            userMapper.insertUser(user);
        }
        return userMapper.queryUsers();
    }
}
```

### 五、笔记与代码解释

> 对应关系：
>
> - IoC相关笔记对应代码：
>   - 基于xml：Spring-IoC-xml
>   - 基于注解：Spring-IoC-annotation
> - AOP相关笔记对应代码：
>   - 代理模式：ProxyPattern
>   - 基于xml：Spring-AOP-xml
>   - 基于注解：Spring-AOP-annotation
> - 第四章整合MyBatis对应代码：Spring-MyBatis

