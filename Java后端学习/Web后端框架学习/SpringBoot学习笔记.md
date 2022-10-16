# SpringBoot学习笔记

### 一、SpringBoot入门

#### 1.1 SpringBoot简介

> SpringBoot是Spring的再封装，用来简化Spring应用开发，约定大于配置，去繁从简，just run就能创建一个独立的，产品级别的应用

- **背景**：J2EE笨重的开发、繁多的配置、低下的开发效率、复杂的部署流程、第三方技术集成难度大。

- **解决**：
  - 简化Spring应用的开发
  
  - 整个Spring技术栈的一个大整合，”Spring全家桶“时代。
  - SpringBoot -> J2EE一站式解决方案
  - SpringCloud -> 分布式整体解决方案
  
- **优点**：

  - 快速创建独立运行的Spring项目以及主流框架集成
  - 使用嵌入式Servlet容器，应用无需打成War包
  - starters自动依赖于版本控制
  - 大量的自动配置，简化开发，也可以修改默认值
  - 无需配置XML，无代码生成，开箱即用
  - 准生产环境的运行时应用监控
  - 与云计算的天然集成

- 缺点：

  - 上手容易，精通难（需要精通Spring原理）

#### 1.2 微服务简介

> 微服务：一种架构风格
>
> - 一个应用应该是一组小型服务；可以通过HTTP的方式进行互通；
> - 一个微服务架构把每个功能元素放进一个独立的服务中，并且通过跨服务器分发这些服务进行扩展，只有在需要时才复制
> - 每一个功能元素最终都是一个可独立替换。可独立升级的软件单元
>
> 单体应用：ALL IN ONE；
>
> - 一个单体应用程序把它所有的功能放在一个单一进程中，并且通过多个服务器复制这个单体进行扩展
> - “牵一发而动全身”
>
> 详细参照微服务文档：https://martinfowler.com/

![image-20220615164419497](..\image\image-20220615164419497.png)

#### 1.3 环境准备

- jdk1.8
- maven3.x
- IDEA 2017及以上
- Spring Boot （现已经到2版本，2.7.0）

#### 1.4 IDEA设置

> 将maven设置成本机环境的maven

![image-20220615171652756](..\image\image-20220615171652756.png)

#### 1.5 pom依赖

```xml
<groupId>com.movie</groupId>
<artifactId>demo</artifactId>
<version>0.0.1-SNAPSHOT</version>
<name>movie</name>
<description>Movie Demo project for Spring Boot</description>

<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.7.0</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>

<!--这个插件可以将应用打包成一个可执行的jar包-->
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

#### 1.6 编写主程序

```java
package com.movie;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * @SpringBootApplication 来标注一个主程序类，说明这是一个SpringBoot应用
 */
@SpringBootApplication
public class MovieApplication {

    public static void main(String[] args) {
        //将Spring应用启动起来
        SpringApplication.run(MovieApplication.class, args);
    }

}
```

### 二、SpringBoot配置

#### 2.1 依赖管理特性

- 父项目做依赖管理

> 依赖管理
>
> ```xml
> <parent>
>     <groupId>org.springframework.boot</groupId>
>     <artifactId>spring-boot-starter-parent</artifactId>
>     <version>2.3.4.RELEASE</version>
>     <relativePath/> <!-- lookup parent from repository -->
> </parent>
> ```
>
> 他的父项目：
>
> ```xml
> <parent>
>     <groupId>org.springframework.boot</groupId>
>     <artifactId>spring-boot-dependencies</artifactId>
>     <version>2.3.4.RELEASE</version>
> </parent>
> ```
>
> **几乎声明了所有开发中常用的依赖的版本号，自动版本仲裁机制**

- 开发导入starter场景启动器

> 1、见到很多 spring-boot-starter-* ：*就是某种场景
> 2、只要引入starter，这个场景的所有常规需要的依赖我们都自动引入
> 3、SpringBoot所有支持的场景：
> https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/html/using-spring-boot.html#using-boot-starter
> 4、见到的 *-spring-boot-starter：第三方为我们提供的简化开发的场景启动器
> 5、所有场景启动器最底层的依赖：
>
> ```xml
> <dependency>
>     <groupId>org.springframework.boot</groupId>
>     <artifactId>spring-boot-starter</artifactId>
>     <version>2.3.4.RELEASE</version>
>     <scope>compile</scope>
> </dependency>
> ```

- 无需关注版本号，自动版本仲裁

> 1、引入依赖默认都可以不写版本号
> 2、引入的非版本仲裁的jar，要写版本号

- 可以修改版本号

> 1、查看spring-boot-dependencies里面规定当前依赖的版本 用的key
> 2、在当前项目里面重写配置   
>
> ```xml
> <properties>
> 	<mysql.version>5.7.19</mysql.version>
> </properties>
> ```

#### 2.2 自动配置特性

- 自动配好Tomcat
  - 引入Tomcat依赖
  - 配置Tomcat

> 在spring-boot-starter-web中：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
    <version>2.3.4.RELEASE</version>
    <scope>compile</scope>
</dependency>
```

- 自动配好SpringMVC

  - 引入SpringMVC全套组件
  - 自动配好SpringMVC常用组件（功能）

- 自动配好Web常见功能，如：字符编码问题

  - SpringBoot帮我们配置好了所有web开发的常见场景

- 默认的包结构

  - **主程序所在包及其下面的所有子包里面的组件都会被默认扫描出来**

  - 无需以前的包扫描配置

  - 想要改变扫描路径，@SpringBootApplication(scanBasePackage="com.feng")

    - 或者@ComponentScan 指定扫描路

    ```java
    @SpringBootApplication(scanBasePackages = "com.movie")
    //等同于：
    @SpringBootConfiguration
    @EnableAutoConfiguration
    @ComponentScan("com.movie")
    ```

- 各种配置拥有默认值

  - 默认配置最终都是映射到MultipartProperties
  - 配置文件的值最终会绑定到某个类上，这个类会在容器中创建对象

- ==按需加载==所有自动配置项

  - 非常多的starter
  - **POM中引入了哪些场景，哪些场景的自动配置才会开启**
  - SpringBoot所有的自动配置功能都在 spring-boot-autoconfigure 包里面

- 等等...

### 三、容器功能

#### 3.1 组件添加

##### 1、@Configuration配置类

- 基本使用：

  ```java
  /**
   * 1. 配置类里面使用@Bean标注在方法上，给容器注册组件，默认也是单实例的
   * 2. 配置类本身也是组件
   * 3. proxyBeanMethods：代理bean的方法：
   *      Full(proxyBeanMethods = true)
   *          全模式，存在组件依赖时使用，确保方法调用得到的是之前在容器中注册的实例
   *      Lite(proxyBeanMethods = false)
   *          轻量级模式，外部调用配置类方法时不会检查容器中是否存在返回类型的实例，
   *			SpringBoot启动快；单单注册组件，没有组件依赖时使用
   */
  @Configuration(proxyBeanMethods = true) //告诉SpringBoot这是一个配置类 == 配置文件
  public class MyConfig {
  
      /**
       * 外部无论对配置类中的这个组件注册方法调用多少次，获取的都是容器中的单例对象
       */
      @Bean //给容器中添加组件。 方法名即为组件id，返回类型即为组件类型，返回值即为组件在容器中的实例
      public Person person01(){
          Person person = new Person("张三", 18);
          person.setPet(pet01()); //组件依赖
          return person;
      }
  
      @Bean
      public Pet pet01(){
          return new Pet("小柴");
      }
  }
  ```

  ```java
  @SpringBootApplication
  public class FuckingTestApplication {
      public static void main(String[] args) {
          ConfigurableApplicationContext run = SpringApplication.run(FuckingTestApplication.class, args);
  
          String[] names = run.getBeanDefinitionNames();
          for (String name : names) {
              System.out.println(name);
          }
  
          Person person01 = run.getBean("person01", Person.class);
          Person person02 = run.getBean("person01", Person.class);
          //无论获取多少次都是IOC容器中的那个单例
          System.out.println("两个person同一个：" + (person01 == person02));
  
          Pet pet1 = person02.getPet();
          Pet pet2 = run.getBean("tom", Pet.class);
          //如果 @Configuration(proxyBeanMethods = true) 则从user01中获取的就是IOC容器的那个单例pet，否则为false
          System.out.println("两个pet同一个：" + (pet1 == pet2));
      }
  }
  ```

- **Full模式与Lite模式**

  - 最佳实战：
    - 配置类组件之间无依赖关系用Lite模式加速容器启动过程，减少判断
    - 配置类组件之间有依赖关系用Full模式，方法会被调用得到之前的单实例组件

- 注册组件的注解：

  @Bean、@Component、@Controller、@Service、@Repository

##### 2、@Import导入组件

- 基本使用：

  ```java
  /**
   * 4. 用@Import：
   *      不用写配置类方法，自动在容器中创建组件，组件的id就是全类名
   */
  @Configuration(proxyBeanMethods = true) //告诉SpringBoot这是一个配置类 == 配置文件
  @Import(value = {Person.class, Pet.class, DBHelper.class})
  public class MyConfig {
  }
  ```

  ```java
  @SpringBootApplication
  public class FuckingTestApplication {
      public static void main(String[] args) {
          ConfigurableApplicationContext run = SpringApplication.run(FuckingTestApplication.class, args);
  
          //根据类型获取组件名
          String[] s = run.getBeanNamesForType(Person.class);
          for (String s1 : s) {
              System.out.println(s1);
          }
          DBHelper bean = run.getBean(DBHelper.class);
          System.out.println(bean);
      }
  }
  ```

##### 3、@Conditional条件装配

> 条件装配：满足Conditional指定的条件，则进行组件注入（**注意装配顺序，被依赖的组件放前面**）

各种条件装配：

![image-20220816164205142](..\image\image-20220816164205142.png)

- 用法：@ConditionalOnBean(name = "tom") 容器中有名字为 tom 的组件才注册组件

  ```java
  @Configuration(proxyBeanMethods = true) //告诉SpringBoot这是一个配置类 == 配置文件
  public class MyConfig {
  
      @Bean("tom")//给容器中添加组件。 方法名即为组件id，返回类型即为组件类型，返回值即为组件在容器中的实例
      public Pet pet01(){
          return new Pet("小柴");
      }
  
      //条件装配：容器中有名字为 tom 的组件才注册组件，注意装配顺序，被依赖的组件放前面
      @ConditionalOnBean(name = "tom")
      @Bean
      public Person person01(){
          Person person = new Person("张三", 18,new Date(),null);
          person.setPet(pet01()); //组件依赖
          return person;
      }
  }
  ```

##### 4、原生配置文件引入

> @ImportResource("classpath:bean.xml")

```java
@Configuration(proxyBeanMethods = true) //告诉SpringBoot这是一个配置类 == 配置文件
@ImportResource("classpath:bean.xml")
public class MyConfig {
}
```

##### 5、配置绑定

> @ConfigurationProperties
> 	==读取properties文件中的内容，封装到 JavaBean中，避免使用java代码过于麻烦==

两种方法：

- **@EnableConfigurationProperties + @ConfigurationProperties**：(一般注入第三方包时使用)

  ```java
  @Configuration(proxyBeanMethods = true) //告诉SpringBoot这是一个配置类 == 配置文件
  // 1. 开启Car配置绑定功能
  // 2. 把这个Car组件自动注册到容器中
  @EnableConfigurationProperties(Car.class)
  public class MyConfig {
      ... ...
  }
  ```

  ```java
  //@Component
  @ConfigurationProperties(prefix = "mycar")
  public class Car {
      private String name;
      private Integer price;
  	... ...
  }
  ```

- **@Component + @ConfigurationProperties**：

  ```java
  /**
   * 只有在容器中的组件，才会拥有SpringBoot提供的强大功能
   */
  @Component
  @ConfigurationProperties(prefix = "mycar")
  public class Car {
      private String name;
      private Integer price;
  	... ...
  }
  ```

### 四、自动配置原理入门

#### 4.1 引导加载自动配置类

@SpringBootApplication的合成注解：

```java
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
      @Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {}
```

##### 1、@SpringBootConfiguration

@Configuration ：代表当前是一个配置类

##### 2、@ComponentScan

指定扫描哪些东西

##### 3、@EnableAutoConfiguration

==由两个注解合成==：

```java
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {}
```

###### 1.@AutoConfigurationPackage

> 负责注册主程序所在包下的所有组件，也就是包扫描（指定默认包规则）：

```java
@Import(AutoConfigurationPackages.Registrar.class) //给容器中导入组件Registrar
public @interface AutoConfigurationPackage {}

//利用Registrar给容器中导入一系列组件
//将指定的一个包下的所有组件导入进来，也就是MainApplication所在包下
```

**Registrar注册组件源码:**

```java
/**
 * {@link ImportBeanDefinitionRegistrar} to store the base package from the importing
 * configuration.
 */
static class Registrar implements ImportBeanDefinitionRegistrar, DeterminableImports {

   /**
   	* metadata：注解元信息
   	* new PackageImports(metadata).getPackageNames().toArray(new String[0]))：
   	* 获取该注解所注类所在的包名，也就是MainApplication
   	* register()方法：将该包下的所有组件注册进来
   	*/
   @Override
   public void registerBeanDefinitions(
      AnnotationMetadata metadata, BeanDefinitionRegistry registry) {
      register(registry, 
               new PackageImports(metadata).getPackageNames().toArray(new String[0]));
   }

   @Override
   public Set<Object> determineImports(AnnotationMetadata metadata) {
      return Collections.singleton(new PackageImports(metadata));
   }

}
```

###### 2.@Import(AutoConfigurationImportSelector.class)

> 负责将131个场景的所有自动配置默认全部加载。（但是最终会按需配置）

```java
1、利用getAutoConfigurationEntry(annotationMetadata);给容器中批量导入一些组件
2、调用List<String> configurations = 
		getCandidateConfigurations(annotationMetadata, attributes);
   获取到所有候选的需要导入到容器中的配置类
3、利用工厂加载Map<String, List<String>> loadSpringFactories(ClassLoader classLoader)；
   得到所有的组件
4、从META-INF/spring.factories位置来加载一个文件。
       默认扫描我们当前系统里面所有META-INF/spring.factories位置的文件
       spring-boot-autoconfigure-2.5.3.jar包里面也有META-INF/spring.factories
       文件里面写死了spring-boot一启动就要给容器中加载的所有配置类
```

![image-20220924144716703](..\image\image-20220924144716703.png)

![image-20220924144542724](..\image\image-20220924144542724.png)

#### 4.2 按需开启自动配置项

> 虽然131个场景的所有配置在启动的时候默认全部加载。
> 但是会按照==条件装配规则==，最终按需配置。

例如：

aop场景虽然默认加载，但是只有pom文件中导入了aop的包，才会有Advice类，有Avice类才会自动装配该aop组件

![image-20220924145642502](..\image\image-20220924145642502.png)

#### 4.3 自动配置原理总结

自动配置流程总结：

> 场景starter  ---> xxxxAutoConfiguration  ---> 导入xxx组件 ---> xxx组件和类xxxProperties配置绑定 ---> 绑定到配置文件

- SpringBoot先加载所有的自动配置类。 **xxxxAutoConfiguration**
- 每个自动配置类会**按照条件进行生效**，**默认都会绑定配置文件指定的值**。从类xxxxProperties里面拿。类xxxxProperties和配置文件application.properties（application.yaml）进行了绑定
- 生效的配置类就会在容器中装配很多组件
- **只要容器中有这些组件，相当于这些功能都有了**（例如：文件上传解析器、编码解析器）
- 定制化配置：
  - 用户直接**在MyConfigure配置类中**，**@Bean替换底层的组件**
  - 或者用户去看这个组件是获取的配置文件的什么值，然后去**修改配置文件中的值**（查看官方文档，或者直接在底层代码寻找）

> xxxxAutoConfiguration ---> 注册组件 ---> xxxxProperties里面拿值 ---> application.properties修改

#### 4.4 最佳实践

- 引入场景依赖
  - https://docs.spring.io/spring-boot/docs/current/reference/html/using.html#using.build-systems.starters
- 查看自动配置了哪些（选做）
  - 自己分析，引入场景对应的自动配置一般都生效了
  - application.properties配置文件中写上，debug=true，开启自动配置报告。Negative（生效）\Positive（生效）
- 是否需要修改
  - 参照文档修改配置项
    - https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#appendix.application-properties
    - 自己分析。xxxxProperties绑定了配置文件的哪些
  - 自定义加入或者替换组件
    - @Bean、@Component...
  - 自定义器 XXXXCustomizer;
  - ....

### 五、常用开发小工具

- ==lombok==（简化 JavaBean，日志slf4j）:

  ```xml
  <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
  </dependency>
  ```

  注解：@Data、@Slf4j 等...

- ==dev-tools==（开发者工具，可以实现热更新）:

  修改代码后，**Ctrl + F9**，实现更新

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-devtools</artifactId>
      <optional>true</optional>
  </dependency>
  ```

- ==Spring Initializr==（项目初始化向导）

  在idea创建项目时选择 **Spring Initializr**，选择开发所需场景 starter，数据库环境等。
  会**自动**按照约定创建项目目录、导入相应依赖、插件、创建配置文件等，初始化工作。

### 六、SpringBoot配置文件

#### 6.1 properties

SpringBoot项目默认的全局properties文件：==application.properties==。语法即为properties语法

#### 6.2 yaml

##### 1、简介

YAML 是 “YAML Ain't Markup Language”（YAML不是一种标记语言）的递归缩写。在开发的这种语言是，YAML的意思其实是：”Yet Another Markup Language“（仍是一种标记语言）。

非常适合用来做以数据为中心的配置文件

##### 2、基本语法

- key: value; 冒号后面有空格
- 大小写敏感
- 使用缩进表示层级关系
- 缩进不允许使用tab，只允许空格
- 缩进的空格数不重要，只要相同层级的元素左对齐即可
- '#'表示注释
- '' 与 "" 表示字符串内容，会被 转义/不转义

##### 3、数据类型

- 字面量：单个的、不可再分的值。date、boolean、string、number、null

  ```yaml
  k: v
  ```

- 对象：键值对的集合。map、hash、set、object

  ```yaml
  行内写法： k: {k1:v1,k2:v2,k3:v3}
  #或
  k:
    k1: v1
    k2: v2
    k3: v3
  ```

- 数组：一组按次排序的值。array、list、queue

  ```yaml
  行内写法： k: [v1,v2,v3]
  #或者
  k:
   - v1
   - v2
   - v3
  ```

##### 4、自定义类绑定的配置提示

使用配置管理器，并且在打包的时候去除（只是开发工具，和业务无关）：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
</dependency>

<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <excludes>
                    <exclude>
                        <groupId>org.springframework.boot</groupId>
                        <artifactId>spring-boot-configuration-processor</artifactId>
                    </exclude>
                </excludes>
            </configuration>
        </plugin>
    </plugins>
</build>
```

### 七、Web静态资源配置

#### 7.1 静态资源目录

