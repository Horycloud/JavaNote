## Maven

[Maven菜鸟教程](https://www.runoob.com/maven/maven-pom.html)

[Maven](https://www.cnblogs.com/whgk/p/7112560.html)

[Maven构建生命周期（lifecycle）](https://www.runoob.com/maven/maven-build-life-cycle.html)

执行任务或目标时，Maven 会在当前目录中查找 POM。它读取 POM，获取所需的配置信息，然后执行目标。

需要使用pom.xml来获取jar包，那么首先该项目就必须为maven项目，maven项目可以这样去想，就是在java项目和web项目的上面包裹了一层maven，本质上java项目还是java项目，web项目还是web项目，但是包裹了maven之后，就可以使用maven提供的一些功能了(通过pom.xml添加jar包)。

## modules

[maven 配置多模块项目 pom modules](https://blog.csdn.net/linhao19891124/article/details/73872303)

[关于项目结构和二方包](https://mp.weixin.qq.com/s/1S5ylGwSAdVTQmuaJ2tBQA)

用项目层次的划分替代包层次的划分有很多好处。

## 仓库的概念

**仓库分为**：`中央仓库、本地仓库、第三方仓库(私服)`

> 中央仓库

这个公共仓库是由Maven自己维护，里面有大量的常用类库，并包含了世界上大部分流行的开源项目构件。目前是以java为主

> 本地仓库

从maven中央仓库下载到本地的 jar 包的默认存放在当前登录系统的用户目录下（如"xxxUser/.m2/repository"）

想要修改存储路径就要去路径 `"xxxUser/apache-maven-3.6.3/conf/settings.xml"`下设置自定义下载地址，如：

```xml
<localRepository>/Users/superfarr/mavenRepository</localRepository>
```

如果项目的依赖JAR本地有，则直接从本地获取，若本地没有，则从中央仓库下载到本地，然后从本地获取。

>第三方仓库（私服）

**第三方仓库**，又称为`内部中心仓库`，也称为`私服`。

私服：一般是由公司自己设立的，只为本公司内部共享使用。它既可以作为公司内部构件协作和存档，也可作为公用类库镜像缓存，减少在外部访问和下载的频率。（使用私服为了减少对中央仓库的访问）

私服可以使用的是局域网，中央仓库必须使用外网。也就是一般公司都会创建这种第三方仓库，保证项目开发时，项目所需用的jar都从该仓库中拿，每个人的版本就都一样。

连接私服，需要单独配置。如果没有配置私服，默认不使用。



## maven java项目结构

`projectName`

+ `pom.xml`                    核心配置，项目根下
+ `target  `                      输出目录
+ `src   ` 
	+ `main `     
		+ `java `              java源码目录 
		+ `resources`     java配置文件目录
	+ `test`
		+ `java`              测试源码目录
		+ `resources`     测试配置目录

其中有个`target`目录，是因为将该 java 项目进行了编译，`src/main/java`下的源代码就会编译成`.class`文件放入`target`目录中，target 就是输出目录。





##  Maven搜索本地JAR包

maven项目的pom.xml文件内的依赖，都是先从本地的maven仓库搜寻，如果找不到，再去寻址下载。这里讲一下，它是如何搜索本地仓库的。

默认是在您当前用户名下的`.m2`的`repository`文件夹下，依据pom.xml提供的信息进行搜寻。

例如：

```xml
<dependency>
    <groupId>com.sina</groupId>
    <artifactId>java-bloomfilter</artifactId>
    <version>1.0</version>
</dependency>
```

最终认定路径是：`"xxxUser/.m2/repository/com/sina/java-bloomfilter/1.0"`





## `<dependencyManagement>`













##  `<dependency>`属性讲解

`<dependency>`属性有9个，分别为

```xml
<dependency>
    <groupId></groupId>
    <artifactId></artifactId>
    <version></version>
  	<type></type>
  	<optional></optional>
  	<exclusions></exclusions>
    <scope></scope>
    <classifier></classifier>
    <systemPath></systemPath>
</dependency>
```

>`groupId、artifactId、version`

**`groupId、artifactId、version`**是依赖的基本坐标，缺一不可。

**`type`**：依赖的类型，比如是`jar`包还是`war`包等。默认为jar，表示依赖的jar包。

+ 注意：`<type>pom.lastUpdated</type>` 中的`lastUpdated`表示使用更新描述信息，占位符作用，通俗点讲，选择该类型，jar包不会被加载进来，而只是将该jar包的一些`描述信息`加载进来。

>`optional`

**`optional`**：标记依赖是否可选。默认值false

+ 比如struts2中内置了log4j这个记录日志的功能，就是将log4j内嵌到struts2的jar包中，而struts2有没有log4j这个东西都没关系，有它，提示的信息更多，没它，也能够运行，只是提示的信息就相对而言少一些。所以这个时候，就可以对它进行可选操作，想要它就要，不想要，就设置为false。

> `exclusions`

**`exclusions`**：排除传递依赖，解决jar冲突问题

+ `依赖传递`的意思就是，A项目 依赖 B项目，B项目 依赖 C项目，当使用A项目时，就会把B也给加载进来，这是传递依赖，依次类推，C也会因此被加载进来。
+ 依赖传递有利有弊，坏处就是jar包的冲突问题，比如，A 依赖 B(B的版本为1.0)，C 依赖 B(B的版本为2.0)，如果一个项目同时需要A和C，那么A、C都会传递依赖将B给加载进来，问题就在这里，两个B的版本不一样，将两个都加载进去就会引起冲突。
+ 这时候就需要使用`exclusions`这个属性配置了。
+ maven也会有一个机制避免两个都加载进去，`maven 默认将配置在前面的优先使用`，但是我们还是需要使用exclusions来配置更合理。这里使用 `spring bean` 和 `struts2 spring plugin` 来举例子说明这个问题并使用exclusions解决这个问题。（spring bean 和 struts2 spring plugin都需要依赖spring-core，但版本不一样）从本地仓库中找到这两个jar包 

`spring bean` 依赖的`spring-core`的版本为3.2.0

`struts2 spring plugin` 依赖的 `spring-core`的版本为3.0.5

如果我们的需求是，将`struts2 spring plugin`配置在前面，但是需要的是3.0.5版本，则需要为`struts2-spring-plugin` 排除依赖（不使用3.0.5依赖)

```xml
<!-- struts2-spring-plugin -->
<dependency>
    <groupId>org.apache.struts</groupId>
    <artifactId>struts2-spring-plugin</artifactId>
    <version>2.3.15.2</version>
    <exclusions>
        <exclusion>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>

<!-- spring-beans -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-beans</artifactId>
    <version>3.2.0.RELEASE</version>
</dependency>
```

>scope

**`scope`**：依赖范围，意思就是通过pom.xml加载进来的jar包，在什么范围内使用生效，范围包括`编译时，运行时，测试时`

|  依赖范围  | 对于`编译` | 对于`测试` | 对于`运行时` |            例子             |
| :--------: | :--------: | :--------: | :----------: | :-------------------------: |
| `compile`  |     Y      |     Y      |      Y       |         spring-core         |
|   `test`   |     -      |     Y      |      -       |            junit            |
| `provided` |     Y      |     Y      |      -       |         servlet-api         |
| `runtime`  |     -      |     Y      |      Y       |          JDBC 驱动          |
|  `system`  |     Y      |     Y      |      -       | 本地的，Maven仓库之外的类库 |

`compile`：默认值，如果选择此值，表示编译、测试和运行都使用当前jar

`test`：表示只在测试时当前jar生效，在别的范围内就不能使用该jar包。例如：junit 。此处不写也不报错，因为默认是compile，compile包扩了测试

`runtime`：表示测试和运行时使用当前jar，编译时不用该jar包。例如：JDBC 驱动。JDBC 驱动，在编译时(也就是我们写代码的时候都是采用接口编程，压根就没使用到JDBC驱动包内任何东西，只有在运行时才用的到，所以这个是典型的使用runtime这个值的例子)，此处不写也不报错，理由同上

`provided`：表示编译和测试时使用当前jar，运行时不在使用该jar了。例如：servlet-api、jsp-api等。



## 依赖调节原则

maven会按照两个原则来解决传递依赖时jar包冲突问题：

**第一原则：路径近者优先原则**

+ `A-->B-->C-->D-->X(1.6)`

+ `E-->D-->X(2.0)`

使用`X(2.0)`，因为其路径更近　

**第二原则：第一声明者优先原则。**就是如果路径相同，maven 默认配置在前面的优先使用

+ `A-->B --> X(1.6)`
+ `C-->D--> X(2.0)`

当路径相同，那么如果A在前面，C在后面，则使用`X(1.6)`

**注意**：maven会先根据第一原则进行选择，第一原则不成，则按第二原则处理。

## Maven配置文件settings.xml

当出现包处理不了，很可能是因为镜像的问题，因为本机上用的是阿里或华为的镜像，两者资源不同，有时候切换一下获取可以。

此文件可以在 `/Users/superfarr/apache-maven-3.6.3/conf/settings.xml `下找到，或者在IDEA的pom.xml鼠标右键点击`Maven->Open 'settings.xml'` 直接在项目中修改。

涉及到数据库驱动时，尽量使其与MySQL版本保持一致！

> **pom 引入自己的编译的 jar 包时出现找不到包的问题**

自己的项目打包的时候会默认将其放在对应的文件夹下，比如名为`mango-common`的项目打包后在本地的路径为：

+ `mavenRepository/com/zhr/mango-common/0.0.1-SNAPSHOT`

这时候在 pom 中引入依赖的时候应该如下

+ 注意：`0.0.1-SNAPSHOT`为版本号

```xml
<dependency>
    <groupId>com.zhr</groupId>
    <artifactId>mango-common</artifactId>
    <version>0.0.1-SNAPSHOT</version>
</dependency>
```

如果还是不行，就自己手动导吧：

`File-->Project Stracture-->Modules-->Dependencies--> + -->JARs or Directories--> superfarr/mavenRepository/com/zhr/mango-common`

导入mango-common整个文件夹即可！

如果还不行，试着将本地的包删除重新编译一下，然后重新在pom导入一下应该就可以了，我是属于这种情况。



## 关于Maven父子工程的理解

比如，构建一个Maven项目，删除`src`目录，以后学习就在这个项目里面建立Moudel，这个空的工程就是Maven主工程。

父工程中有：

```xml
<modules>
  	<module></module>
</modules>
```

子项目会有：

```xml
<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.0.4.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
</parent>
```

父项目中的`jar`包子项目可以直接使用。



## 可选依赖 & 依赖排除

可选依赖（Optional Dependencies）和依赖排除（Dependency Exclusions）

> **概述**

**Maven**中的依赖关系是有传递性的。例如：项目B依赖项目C（B —> C），如果有一个项目A依赖项目B（A —> B）的话，那么项目A也会依赖项目C（A —> C）。虽然，这种依赖的自动传递性给我们维护项目的必要依赖关系带来了极大地帮助，但当我们在某些情况下，需要在项目A中排除对项目C的依赖时，这时又该怎么做呢？

**Maven** 为我们提供了两种解决方案，分别是：

+ `可选依赖（Optional Dependencies）`
+ `依赖排除（Dependency Exclusions）`

> **哪些场景需要排除依赖**？

当项目A依赖项目B，但当项目A不是完全依赖项目B的时候，即项目A只用到了项目B的一部分功能，而正巧项目B该部分功能的实现，并不需要依赖于项目C，这个时候，项目A就应该排除对项目C的依赖，如下图：

```
     A
     |
  |-------|--------|
  |--B1---B---B2---|
  |-------|--------|
              |
              C
```

> **为什么要排除依赖**？

有的人可能有这样的疑问，为什么要排除对项目C的依赖呢？就算包含了对项目C的依赖，也不会出问题啊。事实上，表面上看确实不会出现问题。

但是，我们必须记住一点：当我们使用一个工程时，控制`实际需要`的依赖列表非常重要。而且，排除不必要的依赖还可以帮助我们，节约磁盘、内存等空间，避免许可协议问题以及类路径问题等。

我们在享受Maven依赖的自动传递性带给我们的便利的同时，要时刻注意引入冗余、不必要的依赖对我们项目产生的负面影响。

> **可选依赖**

仍然沿用上面的例子，当项目A依赖项目B的一部分时，排除对项目C的依赖。此时，就需要我们在项目B中将所有和C相关的依赖设置为可选依赖。只有这样，在项目A中声明项目B为直接依赖的时候，才不会将项目B的所有依赖自动引入。这时，项目A只需要额外声明自己真正需要依赖即可。

就可以在项目B的 *`pom.xml`* 文件中配置如下：

```xml
<project>
    ...
    <dependencies>
        <dependency>
            <groupId>com.hory.ProjectC</groupId>
            <artifactId>Project-C</artifactId>
            <version>1.0</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
      	...
    </dependencies>
</project>
```

在项目A中按照正常的方式引入对项目B的依赖即可，这个时候，项目A就不会依赖项目C了。如果项目A用到了项目B中涉及项目C的功能，则需要在项目A的 *`pom.xml`* 文件中额外配置对项目C的依赖。

> **依赖排除**

接上文例子，如果项目B没有把项目C设置为可选依赖（Optional Dependencies），而项目A又想在依赖项目B的同时不依赖项目C，应该如何操作呢？这个时候，我们使用依赖排除（Dependency Exclusions）就可以达到目的。此时，项目B按照正常的方式引入对项目C的依赖，项目A在引入对项目B的依赖时，配置如下：

```xml
<project>
    ...
    <dependencies>
        <dependency>
            <groupId>com.hory.ProjectB</groupId>
            <artifactId>Project-B</artifactId>
            <version>1.0</version>
            <scope>compile</scope>
            <exclusions>
                <exclusion> 
                    <groupId>com.hory.ProjectC</groupId>
                    <artifactId>Project-C</artifactId>
                </exclusion>
            </exclusions> 
        </dependency>
    </dependencies>
</project>
```

> **注意**

需要注意的是，依赖排除（Dependency Exclusions）是基于单个依赖的，并不是POM级别的。也就是说，虽然，项目A在引入对项目B的依赖时，排除了对项目C的依赖。但，并不影响项目A在其他地方引入对项目C的依赖。

例如，项目A又引入了对项目D的依赖，而项目D依赖项目C，这个时候，项目A就又产生了对项目C的依赖。

另外，在**Maven**引入的jar包发生冲突时，也往往需要借助依赖排除（Dependency Exclusions）来解决。





## 搭建私服





## 从私服中获取jar包





## 使用maven对父工程与子模块的拆分和聚合

















