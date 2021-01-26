[MySQL菜鸟教程](https://www.runoob.com/mysql/mysql-tutorial.html)

## 终端操作MySQL

`题目`： 常见的操作数据库的shell命令有哪些？

<font color=DarkOrchid>**安装路径**：</font>

`/usr/local/Cellar/mysql/8.0.19` 这个是通过brew安装的路径

`/usr/local/mysql/bin` 这个是直接安装的路径

<font color=DarkOrchid>**启动/关闭/重启（通过brew安装的）**：</font>

```sh
mysql.server start
mysql.server stop
mysql.server restart
```

<font color=DarkOrchid>**登录 MySQL**：</font>

```sh
# 这个是直接安装的路径
cd /usr/local/mysql/bin 
```

```sh
# 这个是通过brew安装的路径，本机是这个
cd /usr/local/Cellar/mysql/8.0.19/bin 
```

```sh
mysql -u root -p
```

`mysql -h 主机名 -u 用户名 -p`

- `-h` : 指定客户端所要登录的 MySQL 主机名，登录本机(localhost 或 127.0.0.1)的话该参数可以省略；
- `-u` : 登录的用户名；
- `-p` : 告诉服务器将会使用一个密码来登录，如果所要登录的用户名密码为空，可以忽略此选项。
	如果我们要登录本机的 MySQL 数据库，只需要输入以下命令即可：
- `mysql -u root -p`
	按回车确认，如果安装正确且 MySQL 正在运行，会得到以下响应：
- Enter password:
	若密码存在，输入密码登录，不存在则直接按回车登录。登录成功后你将会看到 Welcome to the MySQL monitor... 的提示语。
	然后命令提示符会一直以 mysq> 加一个闪烁的光标等待命令的输入，输入 exit 或 quit 退出登录。



<font color=DarkOrchid>**查看是否有 mysqld 进程**：</font>

```bash
ps -ef|grep mysqld
```



<font color=DarkOrchid>**杀死进程**：</font>

```bash
kill -9 进程号
```



<font color=DarkOrchid>**终端查看MySQL版本信息**：</font>

[原文](https://www.cnblogs.com/niuben/p/11065651.html)

```sh
cd /usr/local/mysql/bin
```

```sh
mysql -V
```

或者

```sh
mysql --version
```

如果 MySQL 未启动，你可以使用以下命令来启动：

```sh
cd /usr/bin
```

```sh
./mysqld_safe &
```

如果你想关闭目前运行的 MySQL 服务器，你可以执行以下命令：

```sh
cd /usr/bin
```

```sh
./mysqladmin -u root -p shutdown
```





**MySQL安装路径**

通过 brew 安装的路径(本机是这个)

```shell
/usr/local/Cellar/mysql/8.0.19
```

直接安装的路径

```shell
/usr/local/mysql/bin
```



**启动MySQL**

```shell
mysql.server start
```

```shell
mysql.server stop
mysql.server restart
```



**登录MySQL**

直接安装的路径

```shell
cd  /usr/local/mysql/bin
```

通过brew安装的路径

```shell
cd /usr/local/Cellar/mysql/8.0.19/bin
```



```shell
mysql -u root -p
```

密码：151869

```shell
mysql -h 主机名 -u 用户名 -p
```

+ -h : 指定客户端所要登录的 MySQL 主机名，登录本机(localhost 或 127.0.0.1)的话该参数可以省略
+ -u : 登录的用户名
+ -p : 告诉服务器将会使用一个密码来登录,，如果所要登录的用户名密码为空, 可以忽略此选项



**终端查看MySQL版本信息**

终端依次输入

```shell
cd  /usr/local/mysql/bin
mysql -V  或者  mysql --version
```





## 终端导入mysql脚本

比如导入一个`mysql`脚本文件`casaba.sql`，路径为`/Users/superfarr/Desktop/casaba.sql`

+ 如果脚本中有创建数据库的语句，我们就不需要再次创建，如果没有，我们需要自己创建

下面的流程为针对脚本中`没有`创建数据库语句的教程

终端输入：

```shell
mysql -u root -p
```

有密码的输入密码

进入MySQL命令行之后

```mysql
CREATE DATABASE casaba;
```

```mysql
USE casaba;
```

SOURCE 『将.sql文件直接拖拽至终端，自动补全其文件目录』

```mysql
SOURCE /Users/superfarr/Desktop/casaba.sql;
```



## 终端导出mysql脚本

假设需要导出数据库名为 blog_db 的 sql 脚本

cd 『打开要将.sql文件生成的文件位置』，比如放在桌面的目录 blog 内

```bash
cd /Users/xx/Desktop/blog
```

然后执行

```bash
mysqldump -u root -p blog_db>blog_db.sql
```

如果有密码，输入密码即可

## 查看当前使用的是哪个数据库

```mysql
select database(); 
```

## 创建表

**创建user及表结构id, username, password**

```mysql
creat  table user(id int primary key auto_increment, username varchar(16),  password varchar(16));
```



**查看表结构**

```mysql
desc  user;
```



**查看表内容**

```mysql
select * from user;
select * from user where username = 'xx' and password = 'xx' ;
select *  from 
```



## 插入

通常是 

```mysql
insert  into user (username,password) values ('zhr','151869' );
```

也可以 

```mysql
insert  into user values (null, 'zhr','151869' );
insert  into user values (null, 'sss','123' );
```



## 分页查询

limit [位置偏移量]行数

位置偏移量是从哪一行开始 (行数从0开始)，行数是指查询几行

如果要查询 第7页，每页8行

起始和末尾行数

0-7 第一页

8-15 第二页

16-23 第三页

...

起始行数为   (页数-1)* 行数

比如查询第9页的所有行：

```mysql
SELECT * FROM web01.user limit 64,8;
```



## 删除

delect  from user ;  此语句慎用，可直接删除表中的所有信息

通常使用where删除指定行

```mysql
delect  from user where id=3;
```

删除之后再添加的话，id是一直增长的，而不是从1开始





删除 tb_user 表中第 2 行到第 8 行的数据：

```mysql
delete from tb_user where id between 2 and 8;
```

或者

```mysql
DELETE FROM tb_user WHERE EXISTS (select * from (select user_id from tb_user limit 2,8) as a where a.user_id=tb_user.user_id);
```



## 修改

```mysql
update user set username = 'hr' where id = 5 ; 
```

不加后面 的 where的话，会 将 所有的username都改为'hr'



**修改表名**

```mysql
rename  table user to student;
```



## UNION

**UNION**

MySQL UNION 操作符用于连接两个以上的 SELECT 语句的结果组合到一个结果集合中。

多个 SELECT 语句会删除重复的数据。

两个表 「Websites」 和 「apps」都有字段country，想要查询两个表的所有城市：

```mysql
SELECT country FROM Websites
UNION
SELECT country FROM apps
ORDER BY country;
```

查询出的城市是**不重复**的...



**UNION ALL**

想要列举所有的country，包括重复的值，用 UNION ALL

```mysql
SELECT country FROM Websites
UNION ALL
SELECT country FROM apps
ORDER BY country;
```



**带有 WHERE 的 SQL UNION ALL**

使用 UNION ALL 从 "Websites" 和 "apps" 表中选取**所有的**中国(CN)的数据（也有重复的值）：

```mysql
SELECT country, Website_name FROM Websites WHERE country='CN'
UNION ALL
SELECT country, app_name FROM apps WHERE WHERE country='CN'
ORDER BY country;
```



## JOIN

> 关联表查询
>
> 从多个数据表中读取数据

JOIN按照功能大致分为以下三类：

+ **INNER JOIN（内连接,或等值连接）**：获取两个表中字段匹配关系的记录。
+ **LEFT JOIN（左连接）：**获取左表所有记录，即使右表没有对应匹配的记录。
+ **RIGHT JOIN（右连接）：** 与 LEFT JOIN 相反，用于获取右表所有记录，即使左表没有对应匹配的记录。

[菜鸟教程](https://www.runoob.com/mysql/mysql-join.html)



















