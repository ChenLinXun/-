# JDBC学习笔记

### 一、JDBC介绍

#### 1.  JDBC概念

1. **JDBC** 就是使用**Java语言**操作**关系型数据库**的一套**API**
2. 全称：(Java DataBase Connectivity)  **Java数据库连接**

#### 2. JDBC本质

1. 官方（**sun公司**）定义的一套操作所有**关系型数据库**的**规则**，即**接口**

2. 各个数据库厂商去实现这套接口，提供**数据库驱动jar包**

3. 我们可以使用这套接口（JDBC）编程，**真正执行的代码是驱动jar包中的实现类**

4. 相关类和接口在 **java.sql包** 与 **javax.sql包**中

   ![image-20220403171552851](..\image\image-20220403171552851.png)

#### 3. JDBC好处

1. 各数据库厂商**使用相同的接口**，Java代码不需要针对不同数据库分别开发
2. 可随时替换底层数据库，**访问数据库的Java代码基本不变**

### 二、JDBC API详解

#### 1. 数据库连接

1. ==注册驱动（获取Driver对象）：==

   1. 手动获取Driver对象（**不建议使用**）：
      1. 方法一：静态加载：
         Driver driver = new com.mysql.jdbc.Driver(); 
         （灵活性差，依赖强，不使用）
      2. **方法二：动态加载：** 
         Class clazz = Class.forName("com.mysql.jdbc.Driver");
         Driver driver = clazz.newInstance();  
         （利用反射，动态加载，灵活性强，依赖弱，使用）
   2. **使用DriverManager注册**，不需要手动创建Driver对象（**现在都使用的方法！**）：
      1. 使用步骤：
         1. 加载Driver类：
            Class clazz = Class.forName("com.mysql.jdbc.Driver");
            Driver driver = clazz.newInstance(); 
         2. 使用DriverManager注册：
            DriverManager.registerDriver(driver);
      2. **简化步骤（建议使用，无需手动创建driver对象）：**
         1. **自动**完成注册：
            **Class.forName("com.mysql.jdbc.Driver");**
         2. 不需要创建driver对象的原因：
            在com.mysql.jdbc.Driver源码中：
            **当Driver类被加载时，在静态代码块中会自动创建driver对象并完成注册：**
            ![image-20220403180036773](..\image\image-20220403180036773.png)
         3. 提示：
            mysql驱动包5.1.6及以上版本不需要注册步骤：Class.forName("com.mysql.jdbc.Driver");
            自动加载 jar包 中META-INF/services/java.sql.Driver文件中的驱动类
            不过还是建议不省略，这样代码比较明确

2. ==获取连接：==

   1. 如果不使用DriverManager注册驱动，则使用driver对象获取连接：

      Connection conn = driver.connect(url, username, password);

      **url：**数据库路径，jdbc:mysql://ip地址:端口号/数据库名称?参数键值对1&参数键值对2...
      例如：jdbc:mysql://localhost:3306/exp_device

      **username：**数据库用户名(root)

      **password：**数据库密码

   2. **建议使用**DriverManager注册驱动，**通过DriverManager获取连接**：

      **Connection conn = DriverManager.getConnection(url, uername, password);**

   3. 连接参数的获取方法：

      一定使用**properties配置文件**获取：（不能写死在代码中！）

      ```java
      //加载配置文件
      Properties properties = new Properties();
      properties.load(new FileReader("配置文件路径"));
      
      //获取连接
      Connection conn = DriverManager.getConnection(properties.getProperty("dbUrl"),
      properties.getProperty("dbUserName"),properties.getProperty("dbPassword"));
      ```

3. ==总结常用获取连接方式（模板）：==

   ```java
   //自动完成注册
   Class.forName("com.mysql.jdbc.Driver");
   
   //加载配置文件
   Properties properties = new Properties();
   properties.load(new FileReader("配置文件路径"));
   
   //获取连接
   Connection con = DriverManager.getConnection(properties.getProperty("dbUrl"),
   properties.getProperty("dbUserName"),properties.getProperty("dbPassword"));
   ```

#### 2. 执行SQL的对象

