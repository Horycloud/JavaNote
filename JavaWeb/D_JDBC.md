[Java--JDBC连接数据库](https://www.cnblogs.com/yangming1996/articles/6666217.html)

### MySQL语法



`查看当前使用的是哪个数据库`

+ ```mysql
  select database(); 
  ```

   

`创建user及表结构id, username, password`

+ ```mysql
  creat  table user(id int primary key auto_increment, username varchar(16),  password varchar(16));
  ```



`查看表结构`

+ ```mysql
  desc  user;
  ```



`插入`

+ ```mysql
  -- 通常是 
  insert  into user (username,password) values ('zhr','151869' );
  ```

+ ```mysql
  -- 也可以 
  isert  into user values (null, 'zhr','151869' );
  isert  into user values (null, 'sss','123' );
  ```



`查看表内容`

+ ```mysql
  selcet * from user;
  select * from user where username = 'xx' and password = 'xx' ;
  select *  from 
  ```



`分页查询`

+ limit [位置偏移量]行数

+ 位置偏移量是从哪一行开始 (行数从0开始)，行数是指查询几行

+ 如果要查询 第7页，每页8行

+ 起始和末尾行数

+ 0-7 第一页

+ 8-15 第二页

+ 16-23 第三页

+ ...

+ 起始行数为   (页数-1)* 行数

+ ```mysql
  # 比如查询第9页的所有行
  SELECT * FROM web01.user limit 64,8;
  ```



`删除`

+ delect  from user ;  此语句慎用，可直接删除表中的所有信息

通常使用where删除指定行

+ ```mysql
  delect  from user where id=3;
  ```

+ 删除之后再添加的话，id是一直增长的，而不是从1开始



`修改`

+ ```mysql
  update user set username = 'hr' where id = 5 ; 
  ```

+ 不加后面 的 where的话，会 将 所有的username都改为'hr'



`修改表名`

+ ```mysql
  rename  table user to student;
  ```



### JDBC

```java
import java.sql.DriverManager;
import java.sql.Connection;
import java.SQLException;
import java.sql.Statement;

public class JDBCDemo{
  public static void main(String[] args){
    selectAll();
    selectByUsernamePassword("Micheal","123");
    selectUserByPage(3,4);
  }
}
```

```java
public  static void selectAll(){
    Connection con = null;
    Statement stmet = null;
    ResultSet rs = null;
    try{
      Class.forName("com.mysql.jdbc.Driver");//① 是用什么驱动连接数据库
      //String url = "jdbc:mysql://localhost:3306/web01"; //一般写法
      String url = "jdbc:mysql://localhost:3306/web01?useUnicade=true&characterEncoding=UTF8&useSSL=false"; //指定编码的写法
      String user = "root";
      String password = "root";
      con = DriverManager.getConnection(url,user,password); //② 建立连接
      stmt = con.creatStatement(); //③ 发起请求
      rs = stmt.executeQuery("select * from user");//发起请求之后得到结果集。其他命令executxxx
      while(rs.next()){
        System.out.println(rs.getInt(1)+","+rs.getString(2)+","+rs.getString(3));//表的列的索引是从1开始的，rs.getInt(1)表示获取第一列的数据
        System.out.println(rs.getInt("id")+","+rs.getString("username")+","+rs.getString("password"));//通过表 头获取列数据
      } //④ 对结果集进行处理
    }catch(Exception e){
      e.printStackTrace();
    }finally{
      try{
        if(rs!=null)rs.close();
      }catch(SQLException e){
        e.printStackTrace();
      }
      
      try{
        if(stmt!=null)stmt.close();
      }catch(SQLException e){
        e.printStackTrace();
      }
      
      try{
        if(con!=null)con.close();
      }catch(SQLException e){
        e.printStackTrace();
      }
    }
}
```

```java
//该方法容易产生SQL注入
//如果黑客执行 selectByUsernamePassword("Micheal","123'or'1'='1"); //多出来的单引号是为了跟SQL语句中的末尾单引号拼接的
public static boolean selectByUsernamePassword(String username,String password){
  try{
    Class.forName("com.mysql.jdbc.Driver");
    String url = "jdbc:mysql://localhost:3306/web01?useUnicade=true&characterEncoding=UTF8&useSSL=false";
    String user = "root";
    String password = "root";
    con = DriverManager.getConnection(url,user,password);
    stmt = con.creatStatement();
    
    String sql = "selcet * from  user where username = '"+username+"' and password = '"+password+"'";
    rs = stmt.executeQuery(sql);
    if(rs.next()){
      return true;  //表示查询到了这个用户 
    }else{
      return false;
    }
  }catch(Exception e){
    e.printStackTrace();
  }finally{
    try{
        if(rs!=null)rs.close();
      }catch(SQLException e){
        e.printStackTrace();
      }
      
      try{
        if(stmt!=null)stmt.close();
      }catch(SQLException e){
        e.printStackTrace();
      }
      
      try{
        if(con!=null)con.close();
      }catch(SQLException e){
        e.printStackTrace();
      }
  }
  
  return false;
}
```

```java
//该方法解决SQL注入问题
public static boolean selectByUsernamePassword(String username,String password){
  try{
    Class.forName("com.mysql.jdbc.Driver");
    String url = "jdbc:mysql://localhost:3306/web01?useUnicade=true&characterEncoding=UTF8&useSSL=false";
    String user = "root";
    String password = "root";
    con = DriverManager.getConnection(url,user,password);
    
    /*将原来的注释掉
    stmt = con.creatStatement();
    String sql = "selcet * from  user where username = '"+username+"' and password = '"+password+"'";
    rs = stmt.executeQuery(sql);
    if(rs.next()){
      return true;  //表示查询到了这个用户 
    }else{
      return false;
    }
    */
    
    String  sql = "select * from user where username = ? and password = ?";
    PrepareStatement pstmt = con.prepareStatement(sql);
    
    pstmt.setString(1,username); //1为索引，pstmt.setString(parameterIndex,x);
    pstmt.setString(2,password);
    
    rs = pstmt.executeQuery();
    if(rs.next()){
      return true;  
    }else{
      return false;
    }
  }catch(Exception e){
    e.printStackTrace();
  }finally{
    try{
        if(rs!=null)rs.close();
      }catch(SQLException e){
        e.printStackTrace();
      }
      
      try{
        if(stmt!=null)pstmt.close();
      }catch(SQLException e){
        e.printStackTrace();
      }
      
      try{
        if(con!=null)con.close();
      }catch(SQLException e){
        e.printStackTrace();
      }
  }
  
  return false;
}
```

```java
//分页查询
//pageNumber：查询第几页；pageCount：查询此页前pageCount行数据
public static void selectUserByPage(int pageNumber,int pageCount){
    Connection con = null;
    PrepareStatement pstmet = null;  //添加参数使用PrepareStatement
    ResultSet rs = null;
    try{
      Class.forName("com.mysql.jdbc.Driver");
      String url = "jdbc:mysql://localhost:3306/web01?useUnicade=true&characterEncoding=UTF8&useSSL=false"; 
      String user = "root";
      String password = "root";
      con = DriverManager.getConnection(url,user,password); 
      pstmt = con.prepareStatement("select * from user limit ?,?"); 
      pstmt.setInt(1,(pageNumber-1)*pageCount);
      pstmt.setInt(2,pageCount);
      rs = pstmt.executeQuery();
      
      while(rs.next()){
        System.out.println(rs.getInt(1)+","+rs.getString(2)+","+rs.getString(3));
      } 
    }catch(Exception e){
      e.printStackTrace();
    }finally{
      try{
        if(rs!=null)rs.close();
      }catch(SQLException e){
        e.printStackTrace();
      }
      
      try{
        if(stmt!=null)pstmt.close();
      }catch(SQLException e){
        e.printStackTrace();
      }
      
      try{
        if(con!=null)con.close();
      }catch(SQLException e){
        e.printStackTrace();
      }
    }
}
```



### JDBCUtils

#### 提取工具类—JDBCUtils

+ `以上代码有好多重复的部分，比如关闭资源等，下面将上面的代码进行改进，对重复的代码进行封装，简化代码`

+ `把上面重复的代码封装进工具类里面`

```java
//对于工具类，我们一般设置方法为静态方法，要不然调用的时候还要再new一个对象，比较麻烦
public class JDBCUtils{
  
  //对于一些比较常修改的参数，我们把它提取为成员变量，方便以后的维护
  private static final String connectionURL = "jdbc:mysql://localhost:3306/web01?useUnicade=true&characterEncoding=UTF8&useSSL=false";
  private static final username = "root";
  private static final password = "root";
  
  //连接池，又称数据源
  //这里是我们自己做的连接池(比较简陋)，现在都有第三方插件(如:dbcp、c3p0，功能比较完善)，我们直接调用即可
  private static List<Connection> conList = new ArrayList<>();
  //当整个程序加载的时候会首先执行静态代码块，它是先于静态方法执行的
  //提前创建好连接，当用到的时候直接在连接池中获取，而不用每次创建新的连接
  static{
    for(int i=0;i<5;i++){
      Connection con = creatConnection();
      conList.add(con);
    }
  }

  public static Connection getConnection(){
    if(conList.isEmpty()==false){
      Connection con = conList.get(0);
      conList.remove(con);
      return con;
    }else{
      return creatConnection();  //如果连接池里面没有空闲的连接，则创建一个新的连接，这个时候创建的新的连接不用放到连接池里面去，直接使用即可
    }
  }
  
  private static Connection createConnection(){
    try{
      Class.forName("com.mysql.jdbc.Driver");  
      return DriverManager.getConnection(connectionURL,username,password); 
    }catch(Exception e){
      e.printStackTrace();
    }
    
    return null;
  }
  
  public static void close(ResultSet rs,Statement stmt,Connection con){
    closeResultSet(rs);
    closeStatement(stmt);
    closeConnection(con);
  }
  
  //该方法专用于后面的转账操作
  public static void closeExchange(Statement stmt1,Statement stmt2,Connection con){
    closeStatement(stmt1);
    closeStatement(stmt2);
    closeConnection(con);
  }

  private static void closeResultSet(ResultSet rs){
   	  try{
        if(rs!=null)rs.close();
      }catch(SQLException e){
        e.printStackTrace();
      }
  }
  private static void closeStatement(Statement stmt){
    	//注意：Statement是java.sql下的Statement
  	  try{
        if(stmt!=null)stmt.close();
      }catch(SQLException e){
        e.printStackTrace();
      }
  }
  private static void closeConnection(Connection con){
   	  /*
   	  try{
        if(con!=null)con.close();
      }catch(SQLException e){
        e.printStackTrace();
      }  
   	  */
    	conList.add(con); //归还连接的时候并不需要关闭，只需要放回连接池即可
    	
  }
}
```



#### 简化之后的方法

```java
public  static void selectAll(){
    Connection con = null;
    Statement stmet = null;
    ResultSet rs = null;
    try{
      con = JDBCUtils.getConnection();  //直接调用
      stmt = con.creatStatement(); 
      rs = stmt.executeQuery("select * from user");
      while(rs.next()){
        System.out.println(rs.getInt(1)+","+rs.getString(2)+","+rs.getString(3));
        System.out.println(rs.getInt("id")+","+rs.getString("username")+","+rs.getString("password"));
      } 
    }catch(Exception e){
      e.printStackTrace();
    }finally{
      JDBCUtils.close(rs,stmt,con);
    }
}
```



```java
//解决了SQL注入问题的selectByUsernamePassword方法
public static boolean selectByUsernamePassword(String username,String password){
  try{
    con = JDBCUtils.getConnection();  //直接调用
    String  sql = "select * from user where username = ? and password = ?";
    PrepareStatement pstmt = con.prepareStatement(sql);
    
    pstmt.setString(1,username); //1为索引，pstmt.setString(parameterIndex,x);
    pstmt.setString(2,password);
    
    rs = pstmt.executeQuery();
    if(rs.next()){
      return true;  
    }else{
      return false;
    }
  }catch(Exception e){
    e.printStackTrace();
  }finally{
    JDBCUtils.close(rs,stmt,con);
  }
  
  return false;
}
```

```java
//分页查询
//pageNumber：查询第几页；pageCount：查询此页前pageCount行数据
public static void selectUserByPage(int pageNumber,int pageCount){
    Connection con = null;
    PrepareStatement pstmet = null;  //添加参数使用PrepareStatement
    ResultSet rs = null;
    try{
      con = JDBCUtils.getConnection();  //直接调用
      pstmt = con.prepareStatement("select * from user limit ?,?"); 
      pstmt.setInt(1,(pageNumber-1)*pageCount);
      pstmt.setInt(2,pageCount);
      rs = pstmt.executeQuery();
      
      while(rs.next()){
        System.out.println(rs.getInt(1)+","+rs.getString(2)+","+rs.getString(3));
      } 
    }catch(Exception e){
      e.printStackTrace();
    }finally{
      JDBCUtils.close(rs,pstmt,con);
    }
}
```

```java
//插入操作
public static void insert(String username,String password){
    Connection con = null;
    PrepareStatement pstmet = null;  
    ResultSet rs = null;
    try{
      con = JDBCUtils.getConnection();  
      String sql = "insert into user(username,password) values(?,?)";
      pstmt = con.prepareStatement(sql); 
      pstmt.setString(1,username);
      pstmt.setString(2,password);
      int result = pstmt.executeUpdate();  //该返回值代表受到影响的行数
      } 
    }catch(Exception e){
      e.printStackTrace();
    }finally{
      JDBCUtils.close(rs,pstmt,con);
    }
}
```

```java
//删除操作
public static void delete(int id){
  	Connection con = null;
    PrepareStatement pstmet = null;  
    ResultSet rs = null;
    try{
      con = JDBCUtils.getConnection();  
      String sql = "delete form user where id = ?";
      pstmt = con.prepareStatement(sql); 
      pstmt.setInt(1,id);
      int result = pstmt.executeUpdate();  
      if(result>0){
        System.out.println("删除成功");
      }else{
        System.out.println("删除失败");
      }
    }catch(Exception e){
      e.printStackTrace();
    }finally{
      JDBCUtils.close(rs,pstmt,con);
    }
}
```

```java
//修改
public static void update(int id,String newPassword){
  	Connection con = null;
    PrepareStatement pstmet = null;  
    ResultSet rs = null;
    try{
      con = JDBCUtils.getConnection();  
      String sql = "update user set password = ? where id = ?";
      pstmt = con.prepareStatement(sql); 
      pstmt.setString(1,newPassword);
      pstmt.setInt(2,id);
      int result = pstmt.executeUpdate();  
      if(result>0){
        System.out.println("修改成功");
      }else{
        System.out.println("修改失败");
      }
    }catch(Exception e){
      e.printStackTrace();
    }finally{
      JDBCUtils.close(rs,pstmt,con);
    }
}
```

```java
//用事务实现转账
public static void transferAccounts(String username1,String username2,int money){
 	  Connection con = null;
    PrepareStatement pstmet1 = null; 
 	  PrepareStatement pstmet2 = null;
    ResultSet rs = null;
    try{
      con = JDBCUtils.getConnection();  
      con.setAutoCommit(false); //开启事务
      
      String sql = "update user set balance = balance-? where username = ?";
      pstmt1 = con.prepareStatement(sql); 
      pstmt1.setInt(1,money);
      pstmt1.setString(2,username1);
      pstmt1.executeUpdate();
      
      //String s = null;
      //s.charAt(2);
      
      sql = "update user set balance = balance+? where username = ?";
      pstmt2 = con.prepareStatement(sql); 
      pstmt2.setInt(1,money);
      pstmt2.setString(2,username2);
      pstmt2.executeUpdate();
      
      con.commit(); //提交事务

    }catch(Exception e){
      e.printStackTrace();
    }finally{
      JDBCUtils.closeExchange(pstmt2,pstmt1,con); //后创建的先关闭
    }
  
}
```



#### 连接池

+ `Connection 是可以重复利用的，当有大量用户都在进行增删改查时，不必每个用户开启—操作—关闭，可以开启，然后多个用户操作之后再关闭，这样即提高了效率。`
+ `DPCP依赖于另两个包`
  + `Apache Commons Pool`
  + `Apache Commons Logging`

+ `现在，做一个连接池，去管理所有用户创建的连接Connection，把没有用的连接放在连接池里面，有人用到就从连接池里取出来，但是不要关闭这个连接，这样就提高的性能。`

+ `可以在前面的JDBCUtils里创建连接池`



连接池第三方插件—DBCP

+ 可以在Apache官网下载（有Binaries和Source两种版本，Source可以查看源代码）
+ 下载之后可以导入JAR包到项目中

```java
//之前我们自己创建的连接池，连接用过之后还要归还
//使用DPCP则不用归还，直接close即可
public Class DBCPDataSource{
  private static final String connectionURL = "jdbc:mysql://localhost:3306/web01?useUnicade=true&characterEncoding=UTF8&useSSL=false";
  private static final username = "root";
  private static final password = "root";
  
  private static BasicDataSource ds;
  
  static{
    //初始化DPCP数据源
    BasicDataSource ds = new BasicDataSource();
    ds.setDriverClassName("com.mysql.jdbc.Driver");
    ds.setUrl(connectionURL);
    ds.setUsername(username);
    ds.setPassword(password);
    
    ds.setInitialSize(5);
    ds.setmaxTotal(20);  //最大连接个数，如果不设置此参数，就会无限制地创建连接个数，造成不必要的负担。超过此连接个数则会等待其他连接个数使用完
    ds.setMinIdle(3);
  }
  
  public Connection getConnection(){
    try{
      return ds.getConnection();
    }catch(SQLException e){
      e.printStackTrace();
    }
    return null;
  }
  
  public static void close(ResultSet rs,Statement stmt,Connection con){
    closeResultSet(rs);
    closeStatement(stmt);
    closeConnection(con);
  }
  
  //该方法专用于后面的转账操作
  public static void closeExchange(Statement stmt1,Statement stmt2,Connection con){
    closeStatement(stmt1);
    closeStatement(stmt2);
    closeConnection(con);
  }

  private static void closeResultSet(ResultSet rs){
   	  try{
        if(rs!=null)rs.close();
      }catch(SQLException e){
        e.printStackTrace();
      }
  }
  private static void closeStatement(Statement stmt){
    	//注意：Statement是java.sql下的Statement
  	  try{
        if(stmt!=null)stmt.close();
      }catch(SQLException e){
        e.printStackTrace();
      }
  }
  private static void closeConnection(Connection con){
   	  try{
        if(con!=null)con.close();  //这里会把连接 归还给DPCP连接池 ，并不会真正地断开连接
      }catch(SQLException e){
        e.printStackTrace();
      }  	
  }
}
```



连接池第三方插件—C3P0

```java
public class C3P0DataSource{
  private static final String connectionURL = "jdbc:mysql://localhost:3306/web01?useUnicade=true&characterEncoding=UTF8&useSSL=false";
  private static final username = "root";
  private static final password = "root";
  
  private static ComboPoolDataSource ds;
  
  static{
    try{
      ds  = new ComboPoolDataSource();
      ds.setDriverClassName("com.mysql.jdbc.Driver");
      ds.setJdbcUrl(connectionURL);
      ds.setUsername(username);
   	  ds.setPassword(password);
      
      ds.setInitialPoolSize(5);
      ds.setMaxPoolSize(20);
     
    }catch(PropertyVetoException e){
      e.printStackTrace();
    }
  }
  
  public static Connection getConnection(){
    try{
      return ds.getConnection();
    }catch(SQLException e){
      e.printStackTrace();
    }
    return null;
  }
  
  public static void close(ResultSet rs,Statement stmt,Connection con){
    closeResultSet(rs);
    closeStatement(stmt);
    closeConnection(con);
  }
  
  //该方法专用于后面的转账操作
  public static void closeExchange(Statement stmt1,Statement stmt2,Connection con){
    closeStatement(stmt1);
    closeStatement(stmt2);
    closeConnection(con);
  }

  private static void closeResultSet(ResultSet rs){
   	  try{
        if(rs!=null)rs.close();
      }catch(SQLException e){
        e.printStackTrace();
      }
  }
  private static void closeStatement(Statement stmt){
    	//注意：Statement是java.sql下的Statement
  	  try{
        if(stmt!=null)stmt.close();
      }catch(SQLException e){
        e.printStackTrace();
      }
  }
  private static void closeConnection(Connection con){
   	  try{
        if(con!=null)con.close();  //这里会把连接 归还给C3P0连接池 ，并不会真正地断开连接
      }catch(SQLException e){
        e.printStackTrace();
      }  	
  }  
}
```

