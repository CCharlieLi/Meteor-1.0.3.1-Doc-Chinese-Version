结构化你的应用
-----------------

一个Meteor应用包含了在浏览器和PhoneGap移动应用上运行的客户端JavaScript以及在Meteor服务器Node.js容器中 运行的服务器端JavaScript，并且Meteor支持HTML模板，CSS规则和静态资源。Meteor会自动打包和传递这些不同的组件，并且你也能灵活地选择如
何在你的目录中结构化这些组件。

###特定目录

在默认情况下，Meteor应用目录下的所有JavaScript文件都会被打包并发送到客户端和>服务器端。但是有些文件和子目录的命名会影响它们的加载顺序以及在哪里加载等等特>征。以下是Meteor中特殊的文件和子目录名称的列表：
- client

任何名称为client的目录都将不会在服务器端加载，类似于包含在if (Meteor.isClient) { ... }中的代码。在产品模式（production mode）下，所有在客户端加载的文件都>会自动链接并压缩。在开发模式（development mode）下，所有文件都将单独发送方便>调试。

和其他文件相比，Meteor应用中的HTML文件在服务器端将会有略微不同的处理方式。Meteor会扫描应用目录中所有HTML文件中的三个顶级标签：<\head>，<\body>和<\template>。head和body部分被分别连接成一个单一的head和body，然后再被发送到客户端进行页
面初始化加载。

- server

任何名称为server的目录都将不会在客户端加载，如同包含在if (Meteor.isServer) { ... }中的代码，除非客户端没有接收到这些文件。任何敏感的文件（如包含密码控制和
认证机制的代码）都应该放在server目录中。

Meteor会收集所有的JavaScript文件，并执行client，public和private子目录中的所有
文件，加载到Node.js服务对象中。在Meteor中，对于每一个请求，服务器端代码都会运
行在一个单独的线程中，而不是Node中的异步回调方式，是因为我们发现通过这种线性>执行模式能更好的适应典型的Meteor服务器端代码。

- public

所有在public目录中的文件如同在client目录中一样，当加载public目录中的资源时，`public/`不会被包含在URL中，URL会将资源看作存放在顶级目录下，如`public/bg.png`将加载为`<img src='/bg.png' />`。public目录是用来存放`favicon.ico`,`robots.txt`及其他类似资源文件的最佳地方。

- private

所有在private目录中的文件只能被服务器端代码访问，并且能通过`[Assets](http://docs.meteor.com/#/full/assets)`API加载。这里可以用来存放个人数据文件和其他不想被外界访问的文件。

- client/compatibility

这个目录用来存放依赖于顶级全局变量的兼容性JavaScript库。这个目录中的文件在执行时将不会被打包到新的作用域，并且将在客户端JavaScript文件执行前被执行。

- test

所有命名为test的目录其内容都将不会被加载。仅用于本地测试代码。

###特定目录之外的文件

所有在特定目录之外的JavaScript文件都会在客户端和服务器端加载，经常用来定义模式和其他的功能。Meteor提供了`Meteor.isClient`和`Meteor.isServer`这两个变量，这样就可以根据判断当前运行在客户端还是服务器端来控制代码行为。

在特定目录之外的CSS和HTML文件只会在客户端加载，服务器端代码将不能使用。

###文件结构实例

Meteor应用的文件结构相当灵活。以下是一个文件结构实例，运用到了刚才提到的特定目录。

```
lib/                      # common code like collections and utilities
lib/methods.js            # Meteor.methods definitions
lib/constants.js          # constants used in the rest of the code

client/compatibility      # legacy libraries that expect to be global
client/lib/               # code for the client to be loaded first
client/lib/helpers.js     # useful helpers for your client code
client/body.html          # content that goes in the <body> of your HTML
client/head.html          # content for <head> of your HTML: <meta> tags, etc
client/style.css          # some CSS code
client/<feature>.html     # HTML templates related to a certain feature
client/<feature>.js       # JavaScript code related to a certain feature

server/lib/permissions.js # sensitive permissions code used by your server
server/publications.js    # Meteor.publish definitions

public/favicon.ico        # app icon

settings.json             # configuration data to be passed to meteor --settings
mobile-config.js          # define icons and metadata for Android/iOS

```

在学习了实例应用之后你可以构建自己的目录结构。运行`meteor create --example todos`然后探索一下一个真实应用的目录结构。


###文件加载顺序

最好的情况是，编码时尽量避免文件加载顺序造成的影响。比如，通过使用`Meteor.startup`或者将与加载顺序有关的代码移到那些能明确控制内容加载顺序以及其相关模块的加载顺序的模块中。但是有时候应用中的加载顺序依赖是不可避免的。

什么时候不要用特定的文件名和目录名：
- 子目录中的文件在父目录文件之前加载，于是目录树较深的文件先加载，根目录的文件后加载。在同一个目录中，文件是按照字母顺序加载的。

以下是所有控制加载顺序的特定文件和目录名的列表：

- lib

根据如上描述进行加载排序之后，lib目录下的所有文件将被移到所有文件执行顺序的最前端，并保持这样的顺序。

- main.*

所有匹配`main.*`这个名称的文件将被移到所有文件执行顺序的最後端，并保持这样的顺序。

###组织你的项目

目前有三种基本方法来根据项目特点和组件结构来组织项目目录。打个比方，现在在我们的项目中有两个类型的对象：'apples'和'oranges'。

#### 方法1：根级文件夹

#### 方法2：'client/'和'server/'中的文件夹

#### 方法3：包
