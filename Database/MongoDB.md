**问题：MongoDB底层原理或者数据结构是什么，事务处理、插入和MySQL有什么区别？为什么会慢？**

# MongoDB简介

[MongoDB菜鸟教程](https://www.runoob.com/mongodb/mongodb-tutorial.html)

关系型数据库遵循ACID规则

+ 事务在英文中是transaction，和现实世界中的交易很类似

**分布式系统**

+ 分布式系统（distributed system）由多台计算机和通信的软件组件通过计算机网络连接（本地网络或广域网）组成
+ 分布式系统是建立在网络之上的软件系统
+ 网络和分布式系统之间的区别更多的在于高层软件（特别是操作系统），而不是硬件

分布式计算的优点：

+ 可靠性（容错）、可扩展性、资源共享、灵活性、更快的速度、开放系统、更高的性能

分布式计算的缺点：

+ 故障排除和诊断、更少的软件支持、网络基础设置的问题（传输问题、高负载、信息丢失）、安全性

**NoSQL**

+ NoSQL指的是非关系型的数据库。NoSQL即 Not Only SQL的缩写，是对不同于传统的关系型数据库的数据库管理系统的统称。

**CAP定理**

在计算机科学中， CAP定理（CAP theorem）， 又被称作布鲁尔定理（Brewer's theorem），它指出对于一个分布式计算系统来说，不可能同时满足以下三点:

+ 一致性(Consistency)：所有节点在同一时间具有相同的数据
+ 可用性(Availability)：保证每个请求不管成功或者失败都有响应

- 分隔容忍(Partition tolerance)：系统中任意信息的丢失或失败不会影响系统的继续运作

CAP理论的核心是：一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求，最多只能同时较好的满足两个。

因此，根据 CAP 原理将 NoSQL 数据库分成了满足 CA 原则、满足 CP 原则和满足 AP 原则三 大类。

其中满足CP的有MongoDB、HBase、Redis



**什么是MongoDB?**

[原文1](https://www.jianshu.com/p/05111cffe807) 文中讲解了什么是MongoDB，以及为什么要使用它？

[原文2](https://zhuanlan.zhihu.com/p/34248254)

MongoDB 是由C++语言编写的，是一个基于分布式文件存储的开源数据库系统。在高负载的情况下，添加更多的节点，可以保证服务器性能。

MongoDB 旨在为WEB应用提供可扩展的高性能数据存储解决方案。

MongoDB 将数据存储为一个文档，数据结构由键值(key=>value)对组成。MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组。

```MongoDB
{
name:"sue",
age:26,
groups:["news","sports"]
}
```

这里的 key=>value 叫做 filed:value，field对应SQL中的字段含义

 比如在MySQL中的表结构是这样的：

|  id  |   user_name   |      email      | age  |    city     |
| :--: | :-----------: | :-------------: | :--: | :---------: |
|  1   |  Mark Hands   |  mark@abc.com   |  25  | Los Angeles |
|  2   | Richard Peter | richard@abc.com |  31  |   Dallas    |

在MongoDB中是这样的：

```MongoDB
{
    id:1,
    user_name:Mark Hands,
    email:mark@abc.com,
    age:25,
    city:Los Angeles
}
{
    id:2,
    user_name:Richard Peter,
    email:richard@abc.com,
    age:31,
    city:Los Dallas
}
```



# MongoDB底层原理

[原文](https://blog.csdn.net/qq_36697880/article/details/100974380)

MongoDB 的集群部署方案中有三类角色：**实际数据存储结点**、**配置文件存储结点**和**路由接入结点**。 

客户端直接与路由节点相连，从配置节点上查询数据，根据查询的结果到实际的存储节点上查询和存储数据。



MongoDB 和 MySQL的区别

> MongoDB的读/写和事务处理和MySQL有什么区别？为什么慢？

不管在读/写方面还是事务处理方面，MongoDB的速度都占据绝对优势

**为什么？**

+ 首先是内存映射机制，数据不是持久化到存储设备中的，而是暂时存储在内存中，这就提高了在IO上效率以及操作系统对存储介质之间的性能损耗。
+ 其次，NoSQL并不是不使用sql，只是不使用关系。没有关系的存在，就表示每个数据都好比是拥有一个单独的存储空间，然后一个聚集索引来指向。搜索性能一定会提高的。
+ 第三，语言。使用JavaScript语法进行操作更加高效、直接。

这些是MongoDB针对关系型数据库的效率要高的原因。

但是不能仅仅看重效率，这种数据库的设计带来的弊端也是有的。**例如数据关系的维护会带来很多冗余数据、客户端代码需要大量针对数据库进行的IO操作、数据挖掘难以实现等等。**

**MongoDB相比MySQL的一些缺点:**

+ 不支持事务操作
+ 占用空间过大
+ MongoDB没有如MySQL那样成熟的维护工具
+ 无法进行关联表查询，不适用于关系多的数据
+ 复杂聚合操作通过mapreduce创建，速度慢
+ 模式自由，自由灵活的文件存储格式带来的数据错误