1. **Statement** ==普通执行SQL对象==

   1. 介绍：Statement对象，用于执行静态SQL语句并返回其生成的结果

   2. 使用方法：

      1. 获取Statement对象：**conn.createStatement();**
      2. Statement对象方法：
         1. **int executeUpdate(sql)**：执行DML、DDL语句
            返回值：(1)DML语句影响的行数 (2)DDL语句执行成功也可能返回0，不报异常一般就成功了
         2. **ResultSet executeQuery(sql)**：执行DQL语句
            返回值：ResultSet 结果集对象

   3. 存在问题：

      1. Statement对象执行SQL语句，存在SQL注入风险，**因此实际开发中不使用Statement对象**！

      2. **SQL注入是利用某些系统没有对用户输入的数据进行充分的检查**，而在用户输入数据中**注入非法**的**SQL语句段或命令**，恶意攻击数据库。例子：
         假如需要执行的sql语句是：
         select name,pwd from admin where name = '?' and pwd = '?'
         如果输入的是用户名是：1' or
         输入的密码是：or '1'= '1
         那么这条语句就变成了：
         select name,pwd from admin where name = '1' or' and pwd = 'or '1'= '1'
         切分出了三个条件，而条件'1'= '1'是永远成立的，那么输入的内容就成了万能密码
         相关代码：

         ```java
         //假如已经获取了连接和statement对象
         //组织sql
         String sql = "select name,pwd form admin where name ='" + admin_name + 
                      "' and pwd = '" + admin_pwd + "'";
         //执行sql
         ResultSet resultSet = statement.executeQuery(sql);
         //处理结果
         ...
         ```

      3. **防范SQL注入**，只要**用PrepareStatement（从Statement扩展而来）替代使用**就可以了

2. **PrepareStatement** ==预编译SQL的执行SQL对象，防止SQL注入==

   1. **介绍：**
      PrepareStatement对SQL语句进行**预编译**，执行的**SQL语句中的参数**，**用问号来表示，如同占位符**，可以防止SQL注入

   2. **使用方法：**

      1. 组织SQL：String sql = "select * from user where username = **?** and password = **?**";

      2. 获取PrepareStatement对象：**conn.prepareStatement(sql);**

      3. 设置参数：

         **setXxx(参数1，参数2)；**

         Xxx：数据类型；如：setInt(参数1，参数2)、setString(参数1，参数2)

         参数1：第几个**?**占位符；  

         参数2：具体值；

      4. 执行SQL：
         不需要传递SQL语句：

         1. **executeUpdate()：**返回受影响行数
         2. **executeQuery()：**返回Result对象

   3. **PrepareStatement详解**

      1. 为什么Statement会导致SQL注入？
         因为编辑SQL语句时，都是采用字符串拼接的形式，把java变量拼接到语句中，从而导致注入

      2. PrepareStatement的优势：

         1. 预编译SQL，性能更高
         2. 防止SQL注入：**将敏感字符进行转义**

      3. **PrepareStatement原理：**

         1. 在获取PrepareStatement对象时，将sql语句发送给mysql服务器进行检查，编译（这些步骤很耗时）
         2. 执行时就不用再进行这些步骤了，速度更快（如果不是预编译，则还要检查和编译）
         3. 如果sql模板一样，则只需进行一次检查、编译
         4. 例子：
            select * from tb_user where username = ?
            setString(1,"zhangsan"); //检查编译了一次
            setString(2,"lisi"); //又检查编译了一次，效率低

      4. **PrepareStatement预编译功能的开启：**

         1. 在properties配置文件的url后加入参数：useServerPrepStmts=true
            jdbc:mysql://localhost:3306/exp_device?useServerPrepStmts=true

         2. 配置MySQL的my.ini文件，执行日志（重启mysql服务后生效）

            ```ini
            #启动PrepareStatement预编译功能
            log-output=FILE
            general-log=1
            general_log_file="D:\develop\FengMysql\mysql-5.7.19-winx64\log\mysql.log"
            slow-query-log=1
            slow_query_log_file="D:\develop\FengMysql\mysql-5.7.19-winx64\log\mysql_slow.log"
            long_query_time=2
            ```

3. **CallableStatement** ==执行存储过程的对象==

   用得不多，搁置。

#### 5. 结果集对象

1. 介绍：**结果集对象ResultSet，封装了DQL查询语句的结果**

2. 获取查询结果：

   1. **boolean next()**：(1)将光标从当前位置向前移动一行  (2)判断当前行是否为有效行
      返回值：
      true：有效行，当前行有数据
      false：无效行，当前行没数据

   2. **xxx getXxx(参数)**：获取数据

      xxx：数据类型；如：int getInt();   String getString();

      参数：

      1. int类型：列的编号，从1开始
      2. String类型：列的名称

#### 4. 事务处理

1. 基本介绍：

   1. JDBC程序中当一个Connection对象创建时，默认情况下是自动提交事务：每执行一条SQL，成功就会向数据库自动提交，而不能回滚
   2. JDBC程序为了让多条SQL作为整体执行，需要使用事务处理