**默认**只要静态资源放在类路径下：/static（or /public or /resources or /META-INF/resources）
访问：当前项目根路径/ + 静态资源名

原理：静态映射/**
请求进来，先去找Controller能不能处理。不能处理的所有请求都交给静态资源处理器，还处理不了报404。

**自定义**修改静态资源路径（修改后以自定义的为准，默认的失效）：
数组形式，可设置多个：

```yaml
spring:
  resources:
    static-locations: [classpath:/haha/,/hehe/]
```

#### 7.2 静态资源访问前缀

默认无前缀，在配置文件中设置：

```yaml
spring:
  mvc:
    static-path-pattern: /static/** #会导致欢迎页 和 网站图标失效
```

**当前项目 + static-path-pattern +静态资源名** = 静态资源文件夹下找

#### 7.3 欢迎页支持

- 静态资源路径下 index.html
  - 可以配置静态资源路径
  - 但是不可以配置静态资源访问前缀。否则导致 index.html 不能访问
  
- controller能处理 /index 请求

  ```java
  @Controller
  public class Index {
      @RequestMapping("/")
      public String index(){
          return "/static/index.html";
      }
  }
  ```

#### 7.4 自定义Favicon

将名为 favicon.ico 的图片放在静态资源路径下，则自动作为网页图标，如果配置了静态路径访问前缀，则会失效。

#### 7.5 静态资源配置原理

为什么配置静态资源前缀，欢迎页和图标就用不了？

源码解释：

- SpringBoot启动默认加载 xxxAutoConfiguration 类（自动配置类）

- SpringMVC功能的自动配置类 WebMvcAutoConfiguration，生效

  ```java
  @Configuration(proxyBeanMethods = false)
  @ConditionalOnWebApplication(type = Type.SERVLET)
  @ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
  @ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
  @AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
  @AutoConfigureAfter({ DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class,
        ValidationAutoConfiguration.class })
  public class WebMvcAutoConfiguration {}
  ```

- 给容器中配了什么（一个静态内部类，也是一个配置类）：

  ```java
  @Configuration(proxyBeanMethods = false)
  @Import(EnableWebMvcConfiguration.class)
  @EnableConfigurationProperties({ WebMvcProperties.class, ResourceProperties.class })
  @Order(0)
  public static class WebMvcAutoConfigurationAdapter implements WebMvcConfigurer {}
  ```

- 配置文件的相关属性和xxx进行了绑定：

  WebMvcProperties.class == **spring.mvc**、ResourceProperties.class == **spring.resources**

  ```java
  @ConfigurationProperties(prefix = "spring.mvc")
  public class WebMvcProperties {}
  
  @ConfigurationProperties(prefix = "spring.resources", ignoreUnknownFields = false)
  public class ResourceProperties {}
  ```

- 当配置类只有一个有参构造器，有参构造中所有参数都从容器中确定

  ```java
  //ResourceProperties resourceProperties；获取和spring.resources绑定的所有的值的对象
  //WebMvcProperties mvcProperties；获取和spring.mvc绑定的所有的值的对象
  //ListableBeanFactory beanFactory；spring的beanFactory工厂
  //HttpMessageConverters；找到所有的HttpMessageConverters
  //ResourceHandlerRegistrationCustomizer； 找到资源处理器的自定义器
  //DispatcherServletPath；能处理的路径
  //ServletRegistrationBean；给应用注册Servlet、Filter...
  public WebMvcAutoConfigurationAdapter(
      ResourceProperties resourceProperties, 
      WebMvcProperties mvcProperties,
      ListableBeanFactory beanFactory, 
      ObjectProvider<HttpMessageConverters> messageConvertersProvider,
      ObjectProvider<ResourceHandlerRegistrationCustomizer> resourceHandlerRegistrationCustomizerProvider,
      ObjectProvider<DispatcherServletPath> dispatcherServletPath,
      ObjectProvider<ServletRegistrationBean<?>> servletRegistrations) {
         this.resourceProperties = resourceProperties;
         this.mvcProperties = mvcProperties;
         this.beanFactory = beanFactory;
         this.messageConvertersProvider = messageConvertersProvider;
         this.resourceHandlerRegistrationCustomizer = resourceHandlerRegistrationCustomizerProvider.getIfAvailable();
         this.dispatcherServletPath = dispatcherServletPath;
         this.servletRegistrations = servletRegistrations;
  }
  ```

- 静态资源处理的默认规则和自定义处理：

  ```java
  @Override
  public void addResourceHandlers(ResourceHandlerRegistry registry) {
     //isAddMappings()获取配置中add-mappings值，默认为true，若设置为false，则取消所有默认配置规则
     if (!this.resourceProperties.isAddMappings()) {
        logger.debug("Default resource handling disabled");
        return;
     }
     
     Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
     CacheControl cacheControl = this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl();
     //webjars的规则
     if (!registry.hasMappingForPattern("/webjars/**")) {
        customizeResourceHandlerRegistration(registry.addResourceHandler("/webjars/**")
              .addResourceLocations("classpath:/META-INF/resources/webjars/")
              .setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
     }
     //默认的静态资源处理规则：
     //getStaticPathPattern() 默认获取值 staticPathPattern = "/**";
     //而mvcProperties又与application.yaml配置文件中spring.mvc绑定，也就是自定义资源路径前缀
     //getResourceLocations(this.resourceProperties.getStaticLocations()) 默认获取值: 
     //CLASSPATH_RESOURCE_LOCATIONS = { "classpath:/META-INF/resources/","classpath:/resources/",    "classpath:/static/", "classpath:/public/" };
     //而resourceProperties又与application.yaml配置文件中spring.resources绑定，也就是自定义静态资源目录
     String staticPathPattern = this.mvcProperties.getStaticPathPattern();
     if (!registry.hasMappingForPattern(staticPathPattern)) {
        customizeResourceHandlerRegistration(registry.addResourceHandler(staticPathPattern)
              .addResourceLocations(getResourceLocations(this.resourceProperties.getStaticLocations()))
              .setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
     }
  }
  ```

- 可以配置add-mappings，禁用掉所有静态资源：

  ```yaml
  spring:
    resources:
      add-mappings: false #禁用所有静态资源
  ```

- 欢迎页处理规则，解释其失效的原因：

  ```java
  //HandlerMapping：处理器映射。保存了每一个Handler能处理哪些请求
  //关键在于WelcomePageHandlerMapping的构造，传入了静态资源路径的参数
  @Bean
  public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext applicationContext,
        FormattingConversionService mvcConversionService, ResourceUrlProvider mvcResourceUrlProvider) {
     WelcomePageHandlerMapping welcomePageHandlerMapping = new WelcomePageHandlerMapping(
           new TemplateAvailabilityProviders(applicationContext), applicationContext, getWelcomePage(),
           this.mvcProperties.getStaticPathPattern());
     welcomePageHandlerMapping.setInterceptors(getInterceptors(mvcConversionService, mvcResourceUrlProvider));
     welcomePageHandlerMapping.setCorsConfigurations(getCorsConfigurations());
     return welcomePageHandlerMapping;
  }
  ```

  在 WelcomePageHandlerMapping 构造器中：

  ```java
  WelcomePageHandlerMapping(TemplateAvailabilityProviders templateAvailabilityProviders,
        ApplicationContext applicationContext, Optional<Resource> welcomePage, String staticPathPattern) {
     //如果欢迎页存在并且静态资源路径是"/**"，才能访问欢迎页，这就是加入静态资源前缀就无法访问欢迎页的原因
     if (welcomePage.isPresent() && "/**".equals(staticPathPattern)) {
        logger.info("Adding welcome page: " + welcomePage.get());
        setRootViewName("forward:index.html");
     }
     //调用Controller，看是否有处理/index请求的
     else if (welcomeTemplateExists(templateAvailabilityProviders, applicationContext)) {
        logger.info("Adding welcome page template: index");
        setRootViewName("index");
     }
  }
  ```

- 网站图标处理，浏览器默认/**，加入静态资源前缀浏览器就找不到

### 八、请求参数处理

#### 8.1 请求映射（Restful风格）

- @xxxMapping

- ==Restful==风格支持（使用HTTP请求方式动词来表示对资源的操作）

  - 以前：请求url：/getUser 获取用户、/delete User 删除用户、/editUser 修改用户、/saveUser 保存用户

    路径太多、起名繁琐

  - 现在：请求url统一为：/user    若带有参数则：/user/{name}

    **根据请求方式的不同，访问不同的Controller**： GET-获取用户    DELETE-删除用户    PUT-修改用户    POST-保存用户

  - **若前后端不分离**，以及前端使用原生html时：

    - 表单提交method必须为post，如果请求方式是put、delete，需要设置隐藏域 _method=put / method=delete：
  
      ```html
      <form action="/user" method="post">
            <input name="_method" type="hidden" value="PUT"/>
            <input value="REST-PUT 提交" type="submit"/>
      </form>
  
    - 需要手动开启核心Filter：**HiddenHttpMethodFilter** ：SpringBoot中手动开启：
  
      ```yaml
    spring:
        mvc:
          hiddenmethod:
            filter:
              enabled: true
      ```
  
    - **注意**：如果是原生表单提交，请求方法参数不能加==@RequestBody==注解
  
  - 示例：
  
    ```java
    @RequestMapping(value = "/user", method = RequestMethod.GET)
    public String selectUser(){
        return "GET-张三";
    }
    @RequestMapping(value = "/user", method = RequestMethod.POST)
    public String addUser(){
        return "POST-张三";
    }
    @RequestMapping(value = "/user", method = RequestMethod.PUT)
    public String updateUser(){
        return "PUT-张三";
    }
    @RequestMapping(value = "/user", method = RequestMethod.DELETE)
    public String deleteUser(){
        return "DELETE-张三";
    }
    ```
  
    开发时==直接使用新注解==即可（@RequestMapping的包装）：
  
    ```java
    @GetMapping("/user")
    public String selectUser(){
        return "GET-张三";
    }
    @PostMapping("/user")
    public String addUser(){
        return "POST-张三";
    }
    @PutMapping("/user")
    public String updateUser(){
        return "PUT-张三";
    }
    @DeleteMapping("/user")
    public String deleteUser(){
        return "DELETE-张三";
    }
    ```
  
- Rest原理（表单提交要使用REST的时候）

  - 表单提交会带上 _method=PUT
  - 请求过来被 HiddenHttpMethodFilter 拦截
    - 请求是否正常，并且是POST
      - 获取到 _method 的值
      - 兼容以下请求：PUT DELETE PATCH
      - 原生request（post），包装模式 requesWrapper重写了getMethod()方法，返回的是传入的值。
      - 过滤器链放行的时候用wrapper。以后的方法调用getMethod()是调用requesWrapper的。

- 如果使用客户端工具，或者是前后端分离的项目：

  - 如PostMan直接发送PUT、DELETE等方式请求，无需Filter
  - 前后端分离，前端发送异步AJAX请求，无需Filter

#### 8.2 请求映射原理

> 从之前SpringMVC的学习知道，SpringMVC提供了一个名为DispatcherServlet的类（SpringMVC中央处理器，也就是前端控制器），用于拦截用户请求交由SpringMVC处理。其负责接收请求，协同各组件工作，响应请求。
>
> 学过的相关内容，在SpringMVC学习笔记的第四章

原理分析：

DispatcherServlet继承了FrameworkServlet，FrameworkServlet继承了HttpServletBean，HttpServletBean继承了HttpServlet，也就是原生的Servlet，通过方法的继承和实现，得出方法调用顺序，从原生HttpServlet的doGet()开始：
HttpServlet的doGet() ---> FrameworkServlet的processRequest() ---> DispatcherServle的doService() ---> DispatcherServle的doDispatch()

![image-20220927215555387](..\image\image-20220927215555387.png)

**SpringMVC的功能分析都从org.springframework.web.servlet.DispatcherServlet** 的 ==doDispatch()==方法 **开始：**

```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
   HttpServletRequest processedRequest = request;
   HandlerExecutionChain mappedHandler = null;
   boolean multipartRequestParsed = false;

   WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);

   try {
      ModelAndView mv = null;
      Exception dispatchException = null;

      try {
         processedRequest = checkMultipart(request);
         multipartRequestParsed = (processedRequest != request);

         // 找到当前请求使用哪个Handler（Controller的方法）处理
         mappedHandler = getHandler(processedRequest);
```

```java
@Nullable
protected HandlerExecutionChain getHandler(HttpServletRequest request) throws Exception {
   if (this.handlerMappings != null) {
      //HandlerMapping：处理器映射，负责根据URL找到对应的Handler。
      //this.handlerMappings:默认储存了所有处理器映射，在这里挨个尝试handler是否有这个进来的请求信息
      for (HandlerMapping mapping : this.handlerMappings) {
         HandlerExecutionChain handler = mapping.getHandler(request);
         if (handler != null) { //找到后就返回
            return handler;
         }
      }
   }
   return null;
}
```

**this.handlerMappings 默认储存了所有处理器映射：**

第一个是加上了@RequestMapping注解的handler(controller)的HandlerMapping，第二个是欢迎页的HandlerMapping

![image-20220927222815848](..\image\image-20220927222815848.png)

**RequestMappingHandlerMapping：保存了所有@RequestMapping 的 handler的映射规则。**

![image-20220927223816904](..\image\image-20220927223816904.png)

总结：

所有的请求映射都在HandlerMapping中。

- SpringBoot自动配置欢迎页的HandlerMapping。访问 / 就能访问到index.html；
- **SpringBoot自动配置了默认的 RequestMappingHandlerMapping**
- 请求进来，**挨个尝试所有的HandlerMapping**看是否有该请求信息。
  - 有，就找到这个请求对应的handler
  - 没有，从下一个HandlerMapping中找
- 我们需要一些自定义的映射处理，可以自己给容器中放HandlerMapping。**自定义HandlerMapping**

#### 8.3 常用参数和注解

- **注解：**

  ==@PathVariable==、==@RequestParam==、==@RequestHeader==、==@RequestBody==、==@CookieValue==、==@ModelAttribute==、==@MatrixVariable==

  ```java
  @RestController
  public class ParameterTestController {
  
      //RESTful风格：/car/{id}/owner/{username}
      //示例：http://localhost:8080/car/8888/owner/陈林迅?age=22&inters=篮球&inters=rap
      @GetMapping("/car/{id}/owner/{username}")
      public Map<String,Object> getCar(@PathVariable("id") Integer id,
                                       @PathVariable("username") String name,
                                       @PathVariable Map<String,String> pv, //获取所有路径参数放到map中
                                       @RequestHeader("User-Agent") String userAgent,
                                       @RequestHeader Map<String,String> header, //获取所有请求头放到map中
                                       @RequestParam("age") Integer age,
                                       @RequestParam("inters") List<String> inters,
                                       @RequestParam Map<String,String> params,//获取所有请求参数放到map中
                                       //获取名为Idea_c84cbbae的cookie的值
                                       @CookieValue("Idea-c84cbbae") String Idea_c84cbbae,
                                       //获取名为Idea_c84cbbae的cookie
                                       @CookieValue("Idea-c84cbbae") Cookie cookie
                                      ){
  
          Map<String,Object> map = new HashMap<>();
  
          map.put("id",id);
          map.put("name",name);
          map.put("pv",pv);
          map.put("userAgent",userAgent);
          map.put("headers",header);
          map.put("age",age);
          map.put("inters",inters);
          map.put("params",params);
          map.put("Idea_c84cbbae",Idea_c84cbbae);
          System.out.println(cookie.getName()+"===>"+cookie.getValue());
          return map;
      }
  
      @PostMapping("/saveCar")
      public Car save(@RequestBody Car car){//原生表单提交，不能使用@RequestBody注解
          return car;
      }
  }
  ```

- **Servlet API：**

  WebRequest、ServletRequest、MultipartRequest、HttpSession、javax.servlet.http.PushBuilder、Principal、lnputStream、Reader、HttpMethod、Locale、TimeZone、Zoneld

- **复杂参数：**

  **Map**、**Model（map、model里面的数据会被放在request请求域中 request.setAttribute）**、Errors/BindingResult、**RedirectAttributes（重定向携带数据）**、**ServletResponse（response）**、SessionStatus、UriComponentsBuilder、ServletUriComponentsBuilder

  ```java
  @Controller
  public class RequestController {
  
      @GetMapping("/params")
      public String testParam(Map<String,Object> map,
                              Model model,
                              HttpServletRequest request,
                              HttpServletResponse response){
  
          //相当于给request域中放数据：
          map.put("hello","world666");
          model.addAttribute("world","hello666");
          request.setAttribute("message","HelloWorld");
  
          //设置Cookie
          Cookie cookie = new Cookie("c1", "v1");
          response.addCookie(cookie);
          return "forward:success";
      }
  
      @ResponseBody
      @GetMapping("/success")
      public Map success(@RequestAttribute(value = "msg",required = false) String msg,
                         @RequestAttribute(value = "code",required = false) Integer id,
                         HttpServletRequest request){
  
          //获取request请求域中的数据
          Object msg1 = request.getAttribute("msg"); //没传，就是没有
          //从/Params转发来的请求域数据：
          Object hello = request.getAttribute("hello");
          Object world = request.getAttribute("world");
          Object message = request.getAttribute("message");
  
          HashMap<Object, Object> map = new HashMap<>();
          map.put("reqMethod_msg",msg1);
          map.put("annotation_msg",msg);
          map.put("hello",hello);
          map.put("world",world);
          map.put("message",message);
  
          return map;
      }
  }
  ```

- **自定义对象参数：**

  不加@RequestBody注解，可以自动类型转换与格式化，可以级联封装。
  
  ```java
  /**
   *   姓名：<input name="userName"/> <br/>
   *   年龄：<input name="age"/> <br/> 
   *   生日：<input name="birth"/> <br/> 
   *   宠物姓名：<input name="pet.name"/>
   */
  @Data
  public class Person {
      private String userName;
      private Integer age;
      private Date birth;
      private Pet pet;
  }
  
  @Data
  public class Pet {
      private String name;
  }
  ```
  
  ```java
  @RestController
  public class ParameterTestController {
  
      /**
       * 数据绑定：页面提交的请求数据（Get、Post）都可以和对象属性进行绑定
       * @param person
       * @return
       */
      @PostMapping("/saveUser")
      public Person saveUser(Person person){return person;}
  }
  ```

#### 8.4 参数处理原理

##### 1、大致处理过程

- 1、仍是分析DispatcherServlet源码

- 2、在之前请求映射原理的分析中，HandlerMapping中找到能处理请求的Handler（Controller.method()）

- 3、为当前Handler找一个适配器 ==HandlerAdapter== ：**RequestMappingHandlerAdapter**

  ( ==HandlerAdapter== ：处理器适配器

  - 作用：**按照处理器映射器解析的用户请求的调用链，通过适配器模式完成处理器Handler的调用**

  ![image-20220928151838703](..\image\image-20220928151838703.png)

  0 ：**支持方法上标注@RequestMapping注解的 Handler**

  1： 支持函数式编程的 Handler

  ... ... )

- 4、使用该适配器真正执行Handler方法，处理请求

  ```java
  protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
  
      	略...
      
           // Determine handler for the current request.
           // 获取处理当前请求的handler
           mappedHandler = getHandler(processedRequest);
           if (mappedHandler == null) {
              noHandlerFound(processedRequest, response);
              return;
           }
  
           // Determine handler adapter for the current request.
           // 获取处理该handler的处理器适配器
           HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());
  
           // Process last-modified header, if supported by the handler.
           String method = request.getMethod();
           boolean isGet = "GET".equals(method);
           if (isGet || "HEAD".equals(method)) {
              long lastModified = ha.getLastModified(request, mappedHandler.getHandler());
              if (new ServletWebRequest(request, response).checkNotModified(lastModified) && isGet) {
                 return;
              }
           }
  
           if (!mappedHandler.applyPreHandle(processedRequest, response)) {
              return;
           }
  
           // Actually invoke the handler.
           // 真正执行handler的方法，处理请求
           mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
  
           if (asyncManager.isConcurrentHandlingStarted()) {
              return;
           }
  
           applyDefaultViewName(processedRequest, mv);
           mappedHandler.applyPostHandle(processedRequest, response, mv);
        }
  ```

- 5、进入 ha.handle()后，执行handler方法的是这行代码：

  ```java
  mav = invokeHandlerMethod(request, response, handlerMethod);
  ```

##### 2、执行目标方法详解

**invokeHandlerMethod(request, response, handlerMethod)方法解析：**

```java
protected ModelAndView invokeHandlerMethod(HttpServletRequest request,
      HttpServletResponse response, HandlerMethod handlerMethod) throws Exception {

    ServletWebRequest webRequest = new ServletWebRequest(request, response);
    try {
        WebDataBinderFactory binderFactory = getDataBinderFactory(handlerMethod);
        ModelFactory modelFactory = getModelFactory(handlerMethod, binderFactory);

        ServletInvocableHandlerMethod invocableMethod = createInvocableHandlerMethod(handlerMethod);
        
        //argumentResolvers：参数解析器
        if (this.argumentResolvers != null) {
            invocableMethod.setHandlerMethodArgumentResolvers(this.argumentResolvers);
        }
        //returnValueHandlers：返回值处理器
        if (this.returnValueHandlers != null) {
            invocableMethod.setHandlerMethodReturnValueHandlers(this.returnValueHandlers);
        }
        
        省略其他invocableMethod的封装过程等...

        //最核心的一步，执行目标方法：（此时目标方法 invocableMethod 已经封装完毕）
        //真正执行目标方法就在这个方法里面：
        invocableMethod.invokeAndHandle(webRequest, mavContainer);
        
        if (asyncManager.isConcurrentHandlingStarted()) {
            return null;
        }
		
        //返回 ModelAndView 给前端控制器 DisPatcherServlet
        return getModelAndView(mavContainer, modelFactory, webRequest);
    }
    finally {
        webRequest.requestCompleted();
    }
}
```

**参数解析器：**

> argumentResolvers：
> 确定将要执行的目标方法的每一个参数的值是什么；
> SpringMVC目标方法能写多少种参数类型，取决于参数解析器。有26个（种）:

![image-20220928163542540](..\image\image-20220928163542540.png)

![image-20220928163953111](..\image\image-20220928163953111.png)

>  参数解析器接口，包含两个方法：
>
> - 当前解析器是否支持解析这种参数 supportsParameter()
> - 支持就调用 resolveArgument() 进行解析

**返回值处理器**：

> returnValueHandlers：
> 确定目标方法返回值的类型。有15种：

![image-20220928164514761](..\image\image-20220928164514761.png)

![image-20221001023342497](..\image\image-20221001023342497.png)

> 返回值处理器接口，包含两个方法：
>
> - 当前处理器是否支持处理这个返回值 supportsReturnType()
>
> - 支持就调用 handleReturValue() 进行处理

**真正执行目标方法的代码：**

**进入invocableMethod.invokeAndHandle(webRequest, mavContainer); 方法中：**

```java
public void invokeAndHandle(ServletWebRequest webRequest, ModelAndViewContainer mavContainer,
			Object... providedArgs) throws Exception {
   //执行invokeForRequest方法：
   Object returnValue = invokeForRequest(webRequest, mavContainer, providedArgs);
   略...
}
```

```java
public Object invokeForRequest(NativeWebRequest request, @Nullable ModelAndViewContainer mavContainer, Object... providedArgs) throws Exception {
    
    //确定目标方法参数值：
    Object[] args = this.getMethodArgumentValues(request, mavContainer, providedArgs);
    if (this.logger.isTraceEnabled()) {
        this.logger.trace("Arguments: " + Arrays.toString(args));
    }

    //最终真正执行目标方法：（该方法调用反射工具类，利用反射，完成目标方法的执行）
    return this.doInvoke(args);
}
```

##### 3、参数处理详解

**如何确定目标方法参数值的？**

> 先判断各参数解析器是否支持这种参数类型，如果支持再进行解析

**getMethodArgumentValues()方法详解：**

```java
protected Object[] getMethodArgumentValues(NativeWebRequest request, @Nullable ModelAndViewContainer mavContainer, Object... providedArgs) throws Exception {

    //获取目标方法的全部参数，例如10个
    MethodParameter[] parameters = getMethodParameters();
    if (ObjectUtils.isEmpty(parameters)) {
        return EMPTY_ARGS;
    }

    Object[] args = new Object[parameters.length];
    
    //遍历这10个参数：
    for (int i = 0; i < parameters.length; i++) {
        MethodParameter parameter = parameters[i];
        //初始化工作，先不管
        parameter.initParameterNameDiscovery(this.parameterNameDiscoverer);
        args[i] = findProvidedArgument(parameter, providedArgs);
        if (args[i] != null) {
            continue;
        }
        //先判断参数解析器是否支持这种参数类型
        if (!this.resolvers.supportsParameter(parameter)) {
            throw new IllegalStateException(formatArgumentError(parameter, "No suitable resolver"));
        }
        try {
            //如果支持，才进行解析（args[i]才不是null）
            args[i] = this.resolvers.resolveArgument(parameter, mavContainer, 
                                                     request, this.dataBinderFactory);
        }
        catch (Exception ex) {
            // Leave stack trace for later, exception may actually be resolved and handled...
            if (logger.isDebugEnabled()) {
                String exMsg = ex.getMessage();
                if (exMsg != null && !exMsg.contains(parameter.getExecutable().toGenericString())) {
                    logger.debug(formatArgumentError(parameter, exMsg));
                }
            }
            throw ex;
        }
    }
    return args; //返回确定的参数
}
```

**如何判断参数解析器是否支持这种参数类型？**

**this.resolvers.supportsParameter(parameter) 方法解析：**

> 挨个判断所有参数解析器，哪个支持解析这个参数

```java
public boolean supportsParameter(MethodParameter parameter) {
    return getArgumentResolver(parameter) != null;
}
```

```java
private HandlerMethodArgumentResolver getArgumentResolver(MethodParameter parameter) {
    HandlerMethodArgumentResolver result = this.argumentResolverCache.get(parameter);
    if (result == null) {
        //遍历所有参数解析器 this.argumentResolvers
        for (HandlerMethodArgumentResolver resolver : this.argumentResolvers) {
            //调用supportsParameter(parameter)判断是否能解析该参数，各解析器的判断依据不尽相同，
            //一些类型的判断是看这个参数是否标注该解析器的注解，还有的判断是看是否是简单类型，如POJO类型的判断。
            //例如：第一个参数解析器是RequestParam类型的，而第一个参数是@PathVarible类型的，则进入下一个循环
            //一直遍历到PathVarible类型的参数解析器，完成判断
            if (resolver.supportsParameter(parameter)) {
                result = resolver;
                //找到对应支持的解析器后，将解析器和参数放到缓存中，等待解析时拿来用
                this.argumentResolverCache.put(parameter, result);
                break;
            }
        }
    }
    return result;
}
```

**判断后，如何进行参数解析？**

**this.resolvers.resolveArgument(parameter, mavContainer, request, this.dataBinderFactory)方法解析：**

```java
public Object resolveArgument(MethodParameter parameter, @Nullable ModelAndViewContainer mavContainer,
			NativeWebRequest webRequest, @Nullable WebDataBinderFactory binderFactory) throws Exception {

    //拿到当前参数的参数解析器
    HandlerMethodArgumentResolver resolver = getArgumentResolver(parameter);
    if (resolver == null) {
        throw new IllegalArgumentException("Unsupported parameter type [" +
                                           parameter.getParameterType().getName() + 
                                           "]. supportsParameter should be called first.");
    }
    //调用方法进行解析：（调用各种解析器解析参数名、参数值）
    return resolver.resolveArgument(parameter, mavContainer, webRequest, binderFactory);
}
```

#### 8.5 POJO封装原理

##### 1、封装过程

> 在使用自定义对象参数，可以自动类型转换与格式化，页面传过来的数据会和对象属性进行绑定。

Step1、根据参数处理的原理得知，自定义对象参数对应支持的**参数处理器**是：**ServletModleAttributeMethodProcessor** 
**判断支持的条件是：是否为简单类型** ：

```java
public boolean supportsParameter(MethodParameter parameter) {
    //该参数是否标注了@ModelAttribute注解，或者 该参数不用必须标注注解 并且 该参数类型不是简单类型
    //this.annotationNotRequired：注解不是必须的
    //!BeanUtils.isSimpleProperty(parameter.getParameterType()：参数类型不是简单类型
    return (parameter.hasParameterAnnotation(ModelAttribute.class) ||
            (this.annotationNotRequired && !BeanUtils.isSimpleProperty(parameter.getParameterType())));
}
```

简单类型：

```java
public static boolean isSimpleValueType(Class<?> type) {
    return (Void.class != type && void.class != type &&
            (ClassUtils.isPrimitiveOrWrapper(type) ||
             Enum.class.isAssignableFrom(type) ||
             CharSequence.class.isAssignableFrom(type) ||
             Number.class.isAssignableFrom(type) ||
             Date.class.isAssignableFrom(type) ||
             Temporal.class.isAssignableFrom(type) ||
             URI.class == type ||
             URL.class == type ||
             Locale.class == type ||
             Class.class == type));
}
```

Step2、**找到支持的对应解析器后，如何进行解析的呢？**

进入**resolver.resolveArgument(parameter, mavContainer, webRequest, binderFactory)**方法查看：

> 先创建一个空的该POJO类型的对象，然后通过WebDataBinder：web数据绑定器，利用它里面的Converters，将请求数据转成指定的数据类型，然后封装到JavaBean里面

```java
public final Object resolveArgument(MethodParameter parameter, @Nullable ModelAndViewContainer mavContainer,
			NativeWebRequest webRequest, @Nullable WebDataBinderFactory binderFactory) throws Exception {

    Assert.state(mavContainer != null, "ModelAttributeMethodProcessor requires ModelAndViewContainer");
    Assert.state(binderFactory != null, "ModelAttributeMethodProcessor requires WebDataBinderFactory");

    String name = ModelFactory.getNameForParameter(parameter);
    //判断参数是否标注了@ModelAttribute注解，没有标注，这里跳过
    ModelAttribute ann = parameter.getParameterAnnotation(ModelAttribute.class);
    if (ann != null) {
        mavContainer.setBinding(name, ann.binding());
    }

    Object attribute = null;
    BindingResult bindingResult = null;

    if (mavContainer.containsAttribute(name)) {
        attribute = mavContainer.getModel().get(name);
    }
    else {
        // Create attribute instance
        try {
            //在这里创建了一个属性都为空的对象（就是该参数POJO类型的对象）
            attribute = createAttribute(name, parameter, binderFactory, webRequest);
        }
        catch (BindException ex) {
            if (isBindExceptionRequired(parameter)) {
                // No BindingResult parameter -> fail with BindException
                throw ex;
            }
            // Otherwise, expose null/empty value and associated BindingResult
            if (parameter.getParameterType() == Optional.class) {
                attribute = Optional.empty();
            }
            bindingResult = ex.getBindingResult();
        }
    }

    if (bindingResult == null) {
        // WebDataBinder：web数据绑定器，利用它里面的Converters，将请求数据转成指定的数据类型，然后封装到JavaBean里面
        // attribute：就是刚才创建的JavaBean（POJO空对象）
        WebDataBinder binder = binderFactory.createBinder(webRequest, attribute, name);
        if (binder.getTarget() != null) {
            if (!mavContainer.isBindingDisabled(name)) {
                //这一步就是利用Converters将请求数据封装到JavaBean里面
                bindRequestParameters(binder, webRequest);
            }
            validateIfApplicable(binder, parameter);
            if (binder.getBindingResult().hasErrors() && isBindExceptionRequired(binder, parameter)) {
                throw new BindException(binder.getBindingResult());
            }
        }
        // Value type adaptation, also covering java.util.Optional
        if (!parameter.getParameterType().isInstance(attribute)) {
            attribute = binder.convertIfNecessary(binder.getTarget(), parameter.getParameterType(), parameter);
        }
        bindingResult = binder.getBindingResult();
    }

    // Add resolved attribute and BindingResult at the end of the model
    Map<String, Object> bindingResultModel = bindingResult.getModel();
    mavContainer.removeAttributes(bindingResultModel);
    mavContainer.addAllAttributes(bindingResultModel);

    return attribute;
}
```

**类型转换器converters：**

> WebDataBinder：web数据绑定器，它里面包含了数据类型转换服务，GenericConversionService
>
> 该服务包含了124个数据类型转换器：Converters，在设置每一个值的时候，找它里面的所有converter，哪个可以将这个数据类型转换到指定的类型
>
> Converters把浏览器传来的Http文本信息，转换成POJO中的属性类型，例如把String类型转换成Integer类型，都靠这124个类型转换器完成，转换完成后，利用反射将数据绑定到 target POJO对象中，完成参数解析。
>
> Spring支持我们可以给WebDataBinder里面放自己的Converter，自定类型转换器
> Converter的总接口：**public interface Converter<S, T>{}** 

![image-20220929144014982](..\image\image-20220929144014982.png)

3、**具体转换绑定解析流程：**
**bindRequestParameters(binder, webRequest);方法：**

```java
protected void bindRequestParameters(WebDataBinder binder, NativeWebRequest request) {
    //获取原生的ServletRequest对象
    ServletRequest servletRequest = request.getNativeRequest(ServletRequest.class);
    Assert.state(servletRequest != null, "No ServletRequest");
    //将传入的web数据绑定器 binder进行向下转型
    ServletRequestDataBinder servletBinder = (ServletRequestDataBinder) binder;
    //调用绑定器进行数据绑定
    servletBinder.bind(servletRequest);
}
```

**servletBinder.bind(servletRequest);方法：**

```java
public void bind(ServletRequest request) {
    //先获取原生request请求里的所有k-v对，也就是所有参数
    MutablePropertyValues mpvs = new ServletRequestParameterPropertyValues(request);
    MultipartRequest multipartRequest = WebUtils.getNativeRequest(request, MultipartRequest.class);
    if (multipartRequest != null) {
        bindMultipart(multipartRequest.getMultiFileMap(), mpvs);
    }
    //额外添加绑定的值，先不管
    addBindValues(mpvs, request);
    //真正绑定在这里面
    doBind(mpvs);
}
```

**doBind(mpvs);方法：**

```java
protected void doBind(MutablePropertyValues mpvs) {
    //检查参数
    checkFieldDefaults(mpvs);
    checkFieldMarkers(mpvs);
    //真正的绑定在这里
    super.doBind(mpvs);
}
```

**super.doBind(mpvs);方法：**

```java
protected void doBind(MutablePropertyValues mpvs) {
    //检查
    this.checkAllowedFields(mpvs);
    this.checkRequiredFields(mpvs);
    //真正的绑定，apply，应用参数的值
    this.applyPropertyValues(mpvs);
}
```

**this.applyPropertyValues(mpvs);方法：**

```java
protected void applyPropertyValues(MutablePropertyValues mpvs) {
    try {
        //setPropertyValues：设置属性值
        this.getPropertyAccessor().setPropertyValues(mpvs, 
                                                     this.isIgnoreUnknownFields(), 
                                                     this.isIgnoreInvalidFields());
    } catch (PropertyBatchUpdateException var7) {
        PropertyAccessException[] var3 = var7.getPropertyAccessExceptions();
        int var4 = var3.length;

        for(int var5 = 0; var5 < var4; ++var5) {
            PropertyAccessException pae = var3[var5];
            this.getBindingErrorProcessor().processPropertyAccessException(pae, this.getInternalBindingResult());
        }
    }

}
```

**this.getPropertyAccessor().setPropertyValues()设置属性值：**

```java
public void setPropertyValues(PropertyValues pvs, boolean ignoreUnknown, boolean ignoreInvalid)
			throws BeansException {

    List<PropertyAccessException> propertyAccessExceptions = null;
    List<PropertyValue> propertyValues = (pvs instanceof MutablePropertyValues ?
                                          ((MutablePropertyValues) pvs).getPropertyValueList() : 
                                          Arrays.asList(pvs.getPropertyValues()));
    //拿到所有的属性值，挨个进行属性设置：
    for (PropertyValue pv : propertyValues) {
        try {
            //进行属性设置（在这里面利用converters进行类型转换，并且利用各种反射工具进行属性设置）：
            setPropertyValue(pv);
        }
        catch (NotWritablePropertyException ex) {
           略...
    }
    略...
}
```

##### 2、自定义Converter

> 分析POJO封装原理得知，参数解析器解析POJO属性时，需要使用Converter转换器，进行数据转换和绑定
>
> SpringBoot支持我们除了使用默认的124个Converter转换器，也可以自定义Converter，根据自定义逻辑进行数据转换

示例：

```html
<h2>测试自定义Converter进行POJO封装</h2>
<form action="/saveUser" method="post">
    姓名<input name="userName" value="张三"> <br/>
    年龄<input name="age" value="20"> <br/>
    生日<input name="birth" value="2000/03/21"> <br/>
    宠物<input name="pet" value="小柴,3"> <br/>
    <input type="submit" name="提交"> (公司自定义规则，不使用级联映射，对象参数中的属性用逗号分隔)
</form>
```

> - 创建配置类，用于配置web相关自定义配置
> - @Bean注册添加WebMvcConfigurer组件，该组件是SpringBoot组件，可通过实现该组件中的方法添加许多自定义组件
> - 实现addFormatters()方法可以添加 Formatter格式转换器、Converter数据类型转换器
> - FormatterRegistry继承了ConverterRegistry，通过其可以实现addConverter()方法完成添加自定义Converter
> - 实现addConverter() 需要传入一个Converter对象，通过实现Converter接口完成自定义Converter
> - Converter接口：interface Converter<S, T> ，S：source源类型，T：target目标类型

```java
@Configuration
public class WebConfig {
    @Bean
    public WebMvcConfigurer webMvcConfig(){
        return new WebMvcConfigurer() {
            @Override
            public void addFormatters(FormatterRegistry registry) {
                registry.addConverter(new Converter<String, Pet>() {
                    @Override
                    public Pet convert(String source) {
                        if(!StringUtils.isEmpty(source)){
                            //实现自定义转换逻辑
                            String[] split = source.split(",");
                            Pet pet = new Pet();
                            pet.setName(split[0]);
                            pet.setAge(Integer.parseInt(split[1]));
                            return pet;
                        }
                        return null;
                    }
                });
            }
        };
    }
}
```

### 九、数据响应与内容协商

> 数据响应的两种类型：

![image-20220929174648100](..\image\image-20220929174648100.png)

#### 9.1 响应JSON

##### 1、响应JSON方式的使用

**jackson.jar + @ResponesBody**

引入starter-web场景时，tarter-web自动引入了starter-json：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-json</artifactId>
    <version>2.3.4.RELEASE</version>
    <scope>compile</scope>
</dependency>
```

starter-json就引入了jackson的相关jar包：

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.11.2</version>
    <scope>compile</scope>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.datatype</groupId>
    <artifactId>jackson-datatype-jdk8</artifactId>
    <version>2.11.2</version>
    <scope>compile</scope>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.datatype</groupId>
    <artifactId>jackson-datatype-jsr310</artifactId>
    <version>2.11.2</version>
    <scope>compile</scope>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.module</groupId>
    <artifactId>jackson-module-parameter-names</artifactId>
    <version>2.11.2</version>
    <scope>compile</scope>
</dependency>
```

**使用**也非常简单：

- 返回 JSON 数据的方法上使用==@ResponesBody==注解即可
- 如果一个Controller中的所有方法都返回 JSON数据，只需要在该Controller上使用==@RestController==即可

##### 2、返回值处理器选取原理

**之前在invokeHandlerMethod(request, response, handlerMethod)方法中，先将15种返回值处理器封装进去**

```java
 //returnValueHandlers：返回值处理器
if (this.returnValueHandlers != null) {
    invocableMethod.setHandlerMethodReturnValueHandlers(this.returnValueHandlers);
}
```

**然后进入到invocableMethod.invokeAndHandle(webRequest, mavContainer); 方法中：**

进入invokeForRequest()方法，完成handler方法调用后，得到返回值returnValue，也就是POJO对象：

```java
public void invokeAndHandle(ServletWebRequest webRequest, ModelAndViewContainer mavContainer,
			Object... providedArgs) throws Exception {

    //反射执行handler方法，获得返回值POJO对象
    Object returnValue = invokeForRequest(webRequest, mavContainer, providedArgs);
    setResponseStatus(webRequest);
    
    //如果返回值为空，说明请求方法没有返回值，直接返回
    if (returnValue == null) {
			if (isRequestNotModified(webRequest) || getResponseStatus() != null || 
                mavContainer.isRequestHandled()) {
				disableContentCachingIfNecessary(webRequest);
				mavContainer.setRequestHandled(true);
				return;
			}
		}
    
    //如果返回值是一个字符串，那么有可能是返回页面
    else if (StringUtils.hasText(getResponseStatusReason())) {
        mavContainer.setRequestHandled(true);
        return;
    }

    mavContainer.setRequestHandled(false);
    Assert.state(this.returnValueHandlers != null, "No return value handlers");
    try {
        //返回值不是字符串，那么就会调用返回值处理器来处理返回值：
        this.returnValueHandlers.handleReturnValue(
            returnValue, getReturnValueType(returnValue), mavContainer, webRequest);
    }
    catch (Exception ex) {
        if (logger.isTraceEnabled()) {
            logger.trace(formatErrorForReturnValue(returnValue), ex);
        }
        throw ex;
    }
}
```

**handleReturnValue(returnValue, getReturnValueType(returnValue), mavContainer, webRequest);方法详解：**

```java
public void handleReturnValue(@Nullable Object returnValue, MethodParameter returnType,
			ModelAndViewContainer mavContainer, NativeWebRequest webRequest) throws Exception {

    //搜索能够处理返回值的返回值处理器
    HandlerMethodReturnValueHandler handler = selectHandler(returnValue, returnType);
    if (handler == null) {
        throw new IllegalArgumentException("Unknown return value type: " + returnType.getParameterType().getName());
    }
    //搜索到后，进行处理
    handler.handleReturnValue(returnValue, returnType, mavContainer, webRequest);
}
```

**selectHandler(returnValue, returnType);方法：**

```java
private HandlerMethodReturnValueHandler selectHandler(@Nullable Object value, MethodParameter returnType) {
    //先判断返回值是不是一个异步返回值
    boolean isAsyncValue = isAsyncReturnValue(value, returnType);
    //遍历全部的15个返回值处理器，看是否支持处理该返回值
    for (HandlerMethodReturnValueHandler handler : this.returnValueHandlers) {
        if (isAsyncValue && !(handler instanceof AsyncHandlerMethodReturnValueHandler)) {
            continue;
        }
        //调用返回值处理器的支持判断方法，进行判断，支持就返回该返回值处理器
        //这里会找到叫 RequestResponseBodyMethodProcessor的处理器，它会判断返回值是否标注有@ResponseBody注解
        if (handler.supportsReturnType(returnType)) {
            return handler;
        }
    }
    return null;
}
```

找到能处理返回值的处理器后，**如何进行返回值处理呢？**那么需要研究返回值处理器的handleReturnValue()方法，以及HTTPMessageConverter http消息转换器的原理。

##### 3、返回值处理原理

先了解两个知识：

**内容协商**

> 简单解释：
>
> 浏览器默认以请求头的方式告诉服务器，浏览器能接收什么样的媒体类型MediaType。
> 服务器最终根据自身的能力，决定服务器能产出什么样的媒体类型数据。
> 若这两者相同，那就是协商成功的内容，两边都能接收这样的媒体数据。
>
> - 基于请求头的内容协商（浏览器默认方式）：
>
>   - 内容类型有：
>     ![image-20221002022951890](..\image\image-20221002022951890.png)
>
>   - 0.9 、0.8 是媒体类型权重，权重大的优先考虑
>   - */ *代表所有类型
>
> - 基于请求参数的内容协商：
>
>   - 需要在application.yaml中开启 基于参数内容协商功能
>   - 请求url后添加参数format，例如format=json

**消息转换器HTTPMessageConverter**

> 返回值处理器，如RequestResponseBodyMethodProcessor中，
>
> 利用HTTPMessageConverter消息转换器，将此返回值类型，转为MediaType媒体类型的数据
> 例如：将Person对象转 JSON（响应），或者 JSON转Person（请求）
>
> MessageConverter规范：
> ![image-20221002032056720](..\image\image-20221002032056720.png)
>
> - canXXX()：是否支持读（ JSON转Person（请求））/写（Person对象转 JSON（响应））
> - getSupportedMediaTypes()：获取支持的所有内容类型
> - read/write：具体的读/写
>
> 默认的MessageConverter：
> ![image-20221002032512206](..\image\image-20221002032512206.png)
>
> - 0 - 只支持Byte类型的返回值数据，supports(clazz)方法只有是Byte类型返回true
> - 1 - String
> - 2 - String
> - 3 - Resource
> - 4 - ResourceRegion
> - 5 - DOMSource.class\StAXSource.class\StreamSource.class\Source.class
> - 6 - MultiValueMap
> - 7 - 所有类型，supports(clazz)直接返回true
> - 8 - 所有类型，supports(clazz)直接返回true
> - 9 - 支持注解方式xml处理的

**1、返回值处理详解：**

**handleReturnValue(returnValue, returnType, mavContainer, webRequest);方法详解：**

```java
public void handleReturnValue(@Nullable Object returnValue, MethodParameter returnType,
			ModelAndViewContainer mavContainer, NativeWebRequest webRequest)
			throws IOException, HttpMediaTypeNotAcceptableException, HttpMessageNotWritableException {

    mavContainer.setRequestHandled(true);
    //请求数据
    ServletServerHttpRequest inputMessage = createInputMessage(webRequest);
    //响应数据
    ServletServerHttpResponse outputMessage = createOutputMessage(webRequest);

    // Try even with null return value. ResponseBodyAdvice could get involved.
    // 使用消息转换器进行写出操作，将数据写为Json
    writeWithMessageConverters(returnValue, returnType, inputMessage, outputMessage);
}
```

**writeWithMessageConverters(returnValue, returnType, inputMessage, outputMessage);方法：**

```java
protected <T> void writeWithMessageConverters(@Nullable T value, MethodParameter returnType,
			ServletServerHttpRequest inputMessage, ServletServerHttpResponse outputMessage)
			throws IOException, HttpMediaTypeNotAcceptableException, HttpMessageNotWritableException {

    Object body;
    Class<?> valueType;
    Type targetType;

    //判断返回值是否为字符串
    if (value instanceof CharSequence) {
        body = value.toString();
        valueType = String.class;
        targetType = String.class;
    }
    //若不是字符串：
    else {
        body = value; //解析Json封装过程，这里就是POJO
        //返回值类型和目标类型都是POJO类型（也就是说，希望浏览器能认识这个媒体类型是一个POJO对象）
        valueType = getReturnValueType(body, returnType);
        targetType = GenericTypeResolver.resolveType(getGenericType(returnType), 
                                                     returnType.getContainingClass());
    }

    //判断是不是资源类型，也就是判断是不是一个流数据
    if (isResourceType(value, returnType)) {
        //流数据处理办法
        ... ...
    }

    //MediaType媒体类型（内容协商，浏览器默认会以请求头的方式告诉服务器它能接受什么样的媒体类型）
    MediaType selectedMediaType = null;
    //先看请求头中是否已经有这种媒体类型（也就是可能之前请求已经处理了一部分，请求头中已经有了该媒体类型）
    MediaType contentType = outputMessage.getHeaders().getContentType();
    boolean isContentTypePreset = contentType != null && contentType.isConcrete();
    if (isContentTypePreset) {
        if (logger.isDebugEnabled()) {
            logger.debug("Found 'Content-Type:" + contentType + "' in response");
        }
        //如果有，就直接设置成该媒体类型
        selectedMediaType = contentType;
    }
    //若是没有
    else {
        //获取原生request对象
        HttpServletRequest request = inputMessage.getServletRequest();
        //得到浏览器能接收的媒体类型acceptableTypes，和服务器能转化出来的所有媒体类型producibleTypes
        List<MediaType> acceptableTypes = getAcceptableMediaTypes(request);
        List<MediaType> producibleTypes = getProducibleMediaTypes(request, valueType, targetType);

        //如果返回值不是null，而没有服务器能转化出来的媒体类型，则抛出异常
        if (body != null && producibleTypes.isEmpty()) {
            throw new HttpMessageNotWritableException(
                "No converter found for return value of type: " + valueType);
        }
        //mediaTypesToUse能使用的所有媒体类型（协商成功的）
        List<MediaType> mediaTypesToUse = new ArrayList<>();
        //两层遍历浏览器能接受的媒体类型对应服务器能生产出的媒体类型
        for (MediaType requestedType : acceptableTypes) {
            for (MediaType producibleType : producibleTypes) {
                //如果两边都能接受，则将该媒体类型添加到集合里
                if (requestedType.isCompatibleWith(producibleType)) {
                    mediaTypesToUse.add(getMostSpecificMediaType(requestedType, producibleType));
                }
            }
        }
        //如果没有能用的媒体类型，抛出异常
        if (mediaTypesToUse.isEmpty()) {
            抛出异常...
        }

        //根据权重进行排序
		//例如：xml类型权重0.9比json权重0.8大，并且服务器引入了xml类型依赖，也就是服务器能生产xml类型，优先匹配xml类型
        MediaType.sortBySpecificityAndQuality(mediaTypesToUse);

        //从能用的媒体类型中挑出一个来用，最终得到媒体类型为：application/json;q=0.8
        //也就是说json媒体类型支持将POJO类型的数据以json媒体返回到浏览器中，浏览器能根据json辨识出这是个POJO对象
        for (MediaType mediaType : mediaTypesToUse) {
            //如果这个媒体类型是具体的，就用这个（一般就是排序后的第一个，权重最大）
            if (mediaType.isConcrete()) {
                selectedMediaType = mediaType;
                break;
            }
            else if (mediaType.isPresentIn(ALL_APPLICATION_MEDIA_TYPES)) {
                selectedMediaType = MediaType.APPLICATION_OCTET_STREAM;
                break;
            }
        }
        
        略...
    }
    //如果挑出来了一个确定要用的媒体类型：
    if (selectedMediaType != null) {
        selectedMediaType = selectedMediaType.removeQualityValue();
        //遍历所有的messageConverters：消息转换器，判断看哪个转换器能处理将这个返回值转成媒体类型
        for (HttpMessageConverter<?> converter : this.messageConverters) {
            
            //判断该converter是不是GenericHttpMessageConverter类型的，不是就设定为null
            GenericHttpMessageConverter genericConverter = (converter 
                                                            instanceof GenericHttpMessageConverter ?
                                                            (GenericHttpMessageConverter<?>) converter : 
                                                            null);
            
            //如果genericConverter不为null，则该converter是GenericHttpMessageConverter类型的，调用强转后的方法判断
            //如果genericConverter为null，说明不是，调用自身的canWrite()方法判断
            //最后得到，能支持POJO类型转成Json的是：MappingJackson2HttpMessageConverter
            if (genericConverter != null ?
                ((GenericHttpMessageConverter) converter).canWrite(targetType, valueType, selectedMediaType)
                : converter.canWrite(valueType, selectedMediaType)) 
            {
                //如果canWrite()返回true，说明可以将该返回值类型，也就是Class类型对象（POJO）写成JSON数据
                //拿到需要响应的内容body（POJO对象），beforeBodyWrite：写成JSON并写出去之前
                body = getAdvice().beforeBodyWrite(body, returnType, selectedMediaType,
                                                   (Class<? extends HttpMessageConverter<?>>)
                                                   converter.getClass(),
                                                   inputMessage, outputMessage);
                if (body != null) {
                    Object theBody = body;
                    //转之前加一些头数据，这里先不用管
                    LogFormatUtils.traceDebug(logger, traceOn ->
                                              "Writing [" + LogFormatUtils.formatValue(theBody, !traceOn)
                                              + "]");
                    addContentDispositionHeader(inputMessage, outputMessage);

                    //如果该converter是GenericHttpMessageConverter类型的，直接调用其write()方法写，完成响应
                    if (genericConverter != null) {
                        genericConverter.write(body, targetType, selectedMediaType, outputMessage);
                    }

                    //如果不是，调用强转成HttpMessageConverter后的write()方法写，完成响应
                    else {
                        ((HttpMessageConverter) converter).write(body, selectedMediaType, outputMessage);
                    }
                }
                else {
                    if (logger.isDebugEnabled()) {
                        logger.debug("Nothing to write: null body");
                    }
                }
                return;
            }
        }
    }

    if (body != null) {
        略...
    }
}
```

**2、CanWrite()方法详解：**

**canWrite(Class<?> clazz, @Nullable MediaType mediaType);方法：**

```java
public boolean canWrite(Class<?> clazz, @Nullable MediaType mediaType) {
    //先判断支不支持这种类型的返回值，再看能不能支持将返回值类型转成媒体类型
    return supports(clazz) && canWrite(mediaType);
}
```

**在MappingJackson2HttpMessageConverter的supports(clazz)方法中：**

```java
protected boolean supports(Class<?> clazz) {return true;}
```

直接返回true，因为该消息转换器支持所有类型的返回值。

**在MappingJackson2HttpMessageConverter的canWrite(mediaType)方法中：**

```java
protected boolean canWrite(@Nullable MediaType mediaType) {
    //看支持的媒体类型是否为空，即使为空也能支持(离谱)
    if (mediaType == null || MediaType.ALL.equalsTypeAndSubtype(mediaType)) {
        return true;
    }
    //如果不为空，例如json，则遍历该converter支持转换的所有媒体类型
    for (MediaType supportedMediaType : getSupportedMediaTypes()) {
        //如果需要转换的媒体类型和支持转换的一致，就返回true
        if (supportedMediaType.isCompatibleWith(mediaType)) {
            return true;
        }
    }
    return false;
}
```

**MappingJackson2HttpMessageConverter能支持转换的媒体类型getSupportedMediaTypes()：**

两种：

![image-20221002041455038](..\image\image-20221002041455038.png)

**2、write()方法详解：**

**genericConverter.write(body, targetType, selectedMediaType, outputMessage);方法：**

```java
public final void write(final T t, @Nullable final Type type, @Nullable MediaType contentType,
                        HttpOutputMessage outputMessage) throws IOException, HttpMessageNotWritableException {
	//拿到所有的响应头
    final HttpHeaders headers = outputMessage.getHeaders();
    //向里面添加响应头contentType，值为application/json
    addDefaultHeaders(headers, t, contentType);

    if (outputMessage instanceof StreamingHttpOutputMessage) {
        流处理...
    }
    else {
        //真正将pojo写成json的方法，并写到outputMessage中（这里面是利用底层的jackson的objectMapper转换的）
        writeInternal(t, type, outputMessage);
        //真正响应出去的方法
        //outputMessage.getBody()会获得一个OutputStream，然后flush()写出去，完成响应
        //因为响应的是Json数据，也就是响应数据而不是响应页面，因此ModelAndView直接为null，不再有后续视图解析的处理步骤
        outputMessage.getBody().flush();
    }
}
```

##### 4、响应JSON原理总结

- 1、返回值处理器判断是否支持这种类型返回值 supportsReturnType
- 2、返回值处理器调用 handleReturnValue 进行处理
- 3、例如RequestResponseBodyMethodProcessor 可以处理将返回值标了@ResponseBody注解的
  - 1、利用MessageConverters进行处理，将数据写为json
    - 1、内容协商（浏览器默认会以请求头的方式告诉服务器它能接受什么样的媒体类型）
    - 2、服务器最终根据自身能力，决定服务器能产出什么样媒体类型的数据
    - 3、SpringMVC会挨个遍历所有容器底层的 HttpMessageConverter，看谁能处理，例如：
      - 1、得到MappingJackson2HttpMessageConverter可以将返回值，也就是pojo对象写为json
      - 2、利用MappingJackson2HttpMessageConverter将json再写出去（响应）

#### 9.2 内容协商原理

> 根据客户端接收能力不同，返回不同媒体类型的数据。
>
> ![image-20221002022951890](..\image\image-20221002022951890.png)

##### 1、引入xml依赖

```xml
<!--支持返回xml类型数据-->
<dependency>
    <groupId>com.fasterxml.jackson.dataformat</groupId>
    <artifactId>jackson-dataformat-xml</artifactId>
</dependency>
```

##### 2、测试返回json和xml

- 基于请求头的内容协商（浏览器**默认**方式）：

  只需要改变请求头中Accept字段。Http协议中规定，告诉服务器本客户端可以接收的数据类型。

  可使用PostMan修改Accept字段值进行测试。

- 基于请求参数的内容协商：

  在application.yaml中开启配置，参数方式内容协商：

  ```yaml
  mvc:
      contentnegotiation:
        favor-parameter: true
  ```

  示例：localhost:8080/savePerson?format=json

##### 3、内容协商原理分析

**writeWithMessageConverters(returnValue, returnType, inputMessage, outputMessage);方法，内容协商相关部分：**

- 1、判断当前响应头中是否已经有确定的媒体类型。

  ```java
  //MediaType媒体类型（内容协商，浏览器默认会以请求头的方式告诉服务器它能接受什么样的媒体类型）
  MediaType selectedMediaType = null;
  //先看请求头中是否已经有这种媒体类型（也就是可能之前请求已经处理了一部分，请求头中已经有了该媒体类型）
  MediaType contentType = outputMessage.getHeaders().getContentType();
  boolean isContentTypePreset = contentType != null && contentType.isConcrete();
  if (isContentTypePreset) {
      if (logger.isDebugEnabled()) {
          logger.debug("Found 'Content-Type:" + contentType + "' in response");
      }
      //如果有，就直接设置成该媒体类型
      selectedMediaType = contentType;
  }
  ```

- 2、如果没有，则获取客户端支持的媒体类型。（获取客户端请求头==Accept字段==）

  例如：客户端需要【application/xml】。

  ```java
  List<MediaType> acceptableTypes = getAcceptableMediaTypes(request);
  ```

  **getAcceptableMediaTypes(request);方法中：**

  **默认使用contentNegotiationManager 内容协商管理器：**

  ```java
  private List<MediaType> getAcceptableMediaTypes(HttpServletRequest request)
  			throws HttpMediaTypeNotAcceptableException {
      return this.contentNegotiationManager.resolveMediaTypes(new ServletWebRequest(request));
  }
  ```

  **默认使用基于请求头的策略==HeaderContentNegotiationStrategy==：**

  ```java
  public List<MediaType> resolveMediaTypes(NativeWebRequest request) throws HttpMediaTypeNotAcceptableException {
      //strategy管理器，this.strategies，没开启基于请求参数策略之前，this.strategies只有一个，			  
      //就是HeaderContentNegotiationStrategy（默认基于请求头）
      //若开启基于请求参数的策略this.strategies就会有两个，遍历时如果发现请求参数中有format参数，使用的获取策略
      //就是ParameterContentNegotiationStrategy（基于请求参数）
      for (ContentNegotiationStrategy strategy : this.strategies) {
          List<MediaType> mediaTypes = strategy.resolveMediaTypes(request);
          if (mediaTypes.equals(MEDIA_TYPE_ALL_LIST)) {
              continue;
          }
          return mediaTypes;
      }
      return MEDIA_TYPE_ALL_LIST;
  }
  ```

  **默认下最后执行的就是原生request对象的获取请求头方法：**

  ```java
  public List<MediaType> resolveMediaTypes(NativeWebRequest request)
  			throws HttpMediaTypeNotAcceptableException {
  
      //获取请求头中accept字段的值
      String[] headerValueArray = request.getHeaderValues(HttpHeaders.ACCEPT);
      if (headerValueArray == null) {
          return MEDIA_TYPE_ALL_LIST;
      }
  
      List<String> headerValues = Arrays.asList(headerValueArray);
      ...
  }
  ```

  **若是基于请求参数的内容协商，获取的浏览器接收的媒体类型的策略==ParameterContentNegotiationStrategy==：**

  ```java
  public List<MediaType> resolveMediaTypes(NativeWebRequest webRequest)
  			throws HttpMediaTypeNotAcceptableException {
  	//getMediaTypeKey(webRequest)：获取请求参数key为format的值，例如json
      return resolveMediaTypeKey(webRequest, getMediaTypeKey(webRequest));
  }
  ```

  ```java
  public List<MediaType> resolveMediaTypeKey(NativeWebRequest webRequest, @Nullable String key)
  			throws HttpMediaTypeNotAcceptableException {
  
      if (StringUtils.hasText(key)) {
          //最终拿到浏览器接受的媒体类型：application/json
          MediaType mediaType = lookupMediaType(key);
          if (mediaType != null) {
              handleMatch(key, mediaType);
              return Collections.singletonList(mediaType);
          }
          mediaType = handleNoMatch(webRequest, key);
          if (mediaType != null) {
              addMapping(key, mediaType);
              return Collections.singletonList(mediaType);
          }
      }
      return MEDIA_TYPE_ALL_LIST;
  }
  ```

- 3、获取服务器能将该数据转化出来的媒体类型

  例如：服务器能将pojo转换出10种媒体类型。
  ![image-20221008234837661](..\image\image-20221008234837661.png)

  ```java
  List<MediaType> producibleTypes = getProducibleMediaTypes(request, valueType, targetType);
  ```

  **getProducibleMediaTypes(request, valueType, targetType);方法：**

  ```java
  protected List<MediaType> getProducibleMediaTypes(
  			HttpServletRequest request, Class<?> valueClass, @Nullable Type targetType) {
  
      Set<MediaType> mediaTypes =
          (Set<MediaType>) request.getAttribute(HandlerMapping.PRODUCIBLE_MEDIA_TYPES_ATTRIBUTE);
      if (!CollectionUtils.isEmpty(mediaTypes)) {
          return new ArrayList<>(mediaTypes);
      }
      else if (!this.allSupportedMediaTypes.isEmpty()) {
          List<MediaType> result = new ArrayList<>();
          //拿到所有的HttpMessageConverter，遍历调用canWrite()方法，看哪个converter能转化当前数据
          for (HttpMessageConverter<?> converter : this.messageConverters) {
              if (converter instanceof GenericHttpMessageConverter && targetType != null) {
                  if (((GenericHttpMessageConverter<?>) converter).canWrite(targetType, valueClass, null)) 
                  {
                      //如果该converter能转化，则将转换后的媒体类型放到结果集合中
                      result.addAll(converter.getSupportedMediaTypes());
                  }
              }
              else if (converter.canWrite(valueClass, null)) {
                  //如果该converter能转化，则将转换后的媒体类型放到结果集合中
                  result.addAll(converter.getSupportedMediaTypes());
              }
          }
          return result;
      }
      else {
          return Collections.singletonList(MediaType.ALL);
      }
  }
  ```

- 4、内容协商最佳匹配：获取客户端接受的同时服务端能产出的媒体类型集合（循环遍历）

  ```java
  //mediaTypesToUse能使用的所有媒体类型（协商成功的）
  List<MediaType> mediaTypesToUse = new ArrayList<>();
  //两层遍历浏览器能接受的媒体类型对应服务器能生产出的媒体类型
  for (MediaType requestedType : acceptableTypes) {
      for (MediaType producibleType : producibleTypes) {
          //如果两边都能接受，则将该媒体类型添加到集合里
          if (requestedType.isCompatibleWith(producibleType)) {
              mediaTypesToUse.add(getMostSpecificMediaType(requestedType, producibleType));
          }
      }
  }
  ```

- 5、从能用的媒体类型集合中获取一个作为最终决定的媒体类型 **selectedMediaType**

  ```java
  //根据权重进行排序
  //例如：xml类型权重0.9比json权重0.8大，并且服务器引入了xml类型依赖，也就是服务器能生产xml类型，优先匹配xml类型
  MediaType.sortBySpecificityAndQuality(mediaTypesToUse);
  
  for (MediaType mediaType : mediaTypesToUse) {
      //如果这个媒体类型是具体的，就用这个（一般就是排序后的第一个，权重最大）
      if (mediaType.isConcrete()) {
          selectedMediaType = mediaType;
          break;
      }
      else if (mediaType.isPresentIn(ALL_APPLICATION_MEDIA_TYPES)) {
          selectedMediaType = MediaType.APPLICATION_OCTET_STREAM;
          break;
      }
  }
  ```

##### 4、自定义MessageConverter

> 实现多协议数据兼容。例如：
>
> 请求同一个handler，根据客户端以及请求参数类型的不同，使用不同的MessageConverter进行内容协商
>
> - 1、浏览器发请求（原生form表单），直接返回xml    [application/xml]     jacksonXmlConverter
> - 2、ajax请求（json字符串），返回json，方便前端调用     [application/json]     jacksonJsonConverter  
> - 3、公司的app发请求，返回自定义媒体类型数据         [application/x-guigu]         xxxxConverter
>
> 步骤：
>
> 1、添加自定义的MessageConverter进系统底层
> 2、系统底层就会统计出所有MessageConverter能操作哪些类型
> 3、客户端内容协商时就会遍历到自定义MessageConverter进行操作

**回顾内容协商时MessageConverter的使用：**

- ==@ResponseBody== 响应数据，调用 **RequestResponseBodyMethodProcessor**返回值处理器处理
- 所有的 **MessageConverter** 合起来可以支持各种媒体类型数据的操作（读、写）
- 获取服务器根据返回值类型能产生哪些媒体类型数据，通过 **MessageConverter** 处理
- 具体处理返回值，并响应出去，通过 **MessageConverter** 处理

**示例：**

> 具体步骤：
> 无论要定制SpringMVC的什么功能。都是给容器中添加一个 WebMvcConfigure：public interface WebMvcConfigurer{}
> 通过实现该接口中的方法，完成相应定制
>
> - 创建配置类，用于配置web相关自定义配置
> - @Bean注册添加WebMvcConfigurer组件，该组件是SpringBoot组件，可通过实现该组件中的方法添加许多自定义组件
> - 实现configureMessageConverters方法，完成自定义MessageConverter
> - configureMessageConverters方法传入了SpringMVC中所有的HttpMessageConverter
> - 给这个converters集合中加入自定义的MessageConverter
> - 自定义MessageConverter，实现MessageConverter接口，重写所有方法

```java
@Configuration
public class WebConfig {

    @Bean
    public WebMvcConfigurer webMvcConfig(){
        return new WebMvcConfigurer() {
            @Override
            public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
                converters.add(new HttpMessageConverter<Person>() {

                    @Override
                    public boolean canRead(Class<?> clazz, MediaType mediaType) {
                        //将前端数据转为POJO对象的工作交由其他MessageConverter完成
                        return false;
                    }

                    @Override
                    public boolean canWrite(Class<?> clazz, MediaType mediaType) {
                        //只要该返回值类型是Person类型的，就可以进行媒体类型的转化
                        return clazz.isAssignableFrom(Person.class);
                    }

                    /**
                     * 服务器要统计所有MessageConverter能写出哪些内容
                     */
                    @Override
                    public List<MediaType> getSupportedMediaTypes() {
                        //自定义的媒体类型为 application/x-guigu ，格式见write方法
                        return MediaType.parseMediaTypes("application/x-guigu");
                    }

                    @Override
                    public Person read(Class<? extends Person> clazz, HttpInputMessage inputMessage) 
                        throws IOException, HttpMessageNotReadableException {
                        return null;
                    }

                    /**
                     * 自定义协议数据的写出
                     */
                    @Override
                    public void write(Person person, MediaType contentType, HttpOutputMessage outputMessage) 
                        throws IOException, HttpMessageNotWritableException {
                        //将POJO类型数据，转换成自定义媒体类型
                        //自定义媒体类型格式：Person属性，用;分隔
                        String data = person.getPersonName()+";"+person.getAge()+";"+person.getBirth()+";"+
                                person.getPet().getName()+";"+person.getPet().getAge();

                        //将数据响应出去
                        OutputStream body = outputMessage.getBody();
                        body.write(data.getBytes());
                        body.flush();
                    }
                });
            }
        };
    }
}
```

**测试：**

- 基于请求头方式：

  使用PostMan将请求头Accpet字段改成"application/guigu"，作为客户端接收的媒体类型即可；

- 基于请求参数方式：

  **由于请求参数策略ParameterContentNegotiationStrategy只支持format=json、forma=xml，**

  **因此需要自定义请求参数策略（注意，该自定义会覆盖原来的策略）：**

  ```java
  @Configuration
  public class WebConfig {
  
      @Bean
      public WebMvcConfigurer webMvcConfig(){
          return new WebMvcConfigurer() {
              @Override
              public void configureContentNegotiation(ContentNegotiationConfigurer configurer) {
                  //自定义策略需要传入的Map
                  Map<String, MediaType> mediaTypes = new HashMap<>();
  
                  //key为请求参数format的值，value为转化出的媒体类型
                  mediaTypes.put("json",MediaType.APPLICATION_JSON);
                  mediaTypes.put("xml",MediaType.APPLICATION_XML);
                  mediaTypes.put("gg",MediaType.parseMediaType("application/x-guigu"));
                  mediaTypes.put("guigu",MediaType.parseMediaType("application/x-guigu"));
                  mediaTypes.put("x-guigu",MediaType.parseMediaType("application/x-guigu"));
  
                  //自定义的基于请求参数的strategy策略，需要传入一个Map，
                  //key为请求参数format的值，value为转化出的媒体类型
                  ParameterContentNegotiationStrategy myParameterStrategy = 
                      new ParameterContentNegotiationStrategy(mediaTypes);
  
                  //使用原来默认的基于请求头的strategy策略（根据Accept请求头值获取浏览器接受的媒体类型）
                  HeaderContentNegotiationStrategy headerContentNegotiationStrategy = 
                      new HeaderContentNegotiationStrategy();
  
                  //contentNegotiationManager 内容协商管理器，调用strategies()方法，添加自定义策略（List集合）
                  //注意！此时SpringMVC以添加的该自定义策略（集合）为准，相当于覆盖了原来的策略（集合）
                  ArrayList<ContentNegotiationStrategy> myStrategies = new ArrayList<>();
                  myStrategies.add(myParameterStrategy); //此时集合中只有一个策略，是自定义基于请求参数的
                  //加上原来默认的基于请求头策略，才可以继续使用默认请求头方式
                  myStrategies.add(headerContentNegotiationStrategy); 
                  configurer.strategies(myStrategies);
              }
          };
      }
  }
  ```

### 十、视图解析与模板引擎

> SpringBoot默认不支持JSP，需要引入第三方模板引擎技术实现页面渲染。

#### 10.1 视图解析

![image-20221009164044646](..\image\image-20221009164044646.png)

#### 10.2 模板引擎-Thymeleaf

> Thymeleaf 是一款用于渲染 XML/XHTML/HTML5 内容的模板引擎。它与 JSP，Velocity，FreeMaker  等模板引擎类似，也可以轻易地与 Spring MVC 等 Web 框架集成。与其它模板引擎相比，Thymeleaf 最大的特点是，即使不启动  Web 应用，也可以直接在浏览器中打开并正确显示模板页面 。

**快速自学入门地址：**

http://c.biancheng.net/spring_boot/thymeleaf.html

**SpringBoot中使用Thymeleaf：**

- 引入依赖：

  ```xml
  <!--Thymeleaf模板引擎-->
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-thymeleaf</artifactId>
  </dependency>
  ```

- 页面跳转时，Thymeleaf默认前缀为 /resources/templates/，需要自己创建templates包，放置html页面，返回的字符串不加前缀

- 页面跳转时，默认后缀为 .html ，因此返回的字符串不用加 .html后缀

#### 10.3 视图解析原理

**重定向视图解析过程：**

**1、**返回值处理过程中（响应页面），所有数据**（（Model、Session、请求参数中的自定义类型对象）和 视图地址 ）**都会被放在 **ModelAndViewContainer** 里面。

在handleReturnValue()方法中：

```java
public void handleReturnValue(@Nullable Object returnValue, MethodParameter returnType,
			ModelAndViewContainer mavContainer, NativeWebRequest webRequest) throws Exception {

    //先获取处理该返回值的处理器，是：ViewNameMethodReturnValueHandler
    HandlerMethodReturnValueHandler handler = selectHandler(returnValue, returnType);
    if (handler == null) {
        throw new IllegalArgumentException("Unknown return value type: " +
                                           returnType.getParameterType().getName());
    }
    handler.handleReturnValue(returnValue, returnType, mavContainer, webRequest);
}
```

ViewNameMethodReturnValueHandler调用handleReturnValue(returnValue, returnType, mavContainer, webRequest);方法：

```java
public void handleReturnValue(@Nullable Object returnValue, MethodParameter returnType,
			ModelAndViewContainer mavContainer, NativeWebRequest webRequest) throws Exception {

    //判断返回值是否是一个字符序列（字符串String属于字符序列），是，说明请求是响应页面
    if (returnValue instanceof CharSequence) {
        //获取视图地址
        String viewName = returnValue.toString();
        //将视图地址放到ModelAndViewContainer中
        mavContainer.setViewName(viewName);
        //判断返回的视图，是否是一个重定向的视图
        if (isRedirectViewName(viewName)) {
            //如果是，设置重定向传感器为true
            mavContainer.setRedirectModelScenario(true);
        }
    }
    else if (returnValue != null) {
        //如果返回值不是字符序列，又是响应页面的，这样的情况时不应该发生的
        // should not happen
        throw new UnsupportedOperationException("Unexpected return type: " +
                                                returnType.getParameterType().getName() + 
                                                " in method: " + returnType.getMethod());
    }
}
```

isRedirectViewName(viewName)判断：

```java
protected boolean isRedirectViewName(String viewName) {
    //只要视图地址前加上了redirect: ，就会被作为重定向处理
    return (PatternMatchUtils.simpleMatch(this.redirectPatterns, viewName) ||
            viewName.startsWith("redirect:"));
}
```

回到invokeHandlerMethod(request, response, handlerMethod)方法中：

```java
protected ModelAndView invokeHandlerMethod(HttpServletRequest request,
      HttpServletResponse response, HandlerMethod handlerMethod) throws Exception {

    ServletWebRequest webRequest = new ServletWebRequest(request, response);
    try {
        WebDataBinderFactory binderFactory = getDataBinderFactory(handlerMethod);
        ModelFactory modelFactory = getModelFactory(handlerMethod, binderFactory);

        ServletInvocableHandlerMethod invocableMethod = createInvocableHandlerMethod(handlerMethod);
        
        //argumentResolvers：参数解析器
        if (this.argumentResolvers != null) {
            invocableMethod.setHandlerMethodArgumentResolvers(this.argumentResolvers);
        }
        //returnValueHandlers：返回值处理器
        if (this.returnValueHandlers != null) {
            invocableMethod.setHandlerMethodReturnValueHandlers(this.returnValueHandlers);
        }
        
        省略其他invocableMethod的封装过程等...

        //最核心的一步，执行目标方法：（此时目标方法 invocableMethod 已经封装完毕）
        //真正执行目标方法就在这个方法里面：
        invocableMethod.invokeAndHandle(webRequest, mavContainer);
        
        if (asyncManager.isConcurrentHandlingStarted()) {
            return null;
        }
		
        //返回 ModelAndView 给前端控制器 DisPatcherServlet
        return getModelAndView(mavContainer, modelFactory, webRequest);
    }
    finally {
        webRequest.requestCompleted();
    }
}
```

**getModelAndView(mavContainer, modelFactory, webRequest);方法：**

```java
private ModelAndView getModelAndView(ModelAndViewContainer mavContainer,
			ModelFactory modelFactory, NativeWebRequest webRequest) throws Exception {
	//更新model
    modelFactory.updateModel(webRequest, mavContainer);
    if (mavContainer.isRequestHandled()) {
        return null;
    }
    //从ModelAndViewContainer中获取视图地址、model数据，放置到ModelAndView对象中返回，
    //最终返回给前端控制器DisPatcherServelt
    ModelMap model = mavContainer.getModel();
    ModelAndView mav = new ModelAndView(mavContainer.getViewName(), model, mavContainer.getStatus());
    if (!mavContainer.isViewReference()) {
        mav.setView((View) mavContainer.getView());
    }
    if (model instanceof RedirectAttributes) {
        Map<String, ?> flashAttributes = ((RedirectAttributes) model).getFlashAttributes();
        HttpServletRequest request = webRequest.getNativeRequest(HttpServletRequest.class);
        if (request != null) {
            RequestContextUtils.getOutputFlashMap(request).putAll(flashAttributes);
        }
    }
    return mav;
}
```

**2、任何目标方法执行完成以后都会返回 ModelAndView（数据和视图地址），若是响应数据，则mav为null。**

回到DisPatcherServelt的doDispatcher()方法中：

```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
    HttpServletRequest processedRequest = request;
    HandlerExecutionChain mappedHandler = null;
    boolean multipartRequestParsed = false;

    WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);

    try {
        ModelAndView mv = null;
        Exception dispatchException = null;

        try {
            省略之前解析过的步骤，映射处理（HandlerMapping）、适配器处理（HandlerAdapter）...
            
            // Actually invoke the handler.
            // 适配器处理handler方法后，返回得到ModelAndView
            mv = ha.handle(processedRequest, response, mappedHandler.getHandler());

            if (asyncManager.isConcurrentHandlingStarted()) {
                return;
            }
			//设置默认的视图地址（如果没有返回的视图地址，设置默认的视图地址为 当前请求路径，如路径：/login，视图：login）
            applyDefaultViewName(processedRequest, mv);
            mappedHandler.applyPostHandle(processedRequest, response, mv);
        }
        catch (Exception ex) {
            ...
        }
        catch (Throwable err) {
            ...
        }
        //真正处理派发结果
        processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
    }
    catch (Exception ex) {
        ...
    }
    finally {
        ...
    }
}
```

**3、前端控制器调用processDispatchResult() 处理派发结果（页面该如何响应）**

- 1、**render**(**mv**, request, response); 进行页面渲染逻辑：

  - 1、resolveViewName(); 根据视图地址得到 **View **对象（View：定义了页面的渲染逻辑）：

    - 1、遍历==视图解析器==，看哪个==视图解析器==能根据当前视图地址得到View对象
    - 2、遍历时包含内容协商，最终的到视图对象 view

  - 2、使用得到的视图对象view，调用自身render()方法进行页面渲染：

    例如：得到**重定向**的视图对象 **RedirectView**

    - 1、获取目标url地址
    - 2、最终就是调用原生Servlet的**sendRedirect(encodedURL)**方法，进行重定向

**processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);方法：**

```java
private void processDispatchResult(HttpServletRequest request, HttpServletResponse response,
			@Nullable HandlerExecutionChain mappedHandler, @Nullable ModelAndView mv,
			@Nullable Exception exception) throws Exception {

    boolean errorView = false;

    if (exception != null) {
        if (exception instanceof ModelAndViewDefiningException) {
            logger.debug("ModelAndViewDefiningException encountered", exception);
            mv = ((ModelAndViewDefiningException) exception).getModelAndView();
        }
        else {
            Object handler = (mappedHandler != null ? mappedHandler.getHandler() : null);
            mv = processHandlerException(request, response, handler, exception);
            errorView = (mv != null);
        }
    }

    // Did the handler return a view to render?
    if (mv != null && !mv.wasCleared()) {
        //进行页面渲染
        render(mv, request, response);
        if (errorView) {
            WebUtils.clearErrorRequestAttributes(request);
        }
    }
    else {
        if (logger.isTraceEnabled()) {
            logger.trace("No view rendering, null ModelAndView returned.");
        }
    }

    if (WebAsyncUtils.getAsyncManager(request).isConcurrentHandlingStarted()) {
        // Concurrent handling started during a forward
        return;
    }

    if (mappedHandler != null) {
        // Exception (if any) is already handled..
        //页面渲染成功后，调用所有拦截的afterCompletion()
        mappedHandler.triggerAfterCompletion(request, response, null);
    }
}
```

**render(mv, request, response);方法：**

```java
protected void render(ModelAndView mv, HttpServletRequest request, HttpServletResponse response) throws Exception {
    // Determine locale for request and apply it to the response.
    Locale locale =
        (this.localeResolver != null ? this.localeResolver.resolveLocale(request) : request.getLocale());
    response.setLocale(locale);

    View view;
    //拿到视图地址
    String viewName = mv.getViewName();
    if (viewName != null) {
        // We need to resolve the view name.
        //根据视图地址得到View对象
        view = resolveViewName(viewName, mv.getModelInternal(), locale, request);
        if (view == null) {
            throw new ServletException("Could not resolve view with name '" + mv.getViewName() +
                                       "' in servlet with name '" + getServletName() + "'");
        }
    }
    else {
        // No need to lookup: the ModelAndView object contains the actual View object.
        view = mv.getView();
        if (view == null) {
            throw new ServletException("ModelAndView [" + mv + "] neither contains a view name nor a " +
                                       "View object in servlet with name '" + getServletName() + "'");
        }
    }

    // Delegate to the View object for rendering.
    if (logger.isTraceEnabled()) {
        logger.trace("Rendering view [" + view + "] ");
    }
    try {
        if (mv.getStatus() != null) {
            response.setStatus(mv.getStatus().value());
        }
        // 使用得到的视图对象view，调用自身render()方法进行页面渲染
        view.render(mv.getModelInternal(), request, response);
    }
    catch (Exception ex) {
        if (logger.isDebugEnabled()) {
            logger.debug("Error rendering view [" + view + "]", ex);
        }
        throw ex;
    }
}
```

**resolveViewName(viewName, mv.getModelInternal(), locale, request);方法：**

```java
protected View resolveViewName(String viewName, @Nullable Map<String, Object> model,
			Locale locale, HttpServletRequest request) throws Exception {

    if (this.viewResolvers != null) {
        //遍历视图解析器，看哪个视图解析器能根据当前视图地址得到View对象
        for (ViewResolver viewResolver : this.viewResolvers) {
            View view = viewResolver.resolveViewName(viewName, locale);
            if (view != null) {
                return view;
            }
        }
    }
    return null;
}
```

**以RedirectView视图对象为例，自身view.render(mv.getModelInternal(), request, response);方法中：**

```java
public void render(@Nullable Map<String, ?> model, HttpServletRequest request,
			HttpServletResponse response) throws Exception {

    if (logger.isDebugEnabled()) {
        logger.debug("View " + formatViewName() +
                     ", model " + (model != null ? model : Collections.emptyMap()) +
                     (this.staticAttributes.isEmpty() ? "" : ", static attributes " + this.staticAttributes));
    }

    Map<String, Object> mergedModel = createMergedOutputModel(model, request, response);
    prepareResponse(request, response);
    //真正执行渲染
    renderMergedOutputModel(mergedModel, getRequestToExpose(request), response);
}
```

**renderMergedOutputModel(mergedModel, getRequestToExpose(request), response);方法：**

```java
protected void renderMergedOutputModel(Map<String, Object> model, HttpServletRequest request,
			HttpServletResponse response) throws IOException {

    //获取目标Url
    String targetUrl = createTargetUrl(model, request);
    targetUrl = updateTargetUrl(targetUrl, model, request, response);

    // Save flash attributes
    RequestContextUtils.saveOutputFlashMap(targetUrl, request, response);

    // Redirect
    //进行重定向
    sendRedirect(request, response, targetUrl, this.http10Compatible);
}
```

**sendRedirect(request, response, targetUrl, this.http10Compatible);方法：**

```java
protected void sendRedirect(HttpServletRequest request, HttpServletResponse response,
			String targetUrl, boolean http10Compatible) throws IOException {

    String encodedURL = (isRemoteHost(targetUrl) ? targetUrl : response.encodeRedirectURL(targetUrl));
    if (http10Compatible) {
        HttpStatus attributeStatusCode = (HttpStatus) request.getAttribute(View.RESPONSE_STATUS_ATTRIBUTE);
        if (this.statusCode != null) {
            response.setStatus(this.statusCode.value());
            response.setHeader("Location", encodedURL);
        }
        else if (attributeStatusCode != null) {
            response.setStatus(attributeStatusCode.value());
            response.setHeader("Location", encodedURL);
        }
        else {
            // Send status code 302 by default.
            //最终就是调用原生Servlet的sendRedirect()方法，进行重定向
            response.sendRedirect(encodedURL);
        }
    }
    else {
        HttpStatus statusCode = getHttp11StatusCode(request, response, targetUrl);
        response.setStatus(statusCode.value());
        response.setHeader("Location", encodedURL);
    }
}
```

### 十一、拦截器

#### 11.1 拦截器介绍

> SpringMVC提供的拦截器类似于Servlet-api中的过滤器，可以对控制器的请求进行拦截实现相关的预处理和后处理。
> **拦截器并不能取代过滤器**

- 过滤器
  - 是Servlet规范的一部分，所有的web项目都可以使用
  - 过滤器在web.xml配置（可以使用注解），能够拦截所有web请求
- 拦截器
  - 是SpringMVC框架的实现，只有在SpringMVC框架中才能使用
  - 拦截器在SpringMVC配置文件中配置，不会拦截SpringMVC放行的静态资源（jsp\html\css...）

#### 11.2 自定义拦截器

**1. 创建拦截器**

> **HandlerInterceptor接口规范：**
>
> ![image-20221010170100191](..\image\image-20221010170100191.png)
>
> - preHandler()：目标方法执行前
> - postHandler()：目标方法执行后
> - afterCompletion()：页面渲染完成后

```java
public class LoginInterceptor implements HandlerInterceptor {
    /**
     * 登录检查
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, 
                             Object handler) throws Exception {
        //如果Session中存在登录用户对象，则放行
        HttpSession session = request.getSession();
        if(session.getAttribute("loginUser") != null){
            return true;
        }
        //否则跳转回首页
        session.setAttribute("NotLoggedIn","请先登录，再访问管理员页面！");
        response.sendRedirect("/");
        return false;
    }
}
```

**2. 配置拦截器**

> 方法：注册组件WebMvcConfigure，一个拦截器可以配置多个拦截路径和放行路径

```java
@Configuration
public class WebConfig {

    @Bean
    public WebMvcConfigurer webMvcConfig(){
        return new WebMvcConfigurer() {
            /**
             * 配置自定义拦截器
             */
            @Override
            public void addInterceptors(InterceptorRegistry registry) {
                registry.addInterceptor(new LoginInterceptor())
                        .addPathPatterns("/**") //拦截的路径 /**拦截所有请求，包括静态
                        .excludePathPatterns("/","/toLogin","/login"); //放行的路径（静态资源放行）
            }
        };
    }
}
```

#### 11.3 拦截器链

> 将多个拦截器按照一定顺序构成一条执行链

![image-20220508195646787](..\image\image-20220508195646787.png)

```java
@Configuration
public class WebConfig {

