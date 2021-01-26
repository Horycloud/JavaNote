### SSH框架

与之前 JavaEE 三层架构相对应的

+ Web 层 —— Struts2 框架
+ service 层 —— Spring 框架
+ dao 层 —— Hibernate 框架



### Struts 2

Struts是什么？

+ Struts2 框架应用于JavaEE三层结构中 Web 层框架
+ Struts2 用于处理访问服务器的请求
+ 取代Servlet

关于Struts2

+ 是在 Struts 1 和 webwork 基础之上发展的`全新`的框架
+ 解决的问题
  + 如果功能很多，创建的Servlet很多。web阶段解决方案：创建 BaseServlet 解决，写到底层反射代码实现。
  + 有了 Struts 就可以解决这个问题。
  + Struts 2 里面封装了一个过滤器，原先针对不同的请求需求创建不同的Servlet，现在有了Struts 2 ，对于不同的操作，只需在一个类里面写不同的方法即可。
  + 这里的类就是 action 







### Hibernate



第一天

+ Web内容回顾
  + JavaEE三层架构
  + MVC模式
+ hibernate 概述
  + 什么是框架
  + 什么是hibernate
  + 什么是orm
+ hibernate 入门
  + 开发环境搭建
  + 添加功能实现
+ hibernate 配置文件详解
  + 映射配置文件
  + 核心配置文件
+ hibernate 核心 API
  + Configuration、SessionFactory、Session、Transaction



`关于Hibernate框架`

+ 其底层就是 JDBC ，使用 Hibernate 最大的好处就是，不需要写 JDBC 的复杂代码，以及 SQL 语句实现。
+ Hibernnate 开源的轻量级框架（不需要依赖于其他的东西，jar包比较小）
+ Hibernate 版本，Hibernate3.x、Hibernate4.x（过渡）、Hibernate5.x （学习）

`关于如何实现不需要 JDBC 的复杂代码以及 SQL 语句的这种功能的`

+ Hibernate 这里用到的思想是 **orm** 思想



####  ORM思想

什么是orm思想？

+ **object relational mapping** 对象关系映射
+ 文字描述：
  + 让实体类和数据库进行一一对应关系（在 web 阶段学习 javabean，更正确的叫法是”实体类 “），即实体类属性（成员变量）和数据库表字段对应
  + 不需要直接操作数据库表，而操作表对应实体类对象
  + 那么怎么让二者联系起来呢？**用配置文件即可**！
  + 之前学过一个比较小的框架 DBUtils，用 DBUtils 还是要写SQL语句，但是 Hibernate 中不需要SQL语句



#### 搭建Hibernate环境

+ ① **导入jar包**

+ ② **创建实体类**

  + ```java
    public class User {
      //Hibernate 中要求实体类有一个属性唯一的，比如id
      private int uid;
      private String username;
      private String password;
      private String address;
      public int getUid(){
        return uid;
      }
      public String getUsername(){
        return username;
      }
      public void setUsername(String username){
        this.username  = username;
      }
      public String getPassword(){
        return password;
      }
      public String getAddress(){
        return address;
      }
      //等其他setter方法
    }
    ```

  + 使用 Hibernate 时，不需要 自己手动创建表，Hibernate 帮助把表创建

