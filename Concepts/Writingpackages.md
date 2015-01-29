创建包
-------------

想要编写一个Meteor包很简单。首先初始化一个meteor包，执行`meteor create --package username：packagename`，这里的`username`是你的Meteor Developer用户名。这样就准备好了从头开始创建一个包，并且目录中已经预先生成了一个package.js控制文件和其他一些javascript脚本。默认情况下，Meteor会将包含package.js的目录名称作为包的名称。

Meteor确保所有的包和应用程序都是可重复创建的。也就是说，如果你在一个机器上创建了自己的包，然后将代码放入到代码库中，在其他任何地方你仍然可以使用这个代码来创建得到同样的包。在你的包目录中，你会发现一个自动生成的`.versions`文件，
