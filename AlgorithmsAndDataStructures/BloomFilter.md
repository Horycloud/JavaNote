## BF简介

[JavaGuide布隆过滤器](https://snailclimb.gitee.io/javaguide/#/docs/dataStructures-algorithms/data-structure/bloom-filter)

 [避免缓存穿透的利器之BloomFilter](https://juejin.im/post/5db69365518825645656c0de)

`布隆过滤器（Bloom Filter）`是一个叫做 Bloom 的老哥于1970年提出的。我们可以把它看作由`二进制向量`（或者说位数组）和一系列`随机映射函数`（哈希函数）两部分组成的数据结构。以下简称 `BF`

相比于我们平时常用的 List、Map 、Set 等数据结构，它占用空间更少并且效率更高，但是缺点是其返回的结果是概率性的，而不是非常准确的。理论情况下添加到集合中的元素越多，误报的可能性就越大。并且，存放在布隆过滤器中的数据不容易被删除。

`位数组`中的每个元素都只占用 1 bit ，并且每个元素只能是 0 或者 1。这样申请一个容纳 100w 元素的位数组只占用 1000000Bit / 8 = 125000 Byte = 125000/1024 kb ≈ 122kb 的空间。

**总结**：一个名叫 Bloom 的人提出的`一种用来检索元素是否在给定大集合中的数据结构`，这种数据结构是高效且性能很好的，但缺点是具有一定的错误识别率和删除难度。并且，理论情况下，添加到集合中的元素越多，误报的可能性就越大。

## BF 原理介绍

**当一个元素加入布隆过滤器中的时候，会进行如下操作：**

1. 使用布隆过滤器中的哈希函数对元素值进行计算，得到哈希值（有几个哈希函数得到几个哈希值）；
2. 根据得到的哈希值，在位数组中把对应下标的值置为 1。

<img src="./image/BF原理.png" style="zoom: 33%;" />

如上图所示，`字符串`经过三个不同的hash函数计算后，得出的值分别为：`0、2、5`，故将索引为：`0、2、5`的值置为 1。

**当我们需要判断一个元素是否存在于布隆过滤器的时候，会进行如下操作：**

1. 对给定元素再次进行相同的哈希计算；
2. 得到值之后判断位数组中的每个元素是否都为 1，如果值都为 1，那么说明这个值在布隆过滤器中，如果存在一个值不为 1，说明该元素不在布隆过滤器中。

再次对字符串进行hash计算，判断`0、2、5`的位置是否都为1。

不同的字符串可能哈希出来的位置相同，这种情况我们可以适当增加位数组大小或者调整我们的哈希函数。

综上，我们可以得出：`布隆过滤器说某个元素存在，小概率会误判。布隆过滤器说某个元素不在，那么这个元素一定不在`。

## BF 使用场景

**判断给定数据是否存在**

+ 比如判断一个数字是否在包含大量数字的数字集中（数字集很大，5亿以上！）
+  `防止缓存穿透`（判断请求的数据是否有效避免直接绕过缓存请求数据库）等等
+ 邮箱的`垃圾邮件过滤`、`黑名单`功能等等

**去重**：比如爬给定网址的时候对已经爬取过的 URL 去重。

## Java手动实现BF

如果想要手动实现一个布隆过滤器，需要：

+ 一个适当大小的位数组保存数据
+ 几个不同的哈希函数
+ 添加元素到位数组（布隆过滤器）的方法实现
+ 判断给定元素是否存在于位数组（布隆过滤器）的方法实现

**下面给出一个实现的代码（对于所有类型对象皆适用）**：

```java
import java.util.BitSet;

public class MyBloomFilter {

    /**
     * 位数组的大小
     */
    private static final int DEFAULT_SIZE = 2 << 24;
  
    /**
     * 通过这个数组可以创建 6 种不同的哈希函数
     */
    private static final int[] SEEDS = new int[]{3, 13, 46, 71, 91, 134};

    /**
     * 位数组。数组中的元素只能是 0 或 1
     */
    private BitSet bits = new BitSet(DEFAULT_SIZE);

    /**
     * 存放包含 hash 函数类的数组
     */
    private SimpleHash[] func = new SimpleHash[SEEDS.length];

    /**
     * 初始化多个包含 hash 函数的类的数组，每个类中的 hash 函数都不一样
     */
    public MyBloomFilter() {
        // 初始化多个不同的 Hash 函数
        for (int i = 0; i < SEEDS.length; i++) {
            func[i] = new SimpleHash(DEFAULT_SIZE, SEEDS[i]);
        }
    }

    /**
     * 添加元素到位数组
     */
    public void add(Object value) {
        for (SimpleHash f : func) {
            bits.set(f.hash(value), true);
        }
    }

    /**
     * 判断指定元素是否存在于位数组
     */
    public boolean contains(Object value) {
        boolean ret = true;
        for (SimpleHash f : func) {
            ret = ret && bits.get(f.hash(value));
        }
        return ret;
    }

    /**
     * 静态内部类。用于 hash 操作！
     */
    public static class SimpleHash {

        private int cap;
        private int seed;

        public SimpleHash(int cap, int seed) {
            this.cap = cap;
            this.seed = seed;
        }

        /**
         * 计算 hash 值
         */
        public int hash(Object value) {
            int h;
            return (value == null) ? 0 : Math.abs(seed * (cap - 1) & ((h = value.hashCode()) ^ (h >>> 16)));
        }

    }
}
```

**测试**：

```java
String value1 = "https://javaguide.cn/";
String value2 = "https://github.com/Snailclimb";
Integer value3 = 13423;
Integer value4 = 22131;

MyBloomFilter filter = new MyBloomFilter();

System.out.println(filter.contains(value1));
System.out.println(filter.contains(value2));
System.out.println(filter.contains(value3));
System.out.println(filter.contains(value4));

filter.add(value1);
filter.add(value2);
filter.add(value3);
filter.add(value4);

System.out.println(filter.contains(value1));
System.out.println(filter.contains(value2));
System.out.println(filter.contains(value3));
System.out.println(filter.contains(value4));
```

**Output**：

```java
false
false
false
false
true
true
true
true
```



## Guava 自带 BF

自己实现的目的主要是为了让自己搞懂布隆过滤器的原理，Guava 中布隆过滤器的实现算是比较权威的，所以实际项目中我们不需要手动实现一个布隆过滤器。

首先我们需要在项目中引入 Guava 的依赖：

```xml
<dependency>
    <groupId>com.google.guava</groupId>
    <artifactId>guava</artifactId>
    <version>28.0-jre</version>
</dependency>
```

实际使用如下：

我们创建了一个最多存放1500个整数的布隆过滤器，并且我们可以容忍误判的概率为百分之（0.01）

```java
// 创建布隆过滤器对象
BloomFilter<Integer> filter = BloomFilter.create(Funnels.integerFunnel(),1500,0.01);
// 判断指定元素是否存在
System.out.println(filter.mightContain(1));
System.out.println(filter.mightContain(2));
// 将元素添加进布隆过滤器
filter.put(1);
filter.put(2);
System.out.println(filter.mightContain(1));
System.out.println(filter.mightContain(2));
```

在我们的示例中，当`mightContain（）` 方法返回`true`时，我们可以 `99％` 确定该元素在过滤器中，当过滤器返回`false`时，我们可以 `100％` 确定该元素不存在于过滤器中。

Guava 提供的布隆过滤器的实现还是很不错的（想要详细了解的可以看一下它的源码实现），但是它有一个重大的缺陷就是`只能单机使用`（另外，容量扩展也不容易），而现在互联网一般都是分布式的场景。为了解决这个问题，我们就需要用到 Redis 中的布隆过滤器了。

## Redis 中的 BF

> 介绍

Redis v4.0 之后有了 `Module`（模块/插件） 功能，Redis Modules 让 Redis 可以使用外部模块扩展其功能 。布隆过滤器就是其中的 Module。详情可以查看 Redis 官方对 Redis Modules 的介绍 ：https://redis.io/modules

另外，官网推荐了一个 RedisBloom 作为 Redis 布隆过滤器的 Module，地址：https://github.com/RedisBloom/RedisBloom

其他还有：

- redis-lua-scaling-bloom-filter （lua 脚本实现）：https://github.com/erikdubbelboer/redis-lua-scaling-bloom-filter
- pyreBloom（Python中的快速Redis布隆过滤器） ：https://github.com/seomoz/pyreBloom
- ......

RedisBloom 提供了多种语言的客户端支持，包括：Python、Java、JavaScript 和 PHP。

> 使用 Docker 安装

如果我们需要体验 Redis 中的布隆过滤器非常简单，通过 Docker 就可以了！我们直接在 Google 搜索`docker redis bloomfilter` 然后排除广告的第一条搜素结果就找到了我们想要的答案（这是我平常解决问题的一种方式，分享一下），具体地址：https://hub.docker.com/r/redislabs/rebloom/ （介绍的很详细 ）。

**具体操作如下**：

```
➜  ~ docker run -p 6379:6379 --name redis-redisbloom redislabs/rebloom:latest
➜  ~ docker exec -it redis-redisbloom bash
root@21396d02c252:/data# redis-cli
127.0.0.1:6379> 

```

> 常用命令

**注意**：`key`：布隆过滤器的名称，`item`：添加的元素。

1. **`BF.ADD `**：将元素添加到布隆过滤器中，如果该过滤器尚不存在，则创建该过滤器。格式：`BF.ADD {key} {item}`。
2. **`BF.MADD `** : 将一个或多个元素添加到 “布隆过滤器” 中，并创建一个尚不存在的过滤器。该命令的操作方式与`BF.ADD`相同，只不过它允许多个输入并返回多个值。格式：`BF.MADD {key} {item} [item ...]` 。
3. **`BF.EXISTS` ** : 确定元素是否在布隆过滤器中存在。格式：`BF.EXISTS {key} {item}`。
4. **`BF.MEXISTS`** ： 确定一个或者多个元素是否在布隆过滤器中存在格式：`BF.MEXISTS {key} {item} [item ...]`。

另外，`BF.RESERVE` 命令需要单独介绍一下：

这个命令的格式如下：

`BF.RESERVE {key} {error_rate} {capacity} [EXPANSION expansion] `

**下面简单介绍一下每个参数的具体含义**：

1. `key`：布隆过滤器的名称
2. `error_rate`：误报的期望概率。这应该是介于0到1之间的十进制值。例如：对于期望的误报率 0.1％（1000中为1），error_rate应该设置为 0.001。该数字越接近零，则每个项目的内存消耗越大，并且每个操作的CPU使用率越高。
3. `capacity`：过滤器的容量。当实际存储的元素个数超过这个值之后，性能将开始下降。实际的降级将取决于超出限制的程度。随着过滤器元素数量呈指数增长，性能将线性下降，大概满足下面的关系：

<img src="./image/performance.png" style="zoom:45%;" />

**可选参数**：

- `expansion`：如果创建了一个新的子过滤器，则其大小将是当前过滤器的大小乘以`expansion`。默认扩展值为 2。这意味着每个后续子过滤器将是前一个子过滤器的两倍。

> 实际使用

```
127.0.0.1:6379> BF.ADD myFilter java
(integer) 1
127.0.0.1:6379> BF.ADD myFilter javaguide
(integer) 1
127.0.0.1:6379> BF.EXISTS myFilter java
(integer) 1
127.0.0.1:6379> BF.EXISTS myFilter javaguide
(integer) 1
127.0.0.1:6379> BF.EXISTS myFilter github
(integer) 0
```



