2. 使用方式：

   **操控connection对象**：

   1. 调用connection对象的**setAutoCommit(false)**方法，取消自动提交事务
   2. 在事务中的所有SQL语句执行成功后，调用**commit()**方法，提交事务
   3. 在其中某个SQL执行失败或出现异常时，在catch块中，调用**rollback()**方法，回滚事务

#### 5. 批处理

1. 介绍：
   当需要**成批插入或更新记录**时，可以采用Java的**批量更新机制**，这一机制允许多条语句一次性提交给数据库进行**批量处理**。通常情况下比单独提交处理更有效率。

2. 方法：

   1. 在==数据库url配置==中添加参数：**rewriteBatchedStatements=true**

   2. **所需API：**
      操纵PreparedStatement对象：

      1. **addBatch()**：添加需要批量处理的SQL语句或参数到批处理包中
      2. **executeBatch()**：执行批量处理
      3. **clearBatch()**：清空批处理包中的语句

   3. 示例：

      ```java
      //假如已经获取了连接conn
      //组织sql
      String sql = "insert into admin values(null,?,?)"
          
      //获取PreparedStatement对象
      PreparedStatement ps = conn.prepareStatement(sql);
      
      //开始执行
      for(int i = 0; i < 5000; i++ ){
          PreparedStatement.setString(1,"jack" + i);
          PreparedStatement.setString(2,"666");
          //将sql语句加入到批处理包中
          PreparedStatement.addBatch();
          //当包中有1000条sql语句时，进行批量处理
          if((i + 1) % 1000 == 0){
              //执行
              PreparedStatement.executeBatch();
              //清空包中语句，装入下一批
              PreparedStatement.clearBath();
          }
      }
      ```

### 三、数据库连接池

#### 1. 连接池简介

1. **数据库连接池**是个==容器==，负责==分配、管理数据库连接==（Connection）
2. 它允许应用程序**重复使用**一个现有的数据库连接，而不是再重新创建一个（每个连接用完后，归还池中）
3. 当池中**连接分配完后**，新的连接请求会被加入到**等待队列**中
4. **释放**空闲时间超过最大空闲时间的**数据库连接**来**避免**因为没有释放数据库连接而引起的**数据库连接遗漏**
5. 好处：
   1. 资源重用
   2. 提升系统响应速度
   3. 避免数据库连接遗漏
6. 图解：
   ![image-20220404145556194](..\image\image-20220404145556194.png)

#### 2. 数据库连接池实现

1. **标准接口：DataSource**
   官方（SUN）提供的**数据库连接池标准接口**，由第三方组织实现此接口
   功能：**获取连接**（Connection getConnection()）
2. 常见的数据库连接池：
   1. DBCP
   2. C3P0
   3. **Druid**

#### 3. Druid数据库连接池

1. 介绍：

   1. Druid连接池是==阿里巴巴==开源的数据库连接池项目
   2. 功能强大，性能优秀，是Java语言**最好的数据库连接池之一**

2. **Druid使用步骤：**

   1. 导入jar包 druid-1.2.4.jar
      Java 项目添加 jar包 到 libs目录中；
      Maven 项目导入依赖坐标：

      ```xml
      <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.2.4</version>
      </dependency>
      ```

   2. 定义配置文件：properties（详细配置可查）

      ```properties
      driverClassName=com.mysql.jdbc.Driver
      url=
      username=root
      password=
      # 初始化连接数量
      initialSize=5
      # 最大连接数量
      maxActive=10
      # 最大等待时间(毫秒)
      maxWait=3000
      ```

   3. 加载配置文件:

      ```java
      Properties pp = new Properties();
      pp.load(new FileReader("src\\DruidConnect.properties"));
      ```

   4. 获取数据库连接池对象:

      ```java
      //获取连接池对象
      DataSource dataSource = DruidDataSourceFactory.createDataSource(pp);
      ```

   5. 获取连接:

      ```java
      //获取数据库连接
      Connection conn = dataSource.getConnection();
      ```

### 五、Apache-DBUtils使用

#### 1. DBUtils介绍

1. 简介：
   **org.apache.commons.dbutils** 是 Apache 组织提供的一个开源的 ==JDBC工具类库==，它是对JDBC的封装，使用**dbutils能极大地简化jdbc编码的工作量！**

