# 路径的表达方式

/

```
path=“/Users/superfarr/IdeaProjects/IO_study01/src/coffeeBean.jpg”;
```

常量拼接

```
path=File.separator+,“Users”+File.separator+“superfarr”+File.separator+“IdeaProjects”+File.separator+“IO_study01”+File.separator+“src”+File.separator+“coffeeBean.jpg”;
```





# API

|                             API                              |                 说明                  |
| :----------------------------------------------------------: | :-----------------------------------: |
|                   pathSeparator separator                    |             路径\|分隔符              |
| File(String parent, String child) 、 File(File parent, String child) 、File(String name) | 构造器\没有盘符以user.dir作为相对目录 |
|    getName()、getPath() 、getAbsolutePath()、 getParent()    |            文件名、路径名             |
|               exist()、isFile()、isDirectory()               |               判断状态                |
|                           length()                           |               文件长度                |
|                   creatNewFile()、delete()                   |         创建新文件、删除文件          |

length为零的时候有两种情况：① 不存在；② 是个文件夹

**注意**：通过newFile()创建文件时，文件名不能随便起，比如：取名为con时,则会创建失败，因为，con、com等是操作系统的设备名（创建的时候需要跟操作系统打交道），故不能成功创建。

**创建文件夹**

|        API         |                  说明                  |
| :----------------: | :------------------------------------: |
| mkdir() 、mkdirs() | 创建目录，如果父目录链不存在则一同创建 |
|       list()       |                下级名称                |
|    listFiles()     |                下级File                |
|    listRoots()     |                 根路径                 |

mkdir()必须保证上一级目录存在，才能创建mkdirs()， 上一级目录不存在，会自动创建

list() 是获得下一级，不是子孙级（获得子孙级需要用递归）

用递归遍历文件夹（打印子孙级目录和文件的名称）

```java
import java.io.File;

public class DirDemo02 {
    private static long len = 0;

    //利用递归打印一个文件/文件夹的子孙级名称
    //File有三个状态：1。文件（不存在 或 为空）——不往下走，2。文件夹——往下走
    public static void printName(File file,int deep){
        for(int i=0;i<deep;i++){
            System.out.print("-");
        }
        System.out.println(file.getName());
        if(null == file || !file.exists()){
            return;
        }else if(file.isDirectory()){
            for(File j:file.listFiles()){
                printName(j,deep+1);
            }
        }
    }

    //获取文件夹的大小
    public static void countFile(File file){
        if(null != file & file.exists()) {   //存在且不为空
            if (file.isFile()) {
                len += file.length();
            } else if (file.isDirectory()) {
                for(File s:file.listFiles()){
                    countFile(s);
                }
            }
        }
    }

    public static void main(String[] args) {
        String fileAbsPath ="/Users/superfarr/IdeaProjects/IO_study01";
        File file = new File(fileAbsPath);   //传入路径，封装成文件对象file
        printName(file,0);

        System.out.println("+++++++++++++++++++++++++++++++++++++++++");

        countFile(file);
        System.out.println(len);
        }
}
```

**字符集**

+ Java字符使用16位的双字节存储
+ US-ASCII、ISO-8859-1、UTF-8、UTF-16BE、UTF-16LE、UTF-16

**编码和解码**

```java
String msg = “性命生命使命”; 
byte[] bytes = msg.getBytes(“字符集名称”); //编码，字符串->字节

String msg01 = new String(bytes); //解码，字节->字符串
String(byte[] bytes, int offset, int length, String charsetName);  //有不同的参数，这里填入的是byte[] bytes这个参数
```



[Java：IO流之节点流与处理流](https://blog.csdn.net/jingzi123456789/article/details/72123937)



**四个抽象类**

|    抽象类    |               说明               |                    常用方法                    |
| :----------: | :------------------------------: | :--------------------------------------------: |
| InputStream  | 字节输入流的父类，数据单位为字节 |            int read()、void close()            |
| OutputStream | 字节输出流的父类，数据单位为字节 |  void write(int)、void flush()、void close()   |
|    Reader    | 字符输入流的父类，数据单位为字符 |            int read()、void close()            |
|    Writer    | 字符输出流的父类，数据单位为字符 | void write(String)、void flush()、void close() |



**java虚拟机无权调用垃圾回收机制，只能通知操作系统去操作。**

+ close()：关闭此输入/出流并释放与此流相关联的任何系统资源；

+ flush() ：刷新此输入/出流并强制任何缓冲的输出字节被写出；



**FileInputStream & FileOutputStream**

+ 文件字节流

+ 通过字节的方式读取/写出文件，适合读取所有类型的文件（图像、时频等），全字符请考虑 FileReader/FileWriter



**FileReader/FileWriter**

文件字符流

字节流可以处理图片和纯文本，如果是纯文本，里面全是字符，不同的字符集，转成字节数组个数不同，为了避免底层字符集的差异，官方推荐采用字符流（FileReader & FileWriter）来处理纯文本。

之前是通过字节数组来实现文件的读写，现在是字符数组，即字符串。
+ byte[ ] 是字节数组
+ char[ ] 是字符数组



**ByteArrayInputStream & ByteArrayOutputStream**

字节数组流（不用关闭）

任何东西都可以转成字节数组

ByteArray    —读取—>   ByteArrayInputStream       —读取—>    程序

ByteArray    —写入—>   ByteArrayOutputStream    —写入—>    程序

与FileInputStream &amp; FileOutputStream不同的是：
+ 源头不是电脑硬件上的文件，而是远程服务器，它是字节数组，JVM可直接访问
+ 字节数组流不用关，但是如果为了保证风格统一，关了也没事
+ 字节数组不要太大，内存速度快但是量小，所以将字符串转为字节数组时，字节数组要小一点



**$P_{180}$IO_工具类**

+ 封装拷贝的过程
+ 封装释放资源



**$P_{180}$处理流**

之前的节点流是与文件和字节数组直接打交道，为了提升性能操作方便，需要对其进行装饰处理，这样的流称之为处理流。

处理流使用的是装饰器设计模式

装饰器设计模式有四个组成部分：视频中讲了一个例子，定义一个接口

```java
interface Say{
    say();
    getvoice();
    setvoice();
}
```

一个类

```java
class People implements Say{}
```

另一个类— 放大器

```java
class Amplifier implements Say(Person p){}
```

① 抽象组件：需要被装饰的抽象对象(接口或抽象父类)，比如上面例子中的接口Say

② 具体组件：需要被装饰的对象，比如上面的Person抽象

③ 装饰类：包含了对抽象组件的引用以及装饰者共有的方法，比如上面的Amplifier

④ 具体的装饰类：

```java
Drink coffee = new Coffee(); //被装饰的对象
Drink suger = new Suger(coffee);
Drink milk = new Milk(coffee);
```



# 为什么IO流必须关闭？

当我们new一个java流对象之后，不仅在计算机内存中创建了一个相应类的实例对象。而且，还占用了相应的系统资源，比如：文件句柄、端口、数据库连接等。

在内存中的实例对象，当没有引用指向的时候，java垃圾收集器会按照相应的策略自动回收，**但是却无法对系统资源进行释放**。所以，我们需要主动调用close()方法释放java流对象。在java7之后，可以使用try-with-resources语句来释放java流对象，从而避免了try-catch-finally语句的繁琐，尤其是在finally子句中，close()方法也会抛出异常。





# select / poll / epoll

见操作系统OS





