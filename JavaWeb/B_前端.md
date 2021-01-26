

### 前端

#### HTML

用一句话来形容HTML、CSS、Js：**HTML是名词，CSS是形容词，JavaScript是动词**

锚点 

提交路径

```html
<form action="asjdb.html" method="get" name="registerform">  <!-- Jsp Servlet-->
<!-- method="get" 将网页内容放到URL里面，比较适合少量的，保密性不强的内容-->
<!-- method="post" 将内容封装到Http请求里面，比较适合大量数据，敏感数据比如密码-->
```

我们每次点击一个链接，但是我们只能看到这个链接，但是这个链接向服务器端发起请求的时候，它需要把整个请求封装一下发送给服务器端，然后服务器端再把数据返回给浏览器，浏览器再去解析这个响应。

后台跟后端的区别：

+ 前端指的是网页部分，后端指的是服务器端。
+ 前端既有前台又有后台。后台是给管理员使用的，前台是给用户使用的。





#### CSS

+ 外部样式表

+ 内部样式表

+ 内联样式表（即在html标签里添加的属性，优先级最高）

！import优先级最高，谁也不能改

`CSS框架`

+ BootStrap：[http://www.bootcss.com/](https://link.zhihu.com/?target=http%3A//www.bootcss.com/)
+ Materialize：[http://www.materializecss.cn/](https://link.zhihu.com/?target=http%3A//www.materializecss.cn/)





#### JavaScipt

JS 是直译型语言、动态类型、弱类型语言、基于原型的语言

作用：轮播、登录校验

声明一个对象（两种方法） ：

+ ```javascript
  var obj = new Object();
  var obj = {};
  ```

**JS 事件**

+ 事件：用户与文档、浏览器的交互行为。比如：点击按钮、鼠标移动

+ 事件相应方式：

  + ① 元素的事件属性 

    + ```html
      <button onclick = "js代码"></button> 
      ```

  + 动态的绑定事件

    + 找到要绑定事件的元素
    + 为找到的元素添加事件的响应方法
    + 当事件触发，浏览器就会自动调用响应函数

**js加载方式**（HTML嵌入 JS 代码的方式）：相当于CSS的**外部样式表、内部样式表、内联样式表**

+ ① 元素的事件属性上编写js代码（不推荐）

+ ② 写在script标签中(推荐)

  + head，需要写 window.onload = funnction(){ }
  + body，不用写 window.onload = funnction(){ }

+ ③ 通过外部文件引入

  + 通过引入的方式，不可以在当前引入的script标签中写代码，需要在另外创建的一个script标签中写

  + ```html
    <script type="text/javascript" src="js文件路径"></script>>
    <script type="text/javascript" src="js文件路径">
      //要在这里写代码
    </script>
    ```

**DOM**（Document Obiect Model）文档对象模型

**BOM** （Browser）浏览器对象模型

+ DOM树
+ html标签、html元素、结点Node
+ html
  + head
    + title
    + style
  + body
    + form -->table-->tr-->...-->text
+ DOM定义了表示和修改文档所需的对象、这些对象的行为和属性以及这些对象之间的关系
+ 可以把DOM认为是页面上数据和结构的一个树形表示
+ 当整个网页被加载的时候，浏览器会给网页上的每个结点创建一个与结点所对应的对象，如果我们想要操作某个结点，我只需要用 Js 处理该结点所对应的对象（进而操作HTML页面）
+ 利用DOM可以获取结点对象，然后对结点对象进行操作
  + 根据几种不同的获取结点的方式，可以实现对结点的集体操作

节点——构成HTNL文档最基本的单元

节点分为三类：

+ ```html
  <p id="pID">This is a paragraph</p>
  ```

+ `元素节点`：HTML 文档中 HTML 标签，比如：<p></p>

+ `属性节点`：元素的属性，比如：id="pID"

+ `文本节点`：HTML 标签中的文本内容，比如：This is a paragraph

节点的属性：

+ nodeName：节点名
+ nodeType：节点类型
+ nodeValue：节点值

​									nodeName     nodeType     nodeValue

`元素节点` 					    标签名				1		      	 null

`属性节点`				 	    属性名				2			     属性值

`文本节点`					     #text			  	3				文本内容



**DOM查询**

```html
<!DOCTYPE html>
<html lang="en">
<!--DOM查询-->
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
        window.onload = function(){
            //获取元素节点
            let pNode = document.getElementById("pID");
            alert("nodeName:"+pNode.nodeName); //P
            alert("nodeType:"+pNode.nodeType);  //1，所有节点的nodeType都会返回一个整数，其中元素节点返回1
            alert("nodeValue:"+pNode.nodeValue); //null
            //获取属性节点
            let attrNode = pNode.getAttributeNode("id");
            //获取文本节点
            let txtNode = pNode.firstChild;
            /*
            getElementByTagName 方法，返回当前节点的指定标签名子节点
            childNodes 属性，表示当前节点的所有子节点
            firstChild 属性，表示当前节点的第一个子节点
            lastChild 属性，表示当前节点的最后一个子节点
             */
        }
    </script>
</head>
<body>
		<p id="pID">This is a paragraph</p>
</body>
</html>
```



**DOM增删改**

```html
<!DOCTYPE html>
<html lang="en">
<!--DOM增删改-->
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
        window.onload = function(){
            //1. 修改
            let bjEle = document.getElementById("bj");
            bjEle.innerText = "南京";
            bjEle.id = "bj2";

            //2. 删除(不能自删)    父元素.removeChild(子元素);
            let bjParent = bjEle.parentElement;   //先获取父节点
            bjParent.removeChild(bjEle);

            //3. 替换，将东京节点替换为广州节点，    父元素.replace(新元素,旧元素)
            let djEle = document.getElementById("dj");  //先获取旧元素
            //创建新元素（分三步：1.创建子节点——>广州文本节点  2.创建父节点——>li节点 3.将子节点加入到父节点内部）
            let textEle  = document.createTextNode("广州");
            let gzEle  = document.createElement("li");
            gzEle.appendChild(textEle);
            let djParent = document.getElementById("city");
            djParent.replaceChild(gzEle,djEle);
        };
    </script>
</head>
<body>
    <ul id="city">
        <li id="bj">北京</li>
        <li id="sh">上海</li>
        <li id="dj">东京</li>
        <li id="se">首尔</li>
    </ul>

</body>
</html>
```



**button**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
        window.onload = function(){
            //1.找到元素
            let btnEle = document.getElementById("btn");
            //2.绑定单击事件
            btnEle.onclick = function(){
                //3.为单击事件幅值一个相应方法
                alert("Hello World");
            }
        }
    </script>
</head>
<body>
    <button id="btn">点击我</button>
</body>
</html>
```



`取消默认行为`

+ 默认行为：指超链接（跳转）、提交按钮（提交表单 ）点击以后会有 一个默认的动作
+ 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
        window.onload = function(){
            let btnEle = document.getElementById("btn");
            btnEle.onclick = function(){
                //取消默认行为
              	return false;
            }
        }
    </script>
</head>
<body>
    <button id="btn">点击我</button>
</body>
</html>
```





#### JQuery

+ JQuery 是一套跨浏览器的 JavaScript 库，简化 HTML 与 JavaScript 之间的操作
+ JQuery 好处：极大地简化了 JavaScript 开发人员遍历 HTML 文档、操作 DOM、处理事件、执行动画和开发Ajax，链式操作（比如：obj.xxx.xxx.xxx），隐式迭代（如果要给一堆元素绑定事件，JQuery可以在内部隐式遍历挨个绑定事件）

+ juery相比js优点：
  + jquery的onload加载事件速度更快，并且多个加载并行
  + jq绑定事件都是使用的事件函数，不需要加on
  + js的onload加载事件，后面的覆盖前面的
  + 在jQuery中，$( )是其运行环境；
  + jQuery的模块可以分为3部分：`入口模块`、`底层支持模块`和`功能模块`。

+ jQuery库包含以下功能：
  + HTML 元素选取、 HTML 元素操作、CSS 操作、HTML 事件函数、 JavaScript 特效和动画、 HTML DOM 遍历和修改、AJAX、Utilities

**核心函数$()的四种用法**

+ ① 传入参数为 [函数] 时，默认为文档加载完之后执行此方法。比如：$(function(){})，类似 window.onload

+ ② 传入参数为 [HTML字符串] 时，$("<div>html</div>")，根据字符串创建元素节点对象
  
  + 例如：$("<li>手机</li>").appendTo("#ulID");
  
+ ③ 传入参数为 [选择器字符串] 时，$("#id")，根据选择器查找出 元素节点对象

+ ④ 传入参数为 [DOM对象] 时，$(this)，将dom对象包装为jQuery对象返回

  + 

+ ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript" src="../js/jquery-3.5.1.js"></script>
      <script type="text/javascript">
          // $() 是jQuery的核心函数，跟jQuery() 是等价的
          // 为$()方法传入一个function(){}作为参数，就相当于window.onload
          $(function(){
              $("#btnID").click(function(){
                  alert("我被点击了");
              });
          });
      </script>
      <style type="text/css">
          p{
              display:block;
              width:200px;
              height:400px;
              borger:1px solid;
          }
      </style>
  </head>
  <body>
      <p id="p1">首行</p>
      <p>你好1</p>
      <p>你好2</p>
      <p>你好3</p>
      <ul id="ulID">
          <li>相机</li>
      </ul>
      <!--用法③，#是链接到本页面，锚点-->
      <a id="aID" href="#p1">跳转到首行</a>
      <button id="btnID">Hello</button>
  </body>
  </html>
  ```

  