2. **DBUtils常用的类和接口：**

   1. **QueryRunner 类**：该类封装了SQL的执行，==增删改查、批处理==；而且是==线程安全==的

   2. **ResultSetHandler 接口**：该接口用于处理java.sql.ResultSet，将查询的结果集数据转换为另一种形式，形式有：

      | ResultSetHandler实现类 | 实现类对结果集的封装                                         |
      | ---------------------- | ------------------------------------------------------------ |
      | ArrayHandler           | 把结果集中的第一行数据转成对象数组                           |
      | ArrayListHandler       | 把结果集中的每一行数据都转成一个数组，再存到List中           |
      | BeanHandler            | 将结果集中的第一行数据封装到一个对应的JavaBean实例中         |
      | ==BeanListHandler==    | ==将结果集中的每一行数据封装到一个对应的JavaBean实例中，再存到List中（很常用）== |
      | ColumnListHandler      | 将结果集中某一列的数据存放到List中                           |
      | KeyedHandler(name)     | 将结果集中地每行数据都封装到map里，再把这些map存到一个Map里，其key为指定的key |
      | MapHandler             | 将结果集中的每一行数据封装到一个Map里，key是列名，value就是对应的值 |
      | MapListHandler         | 将结果集中的每一行数据都封装到一个Map里，然后把这些Map存放到List中 |

3. **QueryRunner 和 ResultSetHandler 的关系：**

   1. 在QueryRunner类的==查询操作==的方法 ==query()== 中，ResultSetHandler接口是其方法的一个参数，也就是说，调用query()方法，**需要传递一个ResultSetHandler接口的实现类对象**（如上表）。
   2. 因此，**query()方法返回的类型**，**与**传递的**ResultSetHandler**接口的**实现类对象有关**

#### 2. DBUtils使用与源码分析

1. **DBUtils的使用：**

   1. 创建QueryRunner对象：

      ```java
      QueryRunner qr = new QueryRunner();
      ```

   2. 调用方法：

      1. **增删改操作**，==update()==方法：
         有许多重载，最常用的一个是：
         **public int update(Connection conn, String sql, Object... params)**
         **传入参数：**连接、sql语句、**任意数量和类型的sql参数**

         ```java
         //假如获取了连接，组织了sql
         qr.update(con,sql,parameters); //parameters可以是任意数量和类型的sql参数
         ```

      2. **查询操作**，==query()==方法：
         有许多重载，最常用的一个是：
         **public <T> T query(Connection conn, String sql, ResultSetHandler<T> rsh, Object... params)**
         **传入参数：**连接、sql语句、**ResultSetHandler接口的实现类对象**、**任意数量和类型的sql参数**

         ```java
         //假如获取了连接，组织了sql
         //传入的ResultSetHandler接口的实现类对象是BeanListHandler类型的
         //而BeanListHandler类将结果集数据封装到javaBean实例中，再存入List集合
         //这里传递的javaBean是Brand类的Class对象，因此会将数据封装到Brand实例中
         //最终返回的是存放这些Brand实例的List集合
         query(con, sql, new BeanListHandler<T>(Brand.class), parameters);
         ```

