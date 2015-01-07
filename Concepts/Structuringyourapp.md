结构化你的应用
-----------------

一个Meteor应用包含了在浏览器和PhoneGap移动应用上运行的客户端JavaScript以及在Meteor服务器Node.js容器中 运行的服务器端JavaScript，并且Meteor支持HTML模板，CSS规则和静态资源。Meteor会自动打包和传递这些不同的组件，并且你也能灵活地选择如
何在你的目录中结构化这些组件。
###特殊的目录

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