    @Bean
    public WebMvcConfigurer webMvcConfig(){
        return new WebMvcConfigurer() {
            /**
             * 配置自定义拦截器
             */
            @Override
            public void addInterceptors(InterceptorRegistry registry) {
                //同一路径多个拦截器，形成拦截器链，拦截顺序就是添加顺序
                registry.addInterceptor(new TestInterceptor1())
                        .addPathPatterns("/**")
                        .excludePathPatterns("/","/toLogin","/login", "/static/**");
                registry.addInterceptor(new TestInterceptor2())
                        .addPathPatterns("/**")
                        .excludePathPatterns("/","/toLogin","/login", "/static/**");
                registry.addInterceptor(new LoginInterceptor())
                        .addPathPatterns("/**") //拦截的路径 /**拦截所有请求，包括静态
                        .excludePathPatterns("/","/toLogin","/login", "/static/**"); //放行的路径（静态资源放行）
            }
        };
    }
}
```

#### 11.4 拦截器原理

**原理解析：**

1、根据当前请求，调用getHandler()方法，得到mappedHandler对象，该对象中包含需要调用的handler方法，以及handler的拦截器链

2、先**顺序执行**所有拦截器的 **preHandle()**方法：

- 1、如果当前拦截器preHandle()返回为true，则执行下一个拦截器的preHandle()
- 2、如果返回false。直接 **倒序执行**所有已经执行了的拦截器的 **afterCompletion()**

**3、如果任何一个拦截器preHandle()返回false，直接跳出，不执行目标handler方法**

**4、所有拦截器preHandle()都返回true，执行目标handler方法**

**5、执行成功目标handler方法后，倒序执行所有拦截器postHandle()**

6、前面的步骤有任何异常都会直接触发 afterCompletion()

7、页面渲染成功后，倒序触发 afterCompletion()

**源码过程：**

在DispatcherServlet的doDispatch()方法中：

```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
   ... ...
    try {
        ModelAndView mv = null;
        Exception dispatchException = null;

        try {
            processedRequest = checkMultipart(request);
            multipartRequestParsed = (processedRequest != request);

            // 根据请求获取HandlerExecutionChain调用链，包含了handler目标方法、拦截器链
            mappedHandler = getHandler(processedRequest);
            if (mappedHandler == null) {
                noHandlerFound(processedRequest, response);
                return;
            }

            // 根据这个调用链拿到处理它的适配器
            HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());
            ...

            //在调用适配器的处理方法前，先调用所有拦截器的preHandle()，一旦返回false，直接return，不执行目标handler方法
            if (!mappedHandler.applyPreHandle(processedRequest, response)) {
                return;
            }

            //所有拦截器的preHandle()放行后，才真正执行目标handler方法
            mv = ha.handle(processedRequest, response, mappedHandler.getHandler());

            if (asyncManager.isConcurrentHandlingStarted()) {
                return;
            }
			
            applyDefaultViewName(processedRequest, mv);
            //成功调用目标handler方法后，调用所有拦截器的postHandle()
            mappedHandler.applyPostHandle(processedRequest, response, mv);
        }
        catch (Exception ex) {
            dispatchException = ex;
        }
        catch (Throwable err) {
            dispatchException = new NestedServletException("Handler dispatch failed", err);
        }
        //处理派发结果，在该方法里面，页面渲染成功后，调用所有拦截的afterCompletion()
        processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
    }
    catch (Exception ex) {
        //一旦出现异常，立刻触发拦截器的afterCompletion()
        triggerAfterCompletion(processedRequest, response, mappedHandler, ex);
    }
    catch (Throwable err) {
        //一旦出现异常，立刻触发拦截器的afterCompletion()
        triggerAfterCompletion(processedRequest, response, mappedHandler,
                               new NestedServletException("Handler processing failed", err));
    }
    finally {
        ... ...
    }
}
```

**HandlerExecutionChain调用链 mappedHandler对象：**

![image-20221010211751452](..\image\image-20221010211751452.png)

**mappedHandler.applyPreHandle(processedRequest, response)；方法：**

```java
boolean applyPreHandle(HttpServletRequest request, HttpServletResponse response) throws Exception {
    HandlerInterceptor[] interceptors = getInterceptors();
    if (!ObjectUtils.isEmpty(interceptors)) {
        //顺序执行所有拦截器的preHandle()方法
        for (int i = 0; i < interceptors.length; i++) {
            HandlerInterceptor interceptor = interceptors[i];
            if (!interceptor.preHandle(request, response, this.handler)) {
                //如果返回false。直接倒序执行所有已经执行了的拦截器的 afterCompletion()
                triggerAfterCompletion(request, response, null);
                return false;
            }
            this.interceptorIndex = i;
        }
    }
    return true;
}
```

**applyPostHandle(processedRequest, response, mv);方法：**

```java
void applyPostHandle(HttpServletRequest request, HttpServletResponse response, @Nullable ModelAndView mv)
			throws Exception {

    HandlerInterceptor[] interceptors = getInterceptors();
    if (!ObjectUtils.isEmpty(interceptors)) {
        //倒序执行所有拦截器postHandle()
        for (int i = interceptors.length - 1; i >= 0; i--) {
            HandlerInterceptor interceptor = interceptors[i];
            interceptor.postHandle(request, response, this.handler, mv);
        }
    }
}
```

**triggerAfterCompletion(request, response, null);方法：**

```java
void triggerAfterCompletion(HttpServletRequest request, HttpServletResponse response, 
                            @Nullable Exception ex)throws Exception {

    HandlerInterceptor[] interceptors = getInterceptors();
    if (!ObjectUtils.isEmpty(interceptors)) {
        //倒序执行所有已经执行了的拦截器的 afterCompletion()
        for (int i = this.interceptorIndex; i >= 0; i--) {
            HandlerInterceptor interceptor = interceptors[i];
            try {
                interceptor.afterCompletion(request, response, this.handler, ex);
            }
            catch (Throwable ex2) {
                logger.error("HandlerInterceptor.afterCompletion threw exception", ex2);
            }
        }
    }
}
```

### 十二、文件上传

#### 12.1 文件上传实现

**文件上传相关配置：**

```yaml
spring:
  servlet:
    multipart:
      max-file-size: 10MB #单个文件上传大小
      max-request-size: 100MB #多个文件上传总大小