+ ③ **使用 XML 配置文件配置实体类和数据库表，映射关系**

  + 其名称和位置没有固定要求，但是建议：在 实体类所在包中创建，建议名称 x.hbm.xml  ，比如 User.hbm.xml ，hbm 是hibernate mapping的缩写 

  + 在 Hibernate 配置文件中引入的约束 都是 DTD 约束

  + ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE hibernate-mapping PUBLIC 
    	"-//Hibernate/Hibernate Mapping DTD 3.0//EN"
    	"http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
    
    <hibernate-mapping>
      <class name="cn.itcast.entity.User" table="t_user">
        <!--id标签
        name属性：实体类中id属性名称
        column属性：-->
        <id name="uid" column="id">
          <!--设置数据库id增长策略
          native:生成id值就是主键自动增长-->
          <generator class="native"></generator>
        </id>
        
        <!--配置其他属性和表字段对应
        name属性：实体类属性名称
        column属性：表字段名称
    		type属性：设置生成表字段的类型，该标签一般不用，hibernate 会自动生成相对应的类型-->
        <property name="username" column="username" type=""></property>
        <property name="password" column="password"></property>
        <property name="address" column="address"></property>
        
      </class>
    </hibernate-mapping>
    ```

+ ④ **创建 Hibernate 核心配置文件**

  + 格式也是 XML ，但是其名称和位置是固定的

  + 位置：必须在 src 下面

  + 名称：必须是 hibernate.cfg.xml ，  cfg 是 configuration 的缩写

  + ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE hibernate-mapping PUBLIC 
    	"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
    	"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
    
    <hibernate-configuration>
      <session-factory>
        <!--① 配置数据库信息（必须的）-->
        <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</proprety>
        <property name="hibernate.connection.url">jdbc:mysql:///hibernate_day01</property>
        <property name="hibernate.connection.username">root</property>
        <property name="hibernate.connection.password">数据库密码</property>
        
        <!--② 配置 Hibernate 信息（可选的）-->
        <!--输出底层 SQL 语句-->
        <property name="hibernate.show_sql">true</property>
        <!--对底层 SQL 语句进行格式化-->
        <property name="hibernate.format_sql">true</property>
        <!--Hibernate 帮创建表，需要配置后
    		update:如果已有表，更新，如没有，创建-->
        <property name="hibernate.hbm2ddl.auto">update</property>
        <!--配置数据库方言
    		例如在MySQL里面实现分页，关键字limit，但是只能用于MySQL
    		配置之后，可以让hibernate 框架识别不同数据库语句，不同数据库配置内容不同-->
        <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
        
        <!--③ 把映射文件放到核心配置文件中（必须的）-->
        <!--映射文件路径名称-->
        <mapping resourcee="cn/itcast/entity/User.hbm.xml"/>
        
      </session-factory>
    </hibernate-configuration>
    ```

  + Hibernate 操作过程中，只会加载核心配置文件，其他配置文件不会加载（这就是为啥放到 src 里面的原因）

  + 这里的格式化是不是擦除数据的意思，是将SQL语句进行格式排列，使其比较美观，例如换行



#### 配置之后测试—实现添加操作

+ 其中前4步和6、7步是固定的，我们只需要写第5步

+ ① 加载 Hibernate 核心配置文件

+ ② 创建 SessionFactory 对象

+ ③ 使用SessionFactory 创建 session 对象

+ ④ 开启事务

+ ⑤ 写具体逻辑 crud 操作

+ ⑥ 提交事务

+ ⑦ 关闭资源

+ ```java
  import org.junit.Test;
  public class HibernateDemo{
    @Test
    public void testAdd(){
      //① 加载 Hibernate 核心配置文件
      //它会到 src 下面找到名称为 Hibernate.cfg.xml 的配置文件
      //创建Configuration 的对象，它会把配置文件放到对象里 
      //Configuration cfg = new Configuration();
      //cfg.configure();
      
      //② 创建 SessionFactory 对象
      //读取 Hibernate 核心配置文件内容，创建 sessionFactory
      //在核心配置文件中，有数据库配置，映射配置
      //在此过程中，它会根据映射关系配置文件，在配置数据库里将表创建
      
      //因为在创建 SessionFactory 的过程中，他又要创建表，这个过程特别耗资源，如何解决：
      //在 hibernate 操作中，建议一个项目一般创建一个 sessionFactory 对象，即单例 设计模式
      //具体实现，写一个类，类中写一个静态代码块（在底部）
      //SessionFactory SessionFactory = cfg.buildSessionFactory();
      //这样上面的步骤可以直接调用工具类代替即可
      SessionFactory sessionFactory = HibernateUtils.getSessionFactory();
      
      //③ 使用SessionFactory 创建 session 对象
      //类似于 JDBC 的连接 Connnection
      //调用 session 里面不同方法实现 crud 操作
      //添加：save方法、修改：update方法、删除：delete方法、根据id查询：get方法
      Session session = sessionFactory.openSession();
      
      //④ 开启事务
      Transaction tx = session.beginTransaction();
      
      //⑤ 写具体逻辑 crud 操作
      //创建对象
      User user = new User();
      user.setUsername("aks");
      user.setPassword("213");
      user.setAddress("巴黎");
      //调用session的方法实现添加
      session.save(user);
      
      //⑥ 提交事务
      //另外，事务回滚的方法是 tx.rollback();
      tx.commit();
      
      //⑦ 关闭资源
      session.close();
      sessionFactory.close();
    }
  }
  
  
  /////////////////写一个类实现单例设计模式/////////////////////
  public class HibernateUtils{
    private static final Configuration;
    private static final SessionFactory SessionFactory;
    
    static{ 
      Configuration = new Configuration().configure();
      SessionFactory = Configuration.buildSessionFactory(); 
    }
    
    public static SessionFactory getSessionFactory(){
      return sessionFactory;
    }
  }
  ```