2. **DBUtils源码分析：**

   1. QueryRunner类的update()方法，**与ResultSetHandler接口无关**，不再具体分析。其方法源码实际上是==对PreparedStatement操作的封装==，返回的是受影响的行数

   2. **QueryRunner类的query()方法的源码分析：**

      1. query()方法源码：

         ```java
         //QueryRunner类对象所调用的query()方法：
         public <T> T query(Connection conn, String sql, ResultSetHandler<T> rsh, Object... params) 
             throws SQLException {
             	//这里调用真正处理sql的query()方法，看下一小节
                 return this.query(conn, false, sql, rsh, params);
             }
         ```

      2. 真正处理sql的地方：
         ==result = rsh.handle(rs); 这一句是关键==

         ```java
         //真正处理sql的query()方法，最核心的一段：
         PreparedStatement stmt = null;
         ResultSet rs = null;
         
         //result查询返回的内容是Object，说明内容的类型是和ResultSetHandler接口的实现类有关的
         Object result = null; 
         
         try {
             //操纵PreparedStatement对象，处理sql的操作：
             stmt = this.prepareStatement(conn, sql);
             this.fillStatement(stmt, params);
             rs = this.wrap(stmt.executeQuery());//wrap()返回的是ResultSet对象
             
             //这里调用了传入的ResultSetHandler实现类对象的handle(ResultSet rs)方法，
         	//把结果集ResultSet对象做了处理，并返回
             //因此query()方法最终返回的内容：和ResultSetHandler实现类有关
             result = rsh.handle(rs);
             
         } catch (SQLException var33) {
             ...
         } finally {
             //关闭资源...
         }
         ```

   3. **ResultSetHandler实现类的源码分析：**

      ==以BeanListHandler<T>类为例：==

      1. BeanListHandler<T>的源码：

         ```java
         public class BeanListHandler<T> implements ResultSetHandler<List<T>> {
            
             //type是构造BeanListHandler对象需要传递的javaBean的Class对象
             //目的是为了将结果集的每一行数据都封装成所传的这个javaBean实例
             private final Class<? extends T> type;
             
             //这是用于实现上述目的（封装javaBean）的类的对象
             private final RowProcessor convert;
         
             //构造器，需要传入一个javaBean的Class对象
             public BeanListHandler(Class<? extends T> type) {
                 this(type, ArrayHandler.ROW_PROCESSOR);
             }
         
             //另一个构造器，能指定RowProcessor，这个先不管
             public BeanListHandler(Class<? extends T> type, RowProcessor convert) {
                 this.type = type;
                 this.convert = convert;
             }
         
             //重点！handle(ResultSet rs)方法，将结果集每行封装到javaBean实例中，
             //并且将所有javaBean实例装入List并返回的方法
             public List<T> handle(ResultSet rs) throws SQLException {
                 return this.convert.toBeanList(rs, this.type);
             }
         }
         ```

      2. 总结：

         1. 在QueryRunner类的query()方法底层，处理完sql后，==取得的结果集ResultSet对象==，==会交给==所传递==ResultSetHandler实现类对象 的 handle(ResultSet rs)方法==进行处理，最终拿到一个==程序员希望形式== 的 **查询结果**
         2. **ResultSetHandler实现类会各自实现handle(ResultSet rs)方法，以满足对查询结果封装的各种形式（具体形式看本章1.2.2的表格）**

### 六、JDBC开发中的BasicDao

#### 1. **BasicDao介绍**

1. 在实际的 JDBC开发中，对应每个表的业务操作应该各不相同，并且组织的sql语句一定不相同
2. 但是每个表的业务最终都要落实到==增删改查==上来，然而最基础的这些==增删改查==操作，每个表都是一样的
3. 因此，可以开发一个BasicDao类，仅仅实现最基础的==增删改查==
4. 每个表的业务操作都对应一个Dao类，这些Dao类都继承BasicDao类
5. 从而不需要在每个表的Dao类中重复地写基本的==增删改查==代码，只需要专注于业务逻辑

#### 2. **BasicDao开发模板**

​	**（可根据需要进行修改）**

```java
//这是一个通用的基本Dao类，完成基本的增删改查功能
//dao包下的其他Dao类可以基于这个基本类来进行设计
//没使用连接池技术，可以修改，使用连接池效率更高
public class DmlBasicDao<T> {

    private QueryRunner qr = new QueryRunner();

    //通用更新方法（插入，修改，删除）
    public int update(String sql,Object... parameters){

        Connection con = null;

        try {
            con = DbUtil.getCon();
            return qr.update(con,sql,parameters);

        } catch (Exception e) {
            throw new RuntimeException(e);//将编译异常转换成运行异常，抛出
        } finally {
            DbUtil.close(null,null,con);
        }

    }

    //通用查询(结果多行)方法
    public List<T> queryMulti(String sql, Class<T> clazz, Object... parameters){

        Connection con = null;

        try {
            con = DbUtil.getCon();
            return qr.query(con, sql, new BeanListHandler<T>(clazz), parameters);

        } catch (Exception e) {
            throw new RuntimeException(e);//将编译异常转换成运行异常，抛出
        } finally {
            DbUtil.close(null,null,con);
        }

    }

    //通用查询(结果单行)方法
    public T querySingle(String sql, Class<T> clazz, Object... parameters){

        Connection con = null;

        try {
            con = DbUtil.getCon();
            return qr.query(con, sql, new BeanHandler<T>(clazz), parameters);

        } catch (Exception e) {
            throw new RuntimeException(e);//将编译异常转换成运行异常，抛出
        } finally {
            DbUtil.close(null,null,con);
        }

    }

    //通用查询(结果单行单列)方法
    public Object queryScalar(String sql, Class<T> clazz, Object... parameters){

        Connection con = null;

        try {
            con = DbUtil.getCon();
            return qr.query(con,sql,new ScalarHandler<>(),parameters);

        } catch (Exception e) {
            throw new RuntimeException(e);//将编译异常转换成运行异常，抛出
        } finally {
            DbUtil.close(null,null,con);
        }

    }

}
```



