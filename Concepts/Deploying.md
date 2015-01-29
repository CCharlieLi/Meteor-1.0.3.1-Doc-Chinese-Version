部署
---------------

Meteor也是一个功能全面的服务器，包含了部署应用程序所需的所有功能，而开发者只需要提供JavaScript，HTML和CSS文件。

### 在Meteor的设施上运行

部署应用程序最简单的方式就是使用`meteor deploy`。之所以提供这个功能是因为个人认为这是我们最想要的：只需要提出一个想法，在一个周末实现它，然后发布给全世界使用，整个过程毫无阻碍。

``` shell
$ meteor deploy myapp.meteor.com
```

现在你的应用程序可以通过myapp.meteor.com访问了。如果这是第一次部署到这个域名，Meteor会为你的应用创建一个新的空数据库。如果你想更新之前的部署，Meteor会保存现有的数据，只更新代码。

你也可以将应用程序部署到自己的域名。只需要为你的域名设置一个CNAME映射到origin.meteor.com，然后部署到这个域名。

``` shell
$ meteor deploy www.myapp.com
```

我们提供的这项功能是完全免费的。这个功能可以帮助你快速实现内部beta测试，实现原型等等。要了解更多关于部署的内容，请阅读meteor deploy。

### 在自己的设施上运行

你也可以在自己的机器上或者任何能够运行Node.js的服务器上运行自己的应用。首先运行命令：

``` shell
$ meteor build my_directory
```

这条命令会生成一个Node.js应用程序的tar包。要运行这个程序，需要提供Node.js 0.10和MongoDB服务环境。（当前版本的Meteor已经在Node 0.10.33上经过了测试）然后就可以通过调用node来运行应用程序，指定程序要监听的HTTP端口和MongoDB终端。

``` shell
$ cd my_directory
$ (cd programs/server && npm install)
$ PORT=3000 MONGO_URL=mongodb://localhost:27017/myapp node main.js
```

有些包可能需要设置一些其他的环境变量，例如`email`包需要`MAIL_URL`环境变量。


