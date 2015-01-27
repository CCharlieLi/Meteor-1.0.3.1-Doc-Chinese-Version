使用包（packages）
------------------

到目前为止，我们所接触到的所有功能都部署在Meteor的标准包中，这都归功于Meteor不同寻常的强大的同构包和构建系统。同构的意思是，这些包既能运行在浏览器，也能运行在移动端和服务器端。在包中也可以包含一些插件，来扩展构建进程，例如`coffeescript`（[CoffeeScript](http://coffeescript.org/) 编译）或者`templating`（编译HTML模板）。

任何人都可以发布Meteor包，目前已经有成千上万个由社区编写的Meteor包发布了。要浏览这些包，最简单的方法是访问由Percolate工作室开发的[Atmosphere](https://atmospherejs.com/)。你也可以使用`meteor search`和`meteor show`命令来查看。

你可以使用`meteor add`来为应用添加包或者使用`meteor remove`来移除包。此外，`meteor list`命令可以查看目前应用中在使用的包，`meteor update`命令会更新所有包到最新的版本。

在初始状态下，所有的应用都包含`meteor-platform`包，它会自动安装组成Meteor核心框架的包。如果你想构建一个自定义核心栈的框架，只需要移除`meteor-platform`然后随意添加你想要的任何包。

Meteor使用的是一个单一加载的包管理系统，也就是说它只会加载每个包的一个版本。在添加或升级一个特定版本的包之前，Meteor会使用一种约束机制来检查这样做是否会导致其他包崩溃。默认情况下，Meteor会使用保守的选择机制，当添加传递性依赖项（其他包所依赖的包，而不是应用本身依赖的包）时，Meteor会试图选择更早的版本。

除了应用中包含的Meteor官方发布的包，`meteor list`和`meteor add`同时也会搜索应用根目录下的`packages`目录。你也可以使用`packages`目录把你的整个应用划分到一些包中，方便复用和测试将要发布的包。详细内容请看[编写包](Writingpackages.md)部分。
