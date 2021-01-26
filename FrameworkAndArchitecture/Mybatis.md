[![](https://img.shields.io/badge/MyBatis-English-blue)](https://mybatis.org/mybatis-3/)[![](https://img.shields.io/badge/MyBatis-Chinese-blue)](https://mybatis.org/mybatis-3/zh/getting-started.html)[![](https://img.shields.io/badge/MyBatis-W3Cschool-orange)](https://www.w3cschool.cn/mybatis/)



+ Mybatis 的重点及难点是其技术架构和重要组成部分，以及基本运行原理。

## 面试题

[JavaGuide](https://snailclimb.gitee.io/javaguide-interview/#/./docs/e-2mybatis?id=_52-mybatis%e9%9d%a2%e8%af%95%e9%a2%98%e6%80%bb%e7%bb%93)

### #{}  ${}

> **#{} 和 ${} 的区别**

- `#{}`是 sql 的参数占位符，Mybatis 会将 sql 中的`#{}`替换为?号，在 sql 执行前会使用 PreparedStatement 的参数设置方法，按序给 sql 的 ? 号占位符设置参数值，比如 ps.setInt(0, parameterValue)，`#{item.name}` 的取值方式为使用反射从参数对象中获取 item 对象的 name 属性值，相当于 `param.getItem().getName()`。
- `${}`是 properties 文件中的变量占位符，它可以用于标签属性值和 sql 内部，属于静态文本替换，比如${driver}会被静态替换为`com.mysql.jdbc.Driver`。

> **使用 #{} 和 ${} 向sql传参时的区别**

+ 首先一点就是，#{} 传递参数时，会在传递的参数上加上引号，在传递属性比如  name=？ 时，可以很方便的使用 #{}

	而 ${} 则不会添加引号，传递的是什么就会直接放到SQL中去执行。

+ #{} 传递的参数实际上是通过占位符去传入到已经预编译好的 SQL 中去的，所以此时的SQL已经完成编译，只需要传参数就完成执行了。而`${}`是直接将参数拼接成完整的SQL去DBMS中编译执行的。

	所以#{}方式实际上比 `${}` 方式更加安全，不会引起SQL注入。但是在传入表名参数时，只能使用 ${}，这时候，必须要在接受参数的时候加入逻辑判断，判断参数中是否存在SQL语句，以防引起注入。



### XML 标签

> XML 映射文件中的标签有哪些？

首先来看一个完整的mapper文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.site.blog.my.core.dao.BlogCategoryMapper">
  
    <resultMap id="BaseResultMap" type="com.site.blog.my.core.entity.BlogCategory">
        <id column="category_id" jdbcType="INTEGER" property="categoryId"/>
        <result column="category_name" jdbcType="VARCHAR" property="categoryName"/>
        <result column="category_icon" jdbcType="VARCHAR" property="categoryIcon"/>
        <result column="category_rank" jdbcType="INTEGER" property="categoryRank"/>
        <result column="is_deleted" jdbcType="TINYINT" property="isDeleted"/>
        <result column="create_time" jdbcType="TIMESTAMP" property="createTime"/>
    </resultMap>

    <sql id="Base_Column_List">
    category_id, category_name, category_icon, category_rank, is_deleted, create_time
    </sql>

    <select id="findCategoryList" parameterType="Map" resultMap="BaseResultMap">
        select
        <include refid="Base_Column_List"/>
        from tb_blog_category
        where is_deleted=0
        order by category_rank desc,create_time desc
        <if test="start!=null and limit!=null">
            limit #{start},#{limit}
        </if>
    </select>

    <select id="getTotalCategories" parameterType="Map" resultType="int">
    select count(*) from tb_blog_category
    where is_deleted=0
    </select>

    <select id="selectByPrimaryKey" parameterType="java.lang.Integer" resultMap="BaseResultMap">
        select
        <include refid="Base_Column_List"/>
        from tb_blog_category
        where category_id = #{categoryId,jdbcType=INTEGER} AND is_deleted = 0
    </select>

    <select id="selectByCategoryIds" resultMap="BaseResultMap">
        select
        <include refid="Base_Column_List"/>
        from tb_blog_category
        where category_id IN
        <foreach collection="categoryIds" item="item" index="index"
                 open="(" separator="," close=")">#{item}
        </foreach>
        AND is_deleted = 0
    </select>

    <select id="selectByCategoryName" parameterType="java.lang.String" resultMap="BaseResultMap">
        select
        <include refid="Base_Column_List"/>
        from tb_blog_category
        where category_name = #{categoryName,jdbcType=VARCHAR} AND is_deleted = 0
    </select>
    <update id="deleteByPrimaryKey" parameterType="java.lang.Integer">
    UPDATE tb_blog_category SET  is_deleted = 1
    where category_id = #{categoryId,jdbcType=INTEGER} AND is_deleted = 0
    </update>
    <update id="deleteBatch">
        update tb_blog_category
        set is_deleted=1 where category_id in
        <foreach item="id" collection="array" open="(" separator="," close=")">
            #{id}
        </foreach>
    </update>
    <insert id="insert" parameterType="com.site.blog.my.core.entity.BlogCategory">
    insert into tb_blog_category (category_id, category_name, category_icon, 
      category_rank, is_deleted, create_time
      )
    values (#{categoryId,jdbcType=INTEGER}, #{categoryName,jdbcType=VARCHAR}, #{categoryIcon,jdbcType=VARCHAR}, 
      #{categoryRank,jdbcType=INTEGER}, #{isDeleted,jdbcType=TINYINT}, #{createTime,jdbcType=TIMESTAMP}
      )
  </insert>
    <insert id="insertSelective" parameterType="com.site.blog.my.core.entity.BlogCategory">
        insert into tb_blog_category
        <trim prefix="(" suffix=")" suffixOverrides=",">
            <if test="categoryId != null">
                category_id,
            </if>
            <if test="categoryName != null">
                category_name,
            </if>
            <if test="categoryIcon != null">
                category_icon,
            </if>
            <if test="categoryRank != null">
                category_rank,
            </if>
            <if test="isDeleted != null">
                is_deleted,
            </if>
            <if test="createTime != null">
                create_time,
            </if>
        </trim>
        <trim prefix="values (" suffix=")" suffixOverrides=",">
            <if test="categoryId != null">
                #{categoryId,jdbcType=INTEGER},
            </if>
            <if test="categoryName != null">
                #{categoryName,jdbcType=VARCHAR},
            </if>
            <if test="categoryIcon != null">
                #{categoryIcon,jdbcType=VARCHAR},
            </if>
            <if test="categoryRank != null">
                #{categoryRank,jdbcType=INTEGER},
            </if>
            <if test="isDeleted != null">
                #{isDeleted,jdbcType=TINYINT},
            </if>
            <if test="createTime != null">
                #{createTime,jdbcType=TIMESTAMP},
            </if>
        </trim>
    </insert>
    <update id="updateByPrimaryKeySelective" parameterType="com.site.blog.my.core.entity.BlogCategory">
        update tb_blog_category
        <set>
            <if test="categoryName != null">
                category_name = #{categoryName,jdbcType=VARCHAR},
            </if>
            <if test="categoryIcon != null">
                category_icon = #{categoryIcon,jdbcType=VARCHAR},
            </if>
            <if test="categoryRank != null">
                category_rank = #{categoryRank,jdbcType=INTEGER},
            </if>
            <if test="isDeleted != null">
                is_deleted = #{isDeleted,jdbcType=TINYINT},
            </if>
            <if test="createTime != null">
                create_time = #{createTime,jdbcType=TIMESTAMP},
            </if>
        </set>
        where category_id = #{categoryId,jdbcType=INTEGER}
    </update>
    <update id="updateByPrimaryKey" parameterType="com.site.blog.my.core.entity.BlogCategory">
    update tb_blog_category
    set category_name = #{categoryName,jdbcType=VARCHAR},
      category_icon = #{categoryIcon,jdbcType=VARCHAR},
      category_rank = #{categoryRank,jdbcType=INTEGER},
      is_deleted = #{isDeleted,jdbcType=TINYINT},
      create_time = #{createTime,jdbcType=TIMESTAMP}
    where category_id = #{categoryId,jdbcType=INTEGER}
    </update>
</mapper>
```



`<insert>、<delete>、<update>、<select>` 

+ 增删改查



`<mapper>` 

+ 命名空间 = dao接口

+ 例如：`<mapper namespace="com.site.blog.my.core.dao.AdminUserMapper">`



`<resultType>`、`<parameterType>` 、`<resultMap>` `<parameterMap>`

当实体类中的属性和数据库中的字段对应时，使用 resultType 和 parameterType 就可以完成CRUD；

当实体类中的属性和数据库中的字段不对应时，就要用 resultMap 和 parameterMap 了。

`<resultMap>`、`<result>`

+ 例如

```xml
<resultMap id="BaseResultMap" type="com.site.blog.my.core.entity.AdminUser">
    <id column="admin_user_id" jdbcType="INTEGER" property="adminUserId" />
    <result column="login_user_name" jdbcType="VARCHAR" property="loginUserName" />
    <result column="login_password" jdbcType="VARCHAR" property="loginPassword" />
    <result column="nick_name" jdbcType="VARCHAR" property="nickName" />
    <result column="locked" jdbcType="TINYINT" property="locked" />
</resultMap>
```

+ type = 实体类全限定名
+ column = 数据库表列名
+ property = 实体类字段名



`<sql>`、`<include>`

sql 标签的作用是 SQL代码的重用

首先声明sql重用代码段：

```xml
<sql id="Base_Column_List">
  	category_id, category_name, category_icon, category_rank, is_deleted, create_time
</sql>
```

然后在 select 查询语句中进行使用，在需要使用sql的内容时使用include标签，在refid中填写上述定义的sql的id名即可例如如下的select查询的信息，

```xml
<select id="findCategoryList" parameterType="Map" resultMap="BaseResultMap">
    select
    <include refid="Base_Column_List"/>
    from tb_blog_category
    where is_deleted=0
    order by category_rank desc,create_time desc
    <if test="start!=null and limit!=null">
      	limit #{start},#{limit}
    </if>
</select>
```



`trim|where|set|foreach|if|choose|when|otherwise|bind`

+ 动态 sql 的 9 个标签 



`<selectKey>`

+ 不支持自增的主键生成策略标签



### MyBatis 分页 ??

> Mybatis 是如何进行分页的？
>
> 分页插件的原理是什么？

Mybatis 使用 RowBounds 对象进行分页，它是针对 ResultSet 结果集执行的内存分页，而非物理分页，可以在 sql 内直接书写带有物理分页的参数来完成物理分页功能，也可以使用分页插件来完成物理分页。

分页插件的基本原理是使用 Mybatis 提供的插件接口，实现自定义插件，在插件的拦截方法内拦截待执行的 sql，然后重写 sql，根据 dialect 方言，添加对应的物理分页语句和物理分页参数。

+ 举例：`select _ from student`，
+ 拦截 sql 后重写为：`select t._ from （select \* from student）t limit 0，10`



### Mybatis 插件 ??

> 简述 Mybatis 的插件运行原理？
>
> 如何编写一个插件？





### MyBatis 批量插入

[原文](https://www.imooc.com/article/details/id/284701)

> **SQL 层面实现批量插入**

**单条插入数据**

```sql
INSERT INTO [tableName] ([columnName1],[columnName2]) 
VALUES ([columnValue1],[columnValue2]);
```

或者

```sql
INSERT INTO [tableName] VALUES ([columnValue1],[columnValue2]);
```

**批量插入**

```sql
INSERT INTO [tableName] ([columnName1],[columnName2])
VALUES 
([columnValue1],[columnValue2]),
([columnValue3],[columnValue4]),
([columnValue5],[columnValue6]);
```

+ 批量的好处：可以避免程序和数据库建立多次连接，从而增加服务器负荷。

> **MyBatis 实现批量插入**

MyBatis批量插入数据到数据库有两种方式：xml文件，注解。

1. mapping.xml 中 insert 语句可以写成单条插入，在调用方循环1000次

```xml
<insert id="insert" parameterType="com.xxp.mybatis.Person">
    insert into person (id, name,sex,address)
    values 
    (#{id,jdbcType=INTEGER},
     #{name,jdbcType=VARCHAR},
     #{sex,jdbcType=VARCHAR},
     #{address,jdbcType=VARCHAR})
</insert>
```

2. mapping.xml 中 insert 语句写成一次性插入一个1000的 list

```xml
<insert id="insertBatch" >
    insert into person (<include refid="Base_Column_List"/>) 
    values 
    <foreach collection="list" item="item" index="index" separator=",">
        (null,#{item.name},#{item.sex},#{item.address})
    </foreach>
</insert>
```





## MyBatis_DAO_XML

> MyBatis 中的 DAO 接口和 XML 文件里的 SQL 是如何建立关系的？

+ [原文](http://www.mamicode.com/info-detail-2676941.html)

+ [SqlSessionFactory接口 和 SqlSession接口是干嘛的](https://blog.csdn.net/u013412772/article/details/73648537)

MyBatis的操作大致可以分为以下几个步骤：

+ ① 读取配置文件
	+ MyBatis配置文件mybatis-config.xml，不是Mapper.xml
	+ 配置文件中包含「配置环境、JDBC事务管理、数据库连接池」等配置
+ ② 根据配置文件构建 SqlSessionFactory 实例
+ ③ 通过 SqlSessionFactory 创建 SqlSession 实例
+ ④ 使用 SqlSession 对象操作数据库（增删改查及提交事务等）
+ ⑤ 关闭 SqlSession

代码如下：

```java
public class MybatisTest{
  	@Test
  	public void findUserByIdTest() throws Exception{
      	//① 读取配置文件
      	String resource = "mybatis-config.xml";
      	InputStream inputStream = Resources.getResourceAsStream(resource);
      	//② 根据配置文件构建 SqlSessionFactory 实例
      	SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
      	//③ 通过 SqlSessionFactory 创建 SqlSession 实例
      	SqlSession sqlSession = sqlSessionFactory.openSession();
      	//④ 使用 SqlSession 对象操作数据库
      	User user = sqlSession.selectOne("com.ssm.mapper.UserMapper.findUserById",3);
      	System.out.println(user.toString());
      	//⑤ 关闭 SqlSession
      	sqlSession.close
    }
}
```



## MyBatis动态SQL

举个例子：对于一些复杂的查询，我们可能会指定多个查询条件，但是这些条件可能存在也可能不存在，例如在58同城上面找房子，我们可能会指定面积、楼层和所在位置来查找房源，也可能会指定面积、价格、户型和所在位置来查找房源，此时就需要根据用户指定的条件动态生成SQL语句。如果不使用持久层框架我们可能需要自己拼装SQL语句。

传统jdbc方法中，在写组合的多表复杂sql语句时，需要去拼接sql语句，稍不注意少写一个空格或“”，就会导致报错。

所以MyBatis提供了动态SQL的功能很好地解决了上述问题。

Mybatis动态sql语言可以被用在任意的sql语句映射中。

Mybatis采用强大的功能基于OGNL的表达式消除其他元素。





## Project & Module Of IntelliJ IDEA 

[原文](https://blog.csdn.net/qq_35246620/article/details/65448689)

在 IntelliJ IDEA 中，没有类似于 Eclipse 工作空间（`Workspace`）的概念，而是提出了`Project`和`Module`这两个概念。接下来，就让我们一起看看 IntelliJ IDEA 官方是如何描述两者的吧！

**对于 Project，IntelliJ IDEA 官方是这样介绍的**：

`A project is a top-level organizational unit for your development work in IntelliJ IDEA. In its finished form, a project may represent a complete software solution. A project is a collection of:`

- `Your work results: source code, build scripts, configuration files, documentation, artifacts, etc.`
- `SDKs and libraries that you use to develop, compile, run and test your code.`
- `Project settings that represent your working preferences in the context of a project.`

`A project has one or more modules as its parts.`

**对于 Module，IntelliJ IDEA 官方是这样介绍的**：

- `A module is a part of a project that you can compile, run, test and debug independently.`
- `Modules are a way to reduce complexity of large projects while maintaining a common (project) configuration.`
- `Modules are reusable: if necessary, a module can be included in more than one project.`

通过上面的介绍，我们知道：在 IntelliJ IDEA 中`Project`是最顶级的结构单元，然后就是`Module`，一个`Project`可以有多个`Module`。目前，主流的大型项目结构基本都是多`Module`的结构，这类项目一般是按功能划分的，比如：`user-core-module`、`user-facade-module`和`user-hessian-module`等等，模块之间彼此可以相互依赖。通过这些`Module`的命名可以看出，它们都是处于同一个项目中的模块，彼此之间是有着不可分割的业务关系。因此，我们可以大致总结出：一个`Project`是由一个或多个`Module`组成。

- 当为单`Module`项目的时候，这个单独的`Module`实际上就是一个`Project`；
- 当为多`Module`项目的时候，多个模块处于同一个`Project`之中，此时彼此之间具有互相依赖的关联关系。

此外， IntelliJ IDEA 的`Project`是一个不具备任何编码设置、构建等开发功能的概念，其主要作用就是起到一个项目定义、范围约束、规范类型的效果，或许，我们也可以简单地理解`Project`就是一个单纯的目录，只是这个目录在命名上必须有其代表性的意义。在缺省情况下，IntelliJ IDEA 是默认单`Project`单`Module`的，这时`Project`和`Module`合二为一，在没有修改存储路径的时候，显然`Project`对`Module`具有强约束作用！不过说实话，这里就是将`Module`的内容放在了`Project`的目录下，实际上还是`Module`自己约束自己。



## XML -> DAO

[Mybatis中的Mapper接口和XML文件里的SQL是如何建立关系的？](https://www.cnblogs.com/626zch/p/10776985.html)

> XML映射文件通常会与DAO接口一一对应，那么：
>
> 这个DAO接口的工作原理是什么？
>
> 参数不同时，方法能重载吗？

在 Mybatis 中，每一个`<select>`、`<insert>`、`<update>`、`<delete>`标签，都会被解析为一个`MappedStatement`对象。

Dao 接口，就是常说的 `Mapper` 接口

+ 接口的全限名，就是映射文件中的 namespace 的值；
+ 接口的方法名，就是映射文件中`MappedStatement`的 id 值；
+ 接口方法内的参数，就是传递给 sql 的参数。

`Mapper`接口是没有实现类的，当调用接口方法时，接口全限名+方法名拼接字符串作为 key 值，可唯一定位到一个`MappedStatement`。

+ 举例：`com.mybatis3.mappers.StudentDao.findStudentById`，可以唯一找到 namespace 为`com.mybatis3.mappers.StudentDao`下面`id = findStudentById`的`MappedStatement`。

Dao 接口里的方法，是不能重载的，因为是全限名+方法名来保证寻找策略。

+ 重载是方法名相同，参数不同
+ 但是 全限名+方法名 可唯一定位到一个`MappedStatement`对象，无法确定参数

Dao 接口的工作原理是 JDK 动态代理，Mybatis 运行时会使用 JDK 动态代理为 Dao 接口生成代理 proxy 对象，代理对象 proxy 会拦截接口方法，转而执行`MappedStatement`所代表的 sql，然后将 sql 执行结果返回。



























