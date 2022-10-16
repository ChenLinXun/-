# SSM整合

### 一、Maven依赖

> SSM项目需要的主要Maven依赖：
>
> - 开发阶段运行项目的jsp、servlet的支持
> - mybatis相关依赖
> - spring相关依赖（包含springmvc）
>
> 开发中扩展的依赖：
>
> - 开发工具
> - 日志工具

```xml
<!--spring依赖版本，统一管理-->
<properties>
    <spring.version>5.2.13.RELEASE</spring.version>
</properties>

<dependencies>

    <!--jsp-->
    <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>jsp-api</artifactId>
        <version>2.2</version>
        <scope>provided</scope>
    </dependency>
    <!--servlet-->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>4.0.1</version>
        <scope>provided</scope>
    </dependency>


    <!--MyBatis以及与Spring整合依赖-->
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
    <!--Mybatis兼容Spring的补丁（整合依赖）-->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.6</version>
    </dependency>
    <!--Druid连接池-->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.2.4</version>
    </dependency>


    <!--spring依赖-->
    <!--context-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <!--aspects-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <!--jdbc-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <!--web-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <!--webmvc-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <!--test-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>${spring.version}</version>
    </dependency>


    <!--开发工具-->
    <!--Junit-->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
    <!--lombok-->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.10</version>
        <scope>provided</scope>
    </dependency>
    <!--jackson-->
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.12.1</version>
    </dependency>
    <!--文件上传-->
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
    <!--pagehelper分页插件-->
    <dependency>
        <groupId>com.github.pagehelper</groupId>
        <artifactId>pagehelper</artifactId>
        <version>5.3.0</version>
    </dependency>

    <!--日志工具-->
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

### 二、配置文件

> Spring的配置文件：（多配置文件分开配置，分工合作，全写到一起太乱不好管理）
>
> - spring-context.xml	只配置注解声明，以及类的管理（干IOC的活）
> - spring-mvc.xml    进行mvc相关配置，例如：静态资源放行、拦截器配置等
> - spring-mybatis.xml    进行spring与mybatis整合相关的配置，例如：数据源、事务管理（AOP）等 
>
> mybatis的配置文件：
>
> - mybatis-config.xml    只进行一些额外的配置，例如：分页插件（其他的配置都交给spring管理）
>
> web.xml的配置：
>
> - web.xml   配置springmvc中央处理器（必须）、统一编码等

#### 1. spring-context.xml

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
    <context:component-scan base-package="com.feng"/>

</beans>
```

#### 2. spring-mvc.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--声明mvc使用注解驱动 并配置转换器工厂-->
    <mvc:annotation-driven conversion-service="converterFactory"/>

    <!--静态资源放行配置-->
    <!--
    <mvc:resources mapping="/css/**" location="/css/"/>
    <mvc:resources mapping="/imgs/**" location="/imgs/"/>
    <mvc:resources mapping="/js/**" location="/js/"/>
    <mvc:resources mapping="/pages/**" location="/pages/"/>
    -->

    <!--配置自定义日期转换器-->
    <bean id="converterFactory"
          class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
        <property name="converters">
            <set>
                <bean class="com.feng.utils.MyDateConverter"/>
            </set>
        </property>
    </bean>

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

    <!--配置拦截器（链）-->
    <!--
    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/book/query"/>
            <bean class="com.feng.utils.MyInterceptor1"/>
        </mvc:interceptor>
        <mvc:interceptor>
            <mvc:mapping path="/book/query"/>
            <bean class="com.feng.utils.MyInterceptor2"/>
        </mvc:interceptor>
    </mvc:interceptors>
    -->

</beans>
```

#### 3. spring-mybatis.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd
       http://www.springframework.org/schema/tx
       http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!--IoC整合MyBatis：-->
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


    <!--AOP事务管理配置(基于注解)：-->
    <!--声明使用注解完成事务配置-->
    <tx:annotation-driven transaction-manager="transactionManager"/>

    <!--AOP事务管理配置(基于xml)：-->
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
            <tx:method name="" isolation="" propagation=""/>
            <tx:method name="" isolation="" propagation=""/>
        </tx:attributes>
    </tx:advice>

    <!--3.切点定义，策略引入-->
    <aop:config>
        <aop:pointcut id="crud" expression="execution()"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="crud"/>
    </aop:config>

</beans>
```

#### 4. mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--类型别名、数据源和连接池、映射文件加载，都交给Spring配置-->

    <!--额外的配置，就仍在mybatis-config.xml中配置-->
    <!--plugins标签：用于配置MyBatis插件（例如：分页插件）-->
    <plugins>
        <plugin interceptor="com.github.pagehelper.PageInterceptor"/>
    </plugins>

</configuration>
```

#### 5. web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--配置SpringMVC前端控制器（中央控制器）-->
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--加载Servlet初始化参数：加载配置文件-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-*.xml</param-value>
        </init-param>
        <!--tomcat服务器初始化时创建该servlet实例-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>SpringMVC</servlet-name>
        <!--DispatcherServlet拦截的资源路径-->
        <url-pattern>/</url-pattern>
    </servlet-mapping>

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

</web-app>
```