第二天

+ 实体类的编写规则
+ Hibernate 主键生成策略
+ 实体类操作
  + crud 操作
  + 实体类对象状态
+ Hibernate 的一级缓存
+ Hibernate 的事务操作
  + 事务代码规则写法
+ Hiebernate 其他的API（查询）



#### 实体类编写规则

+ 属性私有
+ 私有属性要使用公有的 set、get 方法进行操作
+ 要求实体类要有属性作为唯一值（一般为id）
+ 实体类属性建议使用包装类，不建议使用基本数据类型
  + 比如 int  不能等于 null，包装类 Integer 就可以



####  Hibernate 主键生成策略

+ Hibernate 要求实体类里面有一个属性作为唯一值，对应主键，主键可以有不同的生成策略

+ Hibernate 主键生成策略有很多的值，比如 自动增长native

  + ```xml
    <generator class="native"></generator>
    ```

+ 在标签的class属性里面，还有 increment、identity、sequence、native、uuid。比较重要的有两个：native、uuid

+ **native： ** 对于不同的数据库MySQL、Oracle等，适用的参数不同，如何识别数据库类型，一般选择 **native** 会根据相应的数据库类型选择相应的参数

  + > 根据底层数据库对自动生成表示符的能力来选择 identity、sequence、hilo三种生成器中的一种，适合跨数据库平台开发。适用于代理主键。

+ **uuid：** 之前 Web 阶段需要我们自己写代码来生成uuid值，现在Hibernate 会帮我们生成uuid值

  + > Hibernate 采用128位的 UUID 算法来生成标识符。该算法能够在网络环境中生成唯一的字符串标识符，其UUID被编码为一个长度为32位的十六进制字符串。这种策略并不流行，因为字符串类型的主键比整数类型的主键占用更多的数据库空间。适用于代理主键。

  + 使用 uuid 生成策略，实体类id属性类型必须是字符串类型



#### 实体类操作

+ 调用 session 里面的save方法实现

  + 添加操作

    + ```java
      //⑤ 写具体逻辑 crud 操作
      //创建对象
      User user = new User();
      user.setUsername("aks");
      user.setPassword("213");
      user.setAddress("巴黎");
      //调用session的方法实现添加
      session.save(user);
      ```

  + 根据id查询

    + ```java
      public void testGet(){
        SessionFactory sessionFactory = HibernateUtils.getSessionFactory();
        Session session = sessionFactory.openSession();
        Transaction tx = session.beginTransaction();
        
        //调用session里面的get方法
        //第一个参数：实体类的class
        //第二个参数：id值
        session.get(User.class,1);
        
        tx.commit();
        session.close();
        sessionFactory.close();
      }
      ```

  + 修改操作

    + ```mysql
      UPDATE t_user SET username=?,sddress=? WHERE uid=?;
      ```

    + 以上是MySQL语句

    + 用Hibernate ，首先根据 id 查询，返回对象，再修改

    + ```java
      User user = session.get(User.class,2);
      
      ```