```

**前端原生表单提交：**

```html
<h2>文件上传</h2>
<form action="/upload" method="post" enctype="multipart/form-data">
    用户名<input type="text"  value="张三" name="userName"/><br>
    密码<input type="password" value="123" name="password"/><br>
    用户头像<input type="file" name="userImageFile"/><br>
    用户照片<input type="file" name="userPhotosFile" multiple/><br>
    <input type="submit" value="提交"/>
</form>
```

**文件上传Controller：**

> 请求参数：
>
> - 单个文件参数：使用 @RequestPart注解 +  MultipartFile
> - 多个文件参数：使用 @RequestPart注解 +  MultipartFile[]
>
> 保存文件之前，可以对文件名进行处理，以防重复（例如加上时间戳）
>
> 保存文件到磁盘：
>
> - 获取流，按喜欢的方式处理：multipartFile.getInputStream()
> - 使用封装好的方法，传入File对象即可：multipartFile.transferTo( new File ( savePath ) )

```java
@Log4j2
@Controller
public class FileUpLoadController {

    @PostMapping("/upload")
    public String userMessageUpload(Model model,
                                    User user,
                                    @RequestPart MultipartFile userImageFile,
                                    @RequestPart MultipartFile[] userPhotosFile) throws IOException {

        log.info("上传信息：userName={},password={},userImageSize={},userPhotosLength={}",
                user.getUserName(),user.getPassword(),userImageFile.getSize(),userPhotosFile.length);

        //文件保存在服务器的绝对路径
        String dir = "D:\\JavaProject\\projects\\SpringBootLearning\\src\\main\\resources\\static\\images";
        //如果用户头像不为空，进行处理：
        if(!userImageFile.isEmpty()){
            //1. 处理上传文件名，只保留文件名后缀，以当前时间和用户名作为文件名
            String originalFilename = userImageFile.getOriginalFilename();
            //后缀
            String ext = originalFilename.substring(originalFilename.lastIndexOf("."));
            //文件名
            String imageName = System.currentTimeMillis() + user.getUserName() + ext;

            //2. 确定用户头像保存的绝对路径
            String savePath = dir + "/" + imageName;

            //3. 将用户头像保存到硬盘
            userImageFile.transferTo(new File(savePath));

            //4. 将用户头像url保存到用户对象中
            user.setUserImage("images/" + imageName); //保存到数据库过程省略
        }

        //如果用户照片不为空，进行处理：
        if(userPhotosFile.length > 0){
            //逐张处理：
            for (MultipartFile userPhoto : userPhotosFile) {
                if(!userPhoto.isEmpty()){
                    //1. 处理上传文件名，只保留文件名后缀，以当前时间和用户名作为文件名
                    String originalFilename = userPhoto.getOriginalFilename();
                    //后缀
                    String ext = originalFilename.substring(originalFilename.lastIndexOf("."));
                    //文件名
                    String photoName = System.currentTimeMillis() + user.getUserName() + ext;

                    //2. 确定用户照片保存的绝对路径
                    String savePath = dir + "/" + photoName;

                    //3. 将用户照片保存到硬盘
                    userPhoto.transferTo(new File(savePath));

                    //4. 将用户照片url保存到服务器中，略...
                }
            }
        }
        model.addAttribute("upload","上传成功!");
        return "index";
    }
}
```

#### 12.2 文件上传原理

**自动配置原理：**

文件上传自动配置类 - **MultipartAutoConfiguration** 绑定了 **MultipartProperties**（可以在yaml进行相应配置）

- 自动配好了 **StandardServletMultipartResolver** 【文件上传解析器】

**请求参数处理流程：**

- **1、请求进来使用文件上传解析器判断并封装文件上传请求（返回StandardMultipartHttpServletRequest）**
- **2、参数解析器来解析请求中的文件内容，封装成MultipartFile**
- 3、将request中文件信息封装为一个Map；
- 4、最终执行doInvoke(args)完成目标handler执行

**doDispatch()方法中：**

```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
    HttpServletRequest processedRequest = request;
    HandlerExecutionChain mappedHandler = null;
    //用于判断文件上传请求是否已解析
    boolean multipartRequestParsed = false;

    WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);

    try {
        ModelAndView mv = null;
        Exception dispatchException = null;

        try {
            //处理请求之前，checkMultipart()中使用文件上传解析器，判断当前请求是不是一个文件上传请求，
            //如果是，会对request进行包装
            processedRequest = checkMultipart(request);
            //如果两个对象不等，说明request被包装了，multipartRequestParsed值为true，表示文件上传请求已被解析
            multipartRequestParsed = (processedRequest != request);

            //获取handler调用链
            mappedHandler = getHandler(processedRequest);
            if (mappedHandler == null) {
                noHandlerFound(processedRequest, response);
                return;
            }

            //获取处理器适配器
            HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());

            ...

            //执行目标handler方法
            mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
            ...
        }
        ...
    }
    ...
    finally {
        ...
        else {
            // Clean up any resources used by a multipart request.
            if (multipartRequestParsed) {
                cleanupMultipart(processedRequest);
            }
        }
    }
}
```

**checkMultipart(request);方法：**

```java
protected HttpServletRequest checkMultipart(HttpServletRequest request) throws MultipartException {
    //使用文件上传解析器multipartResolver（默认只有一个，就是StandardServletMultipartResolver）
    //判断当前请求是不是一个文件上传请求，如果是，并且文件上传解析器并不为空，则进行解析
    if (this.multipartResolver != null && this.multipartResolver.isMultipart(request)) {
        if (WebUtils.getNativeRequest(request, MultipartHttpServletRequest.class) != null) {
            ...
        }
        else if (hasMultipartException(request)) {
            ...
        }
        else {
            try {
                //解析文件上传请求，然后返回解析后的请求对象（进行包装）
                return this.multipartResolver.resolveMultipart(request);
            }
            catch (MultipartException ex) {
                ...
            }
        }
    }
    // If not returned before: return original request.
    return request;
}
```

**在参数解析的，resolveArgument()方法中：**

```java
public Object resolveArgument(MethodParameter parameter, @Nullable ModelAndViewContainer mavContainer,
			NativeWebRequest webRequest, @Nullable WebDataBinderFactory binderFactory) throws Exception {

    //获取能处理MultipartFile类型参数的参数解析器，最终拿到的解析器为：RequestPartMethodArgumentResolver
    //标了@RequestPart注解的
    HandlerMethodArgumentResolver resolver = getArgumentResolver(parameter);
    if (resolver == null) {
        throw new IllegalArgumentException("Unsupported parameter type [" +
                                           parameter.getParameterType().getName() + "]. 
                                           supportsParameter should be called first.");
    }
    //解析MultipartFile类型参数                                   
    return resolver.resolveArgument(parameter, mavContainer, webRequest, binderFactory);
}
```

### 十三、异常处理机制

#### 13.1 错误处理

**默认规则：**

- **默认情况下，SpringBoot提供/error处理所有错误的映射**
- 对于**机器客户端**（非浏览器，例如PostMan），它将生成**JSON**响应，其中包含错误，HTTP状态和异常消息的详细信息。对于**浏览器**客户端，响应一个**“whitelabel”错误视图**，以**HTML格式**呈现相同的数据
- **要对其进行自定义，添加View解析为error**
- 要完成替换默认行为，可以实现ErrorController并注册该类型的Bean定义，或添加ErrorAttributes类型的组件以使用现有机制但替换其内容
- **static或templates目录下的error目录中的 4xx.html 和 5xx.html会被自动解析**

#### 13.2 异常处理原理

**1、错误处理自动配置原理**

- **ErrorMvcAutoConfiguration 自动配置异常处理规则**

  - **容器中的组件：类型：DefaultErrorAttribute -> id：errorAttributes （处理器异常解析器）** 

    - **定义错误页面中可以包含哪些数据**

    - ```java
      @Bean
      @ConditionalOnMissingBean(value = ErrorAttributes.class, search = SearchStrategy.CURRENT)
      public DefaultErrorAttributes errorAttributes() {
         return new DefaultErrorAttributes();
      }
      ```

  - **容器中的组件：类型：BasicErrorController -> id：basicErrorController**

    - **默认处理 /error 路径的请求**
    - **容器中有组件 View -> id是error**；（响应默认错误页）
    - 容器中有组件 **BeanNameViewResolver（视图解析器）：按照返回的视图名作为组件的id去容器中找View对象**

    - ```java
      @Bean
      @ConditionalOnMissingBean(value = ErrorController.class, search = SearchStrategy.CURRENT)
      public BasicErrorController basicErrorController(ErrorAttributes errorAttributes,
                                                       ObjectProvider<ErrorViewResolver> errorViewResolvers) {
          return new BasicErrorController(errorAttributes, this.serverProperties.getError(),
                                          errorViewResolvers.orderedStream().collect(Collectors.toList()));
      }
      ```

  - **容器中的组件：类型：DefaultErrorViewResolver** -> id：conventionErrorViewResolver

    - 如果发生错误，会以HTTP的状态码作为视图页地址（viewName），找到真正的页面
    - error/404.html、error/5xx.html

    - ```java
      @Configuration(proxyBeanMethods = false)
      static class DefaultErrorViewResolverConfiguration {
      
          private final ApplicationContext applicationContext;
      
          private final ResourceProperties resourceProperties;
      
          DefaultErrorViewResolverConfiguration(ApplicationContext applicationContext,
                                                ResourceProperties resourceProperties) {
              this.applicationContext = applicationContext;
              this.resourceProperties = resourceProperties;
          }
      
          @Bean
          @ConditionalOnBean(DispatcherServlet.class)
          @ConditionalOnMissingBean(ErrorViewResolver.class)
          DefaultErrorViewResolver conventionErrorViewResolver() {
              return new DefaultErrorViewResolver(this.applicationContext, this.resourceProperties);
          }
      
      }
      ```

  - **总结：如果想要返回页面，就会找error视图。（默认是一个白页[StaticView]）**

**2、异常处理步骤流程**

doDispatch()方法中：

**1、**执行目标handler方法，目标方法执行期间有任何异常，都会被catch，并且用**dispatchException**封装，而且会标志当前请求结束

**2、**进入视图解析流程（页面渲染）：
processDispatchResult(processedRequest, response, mappedHandler, **mv**, **dispatchException**);

目标方法执行异常，mv直接为null，processDispatchResult()方法解析时，会获取参数异常信息**dispatchException**

```java
private void processDispatchResult(HttpServletRequest request, HttpServletResponse response,
			@Nullable HandlerExecutionChain mappedHandler, @Nullable ModelAndView mv,
			@Nullable Exception exception) throws Exception {

    boolean errorView = false;

    //当传入的exception不为空，则进行处理
    if (exception != null) {
        if (exception instanceof ModelAndViewDefiningException) {
            logger.debug("ModelAndViewDefiningException encountered", exception);
            mv = ((ModelAndViewDefiningException) exception).getModelAndView();
        }
        else {
            Object handler = (mappedHandler != null ? mappedHandler.getHandler() : null);
            //处理执行handler时发生的异常
            mv = processHandlerException(request, response, handler, exception);
            errorView = (mv != null);
        }
    }
    ... ... 
}
```

**3、processHandlerException(request, response, handler, exception);处理handler发生的异常：**

处理完成，返回ModelAndView：

- **遍历所有的HandlerExceptionResolver处理器异常解析器**，看谁能处理当前异常

- 系统默认的异常解析器：其中就包含了**DefaultErrorAttributes**

  <img src="..\image\image-20221011222129799.png" alt="image-20221011222129799" style="zoom:67%;" />

- **DefaultErrorAttributes先来处理异常，把异常信息保存到request域中，并且返回null**

- **如果没有任何异常处理器能处理异常，异常会被抛出（例如 by zero异常）**

- **抛出后没有任何人处理异常，最终底层就会转发到/error请求，被BasicErrorController 处理。**

```java
protected ModelAndView processHandlerException(HttpServletRequest request, HttpServletResponse response,
			@Nullable Object handler, Exception ex) throws Exception {

    // Success and error responses may use different content types
    request.removeAttribute(HandlerMapping.PRODUCIBLE_MEDIA_TYPES_ATTRIBUTE);

    // Check registered HandlerExceptionResolvers...
    ModelAndView exMv = null;
    if (this.handlerExceptionResolvers != null) {
        //遍历handler异常处理器，看谁能处理该异常，处理成功则返回ModelAndView
        for (HandlerExceptionResolver resolver : this.handlerExceptionResolvers) {
            exMv = resolver.resolveException(request, response, handler, ex);
            if (exMv != null) {
                break;
            }
        }
    }
    //当异常处理器处理成功返回了ModelAndView：
    if (exMv != null) {
        if (exMv.isEmpty()) {
            request.setAttribute(EXCEPTION_ATTRIBUTE, ex);
            return null;
        }
        // We might still need view name translation for a plain error model...
        if (!exMv.hasView()) {
            String defaultViewName = getDefaultViewName(request);
            if (defaultViewName != null) {
                exMv.setViewName(defaultViewName);
            }
        }
        if (logger.isTraceEnabled()) {
            logger.trace("Using resolved error view: " + exMv, ex);
        }
        else if (logger.isDebugEnabled()) {
            logger.debug("Using resolved error view: " + exMv);
        }
        WebUtils.exposeErrorRequestAttributes(request, ex, getServletName());
        return exMv;
    }

    throw ex;
}
```

**4、没人处理的异常，会交由BasicErrorController处理 /error请求**

- 遍历所有的**ErrorViewResolver**，解析错误视图。（只有一个就是：**DefaultErrorViewResolver**）
- 默认的**DefaultErrorViewResolver**，**作用是把响应状态码作为错误页的地址拼接成：error/xxx.html，最终响应这个页面，如果找不到这个页面，则响应默认白页**

#### 13.3 定制错误处理

**定制错误处理逻辑：**

- 1、自定义错误页，**在static或template下的error目录下**，放置自定义的错误处理页面（由**DefaultErrorViewResolver**完成）：

  - 404.html   5xx.html；有精确的错误状态码页面就匹配精确的，没有就找 4xx.html，还没有就是默认白页

- 2、==@ControllerAdvice== + ==@ExceptionHandler==处理全局异常（底层是**ExceptionHandlerExceptionResolver**支持的）

  ```java
  /**
   * 处理整个web controller的异常
   */
  @ControllerAdvice
  public class GlobalExceptionHandler {
  
      /**
       * 处理所有controller的数学运算异常和空指针异常
       */
      @ExceptionHandler(value = {ArithmeticException.class, NullPointerException.class})
      public String handleException(){
          return "err1";
      }
  }
  ```

- 3、==@ResponesStatus== + 自定义异常类（底层是**ResponseStatusExceptionResolver**支持的），把注解的信息封装成mav返回；底层直接调用 response.sendError(statusCode, resolvedReason)，发送/error请求。

  ```java
  @ResponseStatus(value = HttpStatus.FORBIDDEN, reason = "用户数量太多")
  public class UserTooManyException extends RuntimeException{
      public UserTooManyException(){}
      public UserTooManyException(String message){super(message);}
  }
  ```

- 4、自定义实现 HandlerExceptionResolver 处理异常

  ```java
  @Order(Ordered.HIGHEST_PRECEDENCE) //最高优先级
  @Component
  public class CustomerHandlerExceptionResolver implements HandlerExceptionResolver {
      @Override
      public ModelAndView resolveException(HttpServletRequest request,
                                           HttpServletResponse response,
                                           Object handler, Exception ex) {
          try {
              response.sendError(511,"我喜欢的错误");
          } catch (IOException e) {
              e.printStackTrace();
          }
          return new ModelAndView();
      }
  }
  ```

### 十四、Web原生组件注入

> Web原生三大组件：Servlet、Filter、Listener

#### 14.1 使用Servlet API

- 在主程序上加注解==@ServletComponentScan(basePackages = "com.feng")==：**指定原生Web组件都放在哪（相当于注入到容器）**
- 回顾Web原生组件注解：
  - ==@WebServlet(urlPatterns = "/my")==
  - ==@WebFilter(urlPatterns = {"/CSS/ *","/images/ *"})==
  - ==@WebListener==
- 原生组件具体编写省略，可查看JavaWeb笔记或本笔记对应代码

#### 14.2 使用RegistrationBean

**通过编写配置类的方式，注入组件，可通过注入XxxRegistrationBean完成：**

```java
/**
 * 用来注入Web原生组件
 */
@Configuration
public class MyRegisterConfig {

    @Bean
    public ServletRegistrationBean myServlet(){
        return new ServletRegistrationBean(new MyServlet(),"/my","myServlet");
    }

    @Bean
    public FilterRegistrationBean myFilter(){
        FilterRegistrationBean<MyFilter> registrationBean = new FilterRegistrationBean<>(new MyFilter());
        registrationBean.setUrlPatterns(Arrays.asList("/CSS/*","/images/*"));
        return registrationBean;
    }

    @Bean
    public ServletListenerRegistrationBean myListener(){
        return new ServletListenerRegistrationBean(new MyListener());
    }
}
```

#### 14.3 注入原理

> 注入的原生Servlet组件，不被SpringMVC拦截器所拦截，为什么？原理如下：

- 1、SpringMVC通过DispatcherServlet处理请求

- 2、DispatcherServlet是如何注册到Spring容器中的：

  - 容器中自动配置了 DispatcherServlet 组件，并且配置绑定到 WebMvcProperties；对应的配置项为：spring.mvc

    ```yaml
    spring:
      mvc:
        servlet:
          path: /mvc/ #修改DispatcherServlet处理请求以 "/mvc/"开始的 (默认是"/"，不要乱改)
    ```

  - 通过 **ServletRegistrationBean** 把DispatcherServlet注册进来

  - **而DispatcherServlet 默认处理的路径是 "/" 也就是默认所有路径** 

- 3、一旦注入了原生Servlet，那么该Servlet的路径为一个**精确路径**，例如"/my"

  - **Tomcat-Servlet**默认规则：
    - 如果多个Servlet都能处理到同一层路径
    - 以**精确优先原则**进行调用

- 4、**因此请求会根据精确优先原则，直接访问注入的原生Servlet("/my")，不访问DispatcherServlet，那么DispatcherServlet中的拦截器流程就不会经过**

### 十五、嵌入式Servlet容器

#### 15.1 切换嵌入式Servlet容器

- 默认支持的webServer

  - Tomcat、Jetty、Undertow
  - ServletWebServerApplicationContext 容器启动寻找 ServletWebServerFactory 并引导创建服务器

- 切换服务器

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
      <exclusions>
          <exclusion>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-tomcat</artifactId>
          </exclusion>
      </exclusions>
  </dependency>
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-undertow</artifactId>
  </dependency>
  ```

#### 15.2 嵌入式Servlet容器原理

- SpringBoot应用启动发现当前是Web应用。（web场景包-导入tomcat）

- web应用会创建一个web版的ioc容器 **ServletWebServerApplicationContext**

- **ServletWebServerApplicationContext** 启动的时候，寻找 **ServletWebServerFactory**（Servlet的web服务器工厂，也就是Servlet的web服务器），来创建并启动web服务器（WebServer）

- SpringBoot底层默认有三个**WebServer**工厂（支持三种web服务器）：

  TomcatServletWebServerFactory、JettyServletWebServerFactory、UndertowServletWebServerFactory

- SpringBoot底层直接会有一个自动配置类。**ServletWebServerFactoryAutoConfiguration**

- **ServletWebServerFactoryAutoConfiguration**导入了配置类：**ServletWebServerFactoryConfiguration**

- **ServletWebServerFactoryConfiguration**配置类，动态判断系统中导入了哪个web服务器的包，就在容器中注册哪个web服务器工厂。例如默认导入tomcat的包，容器中就有 TomcatServletWebServerFactory

- 容器中的TomcatServletWebServerFactory，创建Tomcat服务器（new Tomcat()）并启动；**TomcatWebServer**的构造器拥有初始化方法 initialize()：this.tomcat.start()；

- **内嵌服务器，就是底层自动调用了服务器启动的代码**

### 十六、定制化原理

#### 16.1 原理套路

根据之前的**自动配置原理**分析，总结出套路：

1、application.yaml配置文件生效原因：

- **场景starter  ---> xxxxAutoConfiguration  ---> 导入xxx组件 ---> xxx组件和类xxxProperties配置绑定 ---> 绑定到配置文件**

2、组件添加或替换生效原因：

- 当SpringMVC处理和响应请求需要调用容器中组件功能时，**往往会遍历所有的相关组件**；当我们**编写配置类**，为容器中注册了这些相关组件后，就会**被遍历到**，根据所写的自定义逻辑进行采用和处理。

#### 16.2 定制化常用方式

- 1、修改配置文件（底层组件常与xxxProperties配置绑定）
- 2、编写自定义配置类 ==@Configuration== ：==@Bean== 替换、增加容器中默认组件。例如：视图解析器
- 3、**web应用，可以编写配置类，直接实现 WebMvcConfigurer 接口 ，定制化web功能；**还可以 加 ==@Bean== 再扩展一些组件
- 4、实现WebMvcConfigurer 接口，再加上 ==@EnableWebMvc== 注解，全面接管SpringMVC，自己重写所有组件（夸张，一般不用）
- ... ...

### 十七、数据访问SQL

#### 17.1 数据源的自动配置

**1、导入JDBC场景**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

导入spring-boot-starter-jdbc，就默认导入了：数据源、jdbc、事务。官方不知道连接何种数据库，因此数据库驱动需要手动导入：

**2、导入数据库驱动**

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
<!--    <version>5.1.49</version> -->
</dependency>
```

不用默认仲裁的版本，使用自己的版本（注意驱动版本和本机mysql版本对应）：

两种方法：

- 1、直接依赖引入具体版本（maven就近依赖原则）

- 2、重新声明版本：

  ```xml
  <properties>
      <java.version>1.8</java.version>
      <mysql.version>5.1.49</mysql.version>
  </properties>
  ```

**2、分析自动配置**

- DataSourceAutoConfiguration：数据源的自动配置类
  - 修改数据源相关配置：spring.datasource
  - 数据库连接池的配置，默认没有自定义配置时，自动配置容器中默认的连接池（ @CoditionalOnMissingBean）
  - 底层默认配置好的连接池是：HikariDataSource

**3、修改配置项**

```yaml
spring:
  # 数据源
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: Socialbigfeng321
    url: jdbc:mysql://localhost:3306/movie_station
```

#### 17.2 使用Druid数据源

**1、druid官方GitHub地址**

https://github.com/alibaba/druid

**2、自定义整合方式**

**引入依赖：**

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>${druid-version}</version>
</dependency>
```

**自定义配置类注册Druid数据源：**

```java
@Configuration
public class DataSourceConfig {

    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource dataSource(){

        DruidDataSource druidDataSource = new DruidDataSource();
        //所有的set()都和配置文件中的 spring.datasource 绑定
        //druidDataSource.setUsername();
        //druidDataSource.setPassword();
        //druidDataSource.setUrl();
        return druidDataSource;
    }
}
```

**配置文件中关于Druid的配置：**

- **打开统计监控信息和防火墙预防sql注入**
- **连接池相关配置**

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: Socialbigfeng321
    url: jdbc:mysql://localhost:3306/springboot_learning
    type: com.alibaba.druid.pool.DruidDataSource    
    #Druid专有配置：
    #配置监控统计拦截的filters，stat:监控统计、wall：防御sql注入
    filters: stat,wall
    #连接池相关配置：
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    poolPreparedStatements: true
```

**使用Druid的内置监控页面以及Web监控功能：**

- 自定义配置类注册Servlet和Filter

```java
@Configuration
public class DataSourceConfig {

    /**
     * 配置druid数据源
     */
    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource dataSource(){return new DruidDataSource();}

    /**
     * 配置使用Druid的内置监控页面
     * (Druid要求配置一个原生Servlet，因为没有web.xml，采用嵌入注册方式)
     */
    @Bean
    public ServletRegistrationBean druidStatView(){

        //Druid监控页面功能要求注入的Servlet类型：
        StatViewServlet statViewServlet = new StatViewServlet();

        //注册到SpringBoot容器中
        ServletRegistrationBean registrationBean = new ServletRegistrationBean(statViewServlet, "/druid");

        //通过配置原生Servlet的初始化参数，为Druid监控配置访问权限(配置访问监控信息的用户与密码)
        HashMap<String, String> inits = new HashMap<>();
        ////Druid后台管理界面的登录账号、密码
        inits.put("loginUsername","chen");
        inits.put("loginPassword","123");
        //后台允许谁可以访问
        //initParams.put("allow", "localhost")：表示只有本机可以访问
        //initParams.put("allow", "")：为空或者为null时，表示允许所有访问
        inits.put("allow", "");
        //deny：Druid 后台拒绝谁访问
        //initParams.put("ling", "192.168.1.20");表示禁止此ip访问

        //配置到该原生Servlet中
        registrationBean.setInitParameters(inits);

        return registrationBean;
    }

    /**
     * 打开Web和Druid数据源之间的关联监控统计功能
     * （Druid要求配置一个原生Filter，因为没有web.xml，采用嵌入注册方式）
     */
    @Bean
    public FilterRegistrationBean webStatFilter(){

        //Druid数据源与web关联监控统计要求注入的Filter类型：
        WebStatFilter webStatFilter = new WebStatFilter();

        //注册到SpringBoot容器中
        FilterRegistrationBean filterBean = new FilterRegistrationBean(webStatFilter);

        //配置该原生Filter的过滤路径为全部
        filterBean.setUrlPatterns(Arrays.asList("/*"));

        //通过配置该原生Filter的初始化参数，设置统计功能排除哪些请求，不参与统计
        //exclusions：设置哪些请求进行过滤排除掉，从而不进行统计
        Map<String, String> initParams = new HashMap<>();
        initParams.put("exclusions", "*.js,*.css,/druid/*,/jdbc/*");
        filterBean.setInitParameters(initParams);

        return filterBean;
    }
}
```

**3、starter整合方式**

**引入starter：**

```xml
<!--Druid数据源starter-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.12</version>
</dependency>
```

**配置相关设置 spring.datasource.druid：（代替了自定义配置类来开启功能的操作）**

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: Socialbigfeng321
    url: jdbc:mysql://localhost:3306/springboot_learning
    type: com.alibaba.druid.pool.DruidDataSource    
  	
  	#使用starter整合Druid数据源的简洁方式：
    druid:
      aop-patterns: com.feng #Spring监控AOP切入点
      filters: stat,wall #监控统计、防火墙（预防sql注入）
      #监控页面开启
      stat-view-servlet:
        enabled: true
        login-username: dlnu
        login-password: 2019
      #web-druid关联统计功能：
      web-stat-filter:
        enabled: true
        exclusions: '*.js,*.css,/druid/*,/jdbc/*'
      #连接池相关配置：
      initial-size: 5
      min-idle: 5
      max-active: 20
      max-wait: 60000
      pool-prepared-statements: true
```

### 十八、整合MyBatis

> 相关网站：https://github.com/mybatis

#### 18.1 整合步骤

> 引入starter --> 在yaml配置相关信息 --> 编写mapper接口 --> 编写sql映射文件

引入依赖：

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.2.2</version>
</dependency>
```

yaml相关常用配置（更多配置可查 **配置绑定类**：MybatisProperties）：

```yaml
# mybatis设置
mybatis:
  type-aliases-package: com.feng.pojo #定义别名路径（sql映射文件的resultType属性就不用再写全类名）
  mapper-locations: classpath:mybatis/mapper/*.xml #定义mapper的sql映射文件路径
  configuration: #MyBatis全局配置
    map-underscore-to-camel-case: true #开启驼峰命名自动映射，自动将数据库列名和pojo属性名映射（不用再写resultMap）
```

#### 18.2 MyBatis配置原理解析

> SpirngBoot自动原理知道，会加载所有的xxxAutoConfiguration类。导入mybatis的starter后，springboot会加载MybatisAutoConfiguration，在这里面进行了一系列自动配置，并且设置了配置绑定，代替了以前整合时要创建 spring-mybatis.xml 进行配置的方法，只需要在yaml中统一配置即可
>
> 在MybatisAutoConfiguration中自动配置了：
>
> - @EnableConfigurationProperties(MybatisProperties.class)：绑定了自动配置类MybatisProperties，与yaml中的配置绑定
>
> - SqlSessionFactory已经自动注入到容器中（无需像以前一样在xml进行配置）
> - SqlSession：自动配置了 SqlSessionTemplate ，它组合了SqlSession
> - @Import(AutoConfiguredMapperScannerRegistrar.class)：定义了扫描加上了@Mapper注解的接口

```java
@org.springframework.context.annotation.Configuration
@ConditionalOnClass({ SqlSessionFactory.class, SqlSessionFactoryBean.class })
@ConditionalOnSingleCandidate(DataSource.class)
@EnableConfigurationProperties(MybatisProperties.class)
@AutoConfigureAfter({ DataSourceAutoConfiguration.class, MybatisLanguageDriverAutoConfiguration.class })
public class MybatisAutoConfiguration implements InitializingBean {

    ... ...

    //配置类只有一个有参时，所有参数从容器中确定（也就是自动注入到容器中的MyBatis组件）
    public MybatisAutoConfiguration(... ...) {
        this.properties = properties;
        this.interceptors = interceptorsProvider.getIfAvailable();
        this.typeHandlers = typeHandlersProvider.getIfAvailable();
        this.languageDrivers = languageDriversProvider.getIfAvailable();
        this.resourceLoader = resourceLoader;
        this.databaseIdProvider = databaseIdProvider.getIfAvailable();
        this.configurationCustomizers = configurationCustomizersProvider.getIfAvailable();
        this.sqlSessionFactoryBeanCustomizers = sqlSessionFactoryBeanCustomizers.getIfAvailable();
    }
    ... ...
    
    @Bean
    @ConditionalOnMissingBean
    public SqlSessionFactory sqlSessionFactory(DataSource dataSource) throws Exception {...}
    
    ... ...
    
    @Bean
    @ConditionalOnMissingBean
    public SqlSessionTemplate sqlSessionTemplate(SqlSessionFactory sqlSessionFactory) {...}
    
    ... ...
    
    public static class AutoConfiguredMapperScannerRegistrar
        implements BeanFactoryAware, EnvironmentAware, ImportBeanDefinitionRegistrar {...}
    
    ... ...
    
    @org.springframework.context.annotation.Configuration
  	@Import(AutoConfiguredMapperScannerRegistrar.class)
  	@ConditionalOnMissingBean({ MapperFactoryBean.class, MapperScannerConfigurer.class })
    public static class MapperScannerRegistrarNotFoundConfiguration implements InitializingBean {...}

    ... ...
  }
}
```

#### 18.3 整合MyBatisPlus并使用

> MyBatisPlus是MyBatis的增强，使用和配置方法和以前一样，新增了自动编写mapper基本crud的sql语句的功能。

**引入依赖（引入后默认引入了jdbc依赖，可以不再引入jdbc依赖）：**

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.0</version>
</dependency>
```

**自动编写基础sql功能的使用：**

**编写Mapper接口时，继承接口： BaseMapper<POJO类型>即可**，若需要编写复杂sql，按之前的编写sql映射文件的方法就行。

### 十九、数据访问NoSQL

==学完redis再来补充这部分内容==

### 二十、单元测试

#### 20.1 Junit5简介

**SpringBoot 2.2.0 版本开始引入 JUnit5 作为单元测试默认库**

作为最新版本的 JUnit框架，JUnit5与之前版本的Junit框架有很大的不同。由三个不同的子项目的几个不同模块组成。

JUnit5 = JUnit Platform + JUnit Jupiter + JUnit Vintage

 JUnit Platform：是在JVM上启动测试框架的基础，不仅支持Junit自制的测试引擎，其他测试引擎也都可以接入

JUnit Jupiter：提供了JUnit5新的的编程模型，是JUnit5的新特性的核心。内部包含了一个测试引擎，用于在JUnit Platform上运行

JUnit Vintage：由于JUnit已经发展多年，为了照顾老项目（Junit3、Junit4），JUnit Vintage提供了兼容3、4版本的测试引擎

**注意：**

**SpringBoot 2.4 以上版本移除了默认对 JUnit Vintage的依赖，如果要兼容Junit4，需要自行引入该引擎依赖。**

#### 20.2 JUnit5使用

**1、引入依赖starter：**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

**2、简单使用：**

- 编写测试方法：标注==@Test==注解（注意使用Junit5版本的，import org.junit.jupiter.api.Test;）
- 在测试类上标注==@SpringBootTest== ，Junit类具有Spring的功能，例如：==@Autowried==

**3、常用注解：**

JUnit5的注解与JUnit4的注解有所变化

- @Test ：表示方法是测试方法。但是与Unit4的@Test不同，他的职责非常单一不能声明任何属性，拓展的测试将会由Jupiter提供额外测试
- @Transactional：表示方法测试完成后自动回滚
- @ParameterizedTest ：表示方法是参数化测试
- @RepeatedTest ：表示方法可重复执行
- @DisplayName ：为测试类或者测试方法设置展示名称
- @BeforeEach ：表示在每个单元测试之前执行
- @AfterEach ：表示在每个单元测试之后执行
- @BeforeAll ：表示在所有单元测试之前执行
- @AfterAll ：表示在所有单元测试之后执行
- @Tag ：表示单元测试类别，类似于JUnit4中的@Categories
- @Disabled ：表示测试类或测试方法不执行，类似于JUnit4中的@lgnore
- @Timeout ：表示测试方法运行如果超过了指定时间将会返回错误
- @SpringBootTest：表示该测试类整合了SpringBoot的功能
- @ExtendWith ：为测试类或测试方法提供扩展类引用，类似于以前的@RunWith

#### 20.3 断言机制

> 断言（assertions）是测试方法中的**核心**部分，用来对测试需要满足的条件进行验证。这些断言方法都是**org.junit.jupiter.api.Assertions 的静态方法**。
>
> 断言机制可以**检查业务逻辑返回的数据是否合理**，**所有的测试运行结束之后会有一个详细的测试报告**
>
> JUnit5 内置的断言可以分成如下几个类别：

**1、简单断言**

**前面的断言失败，后面的断言就不会执行（类似try-catch）**

用来**对单个值**进行简单的验证，如：

Assertions 的静态方法（方法执行后输出message，可以不写message这个参数）：

| 方法                                     | 说明                                 |
| ---------------------------------------- | ------------------------------------ |
| assertEquals(期望值，测试值，message)    | 判断两个对象或两个原始类型是否相等   |
| assertNotEquals(期望值，测试值，message) | 判断两个对象或两个原始类型是否不相等 |
| assertSame(期望值，测试值，message)      | 判断两个对象引用是否指向同一个对象   |
| assertNotSame(期望值，测试值，message)   | 判断两个对象引用是否指向不同对象     |
| assertTrue(测试值，message)              | 判断给定的布尔值是否为true           |
| assertFalse(测试值，message)             | 判断给定的布尔值是否为false          |
| assertNull(测试值，message)              | 判断给定的对象引用是否为null         |
| assertNotNull(测试值，message)           | 判断给定的对象引用是否不为null       |

**2、数组断言**

通过 **assertArrayEquals(期望值、测试值)** 方法判断两个对象或原始类型的数组是否相等

**3、组合断言**

assertAll() 方法接受多个 org.junit.jupiter.api.Executable 函数式接口的实例作为要验证的断言，所有断言通过才算测试通过，可以通过 lambda表达式来提供这些断言

```java
@Test
@DisplayName("组合断言")
void all(){
    assertAll("test",
             ()-> asserTrue(true && true),
             ()-> assertEquals(1,2));
}
```

**4、异常断言**

在JUnit4时期，想要测试方法的异常情况时，需要使用@Rule注解的ExpectedException变量还是比较麻烦得。而JUnit5提供了一种新的断言方式**Assertions.assertThrows()**，配合函数式编程就可以进行使用。

只有业务逻辑抛出期望的异常才测试成功

```java
@Test
@DisplayName("异常测试")
public void exceptionTest(){
    ArithmeticException exception = Assertions.assertThrows(
        //断言业务逻辑会抛出这个异常 
        ArithmeticException.class, () -> {int i = 1 % 0)}, "业务逻辑居然正常运行？");
}
```

**5、超时断言**

JUnit5提供了 **Assertions.assertTimeout()** 测试方法的运行时间是否超时：

```java
@Test
@DisplayName("超时测试")
public void timeoutTest(){
    //测试方法时间超过1s将会异常
    Assertions.assertTimeout(Duration.ofMillis(1000),() -> Thread.sleep(500));
}
```

**6、快速失败**

通过 fail() 方法直接使得测试失败

```java
@Test
@DisplayName("快速失败")
public void shouldFail(){
    if(XXXX){
        fail("This should fail");
    }
}
```

#### 20.4 前置条件

> JUnit5 中的前置条件（assumptions【假设】）类似于断言，不同之处在于**不满足断言会使得测试方法失败，**而**不满足的前置条件只会使得测试方法的执行终止**。前置条件可以看成是测试方法执行的前提，当该前提不满足时，就没有执行的必要。

使用 Assumptions类方法：

- Assumptions.assumeTrue(条件，message)：条件为true时执行后面的测试
- Assumptions.assumeFalse(条件，message)：条件为false时执行后面的测试

#### 20.5 嵌套测试

> JUnit5 可以通过 java 中的内部类和 @Nested 注解实现嵌套测试，从而可以更好地把相关的测试方法组织在一起。在内部类中可以使用@BeforeEach 和 @AfterEach 注解，而且嵌套的层次没有限制。内层的测试可以驱动外层的测试，外层的驱动不了内层的。

定义内层测试：

定义java内部类，在内部类上标注 @Nested 注解。

#### 20.6 参数化测试

> 参数化测试是JUnit5很重要的一个新特性，它使得用不同的参数多次运行测试成为了可能，也为我们的单元测试带来许多便利。

利用==@ValueSource==等注解，指定入参，将可以使用不同的参数进行多次单元测试，而不需要每新增一个参数就新增一个单元测试，省去了很多冗余代码。

==@ValueSource==：为参数化测试指定入参来源，支持八大基础类型以及Sring类型，Class类型

==@NullSource==：表示为参数化测试提供一个null的入参

==@EnumSource==：表示为参数化测试提供一个枚举入参

==@CsvFileSource==：表示读取指定CSV文件内容作为参数化测试入参

==@MethodSource==：表示读取指定方法返回值作为参数化测试入参（方法返回需要是一个流）

真正让人感到惊艳的是，JUnit5 的参数化测试可以支持外部各类入参，如：CSV,YML,JSON 文件甚至方法的返回值，只需要去实现ArgumentsProvider接口，任何外部文件都可以作为它的入参。

### 二十一、指标监控

> 未来每一个微服务在云上部署以后，我们都需要对其进行监控、追踪、审计、控制等。SpringBoot就抽取了Actuator场景，使得我们每个微服务快速引用即可获得生产级别的应用监控、审计等功能。

==学到微服务再来补充这部分内容==

### 二十二、高级特性

#### 22.1 Profile功能

#### 22.2 外部化配置













































































































































