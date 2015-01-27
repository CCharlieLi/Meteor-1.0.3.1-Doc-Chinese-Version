命名空间
---------------------

Meteor的命名空间支持使得用Javascript编写大型应用程序变得更容易。应用中所使用的每个包都在相互隔离的命名空间中，也就是说一个包只能看到自己的全局变量，或者其他包提供的用于特殊用途的包。以下将展示命名空间是如何工作的：

当你声明了一个全局变量，你可以选择这个变量的使用范围在文件范围或包范围。

``` javascript
// File Scope. This variable will be visible only inside this
// one file. Other files in this app or package wont see it.
var alicePerson = {name: "alice"};

// Package Scope. This variable is visible to every file inside
// of this package or app. The difference is that 'var' is
// omitted.
bobPerson = {name: "bob"};
```

注意这里只是采用原生的Javascript语法来区分定义全局变量和局部变量。Meteor会扫描源代码中声明的全局变量，然后生成一个包装器使得所有的全局变量都不会超出命名空间的范围。

除了文件范围和包范围，还可以使用导出变量。导出变量是指当你需要在包范围以外使用包中的变量时，使其变得可用。比如，从`email`包中导出`Email`变量。如果你的应用使用了`email`包（并且必须使用`email`包！）那么就可以使用`Email`变量，并且可以调用`Email.send`。大部分的包只有一个导出变量，但是有些包可能有两个或三个导出变量（如一个包中有多个类同时在工作）。

只有当前直接使用的包的导出变量是可见的。如果你正在使用包A，而包A正在使用包B，那么你只能使用包A的导出变量。包B的导出变量不会“渗透”到当前的命名空间中，因为你正在使用包A。每个命名空间都保持干净整洁。每个应用或包都只能使用自己的全局变量和所需的由其他包提供的API。

当在调试应用程序时，浏览器的JavaScript命令行上下文和应用的命名空间是对应的。你只能使用当前应用的全局变量和所使用的包的导出变量，而不能使用这些包内部的变量，也不能使用传递性依赖项（不是由应用程序直接使用的包，而是其他包所依赖的包）的导出变量。

如果你想使用浏览器内置调试器查看一个包的内部，你有两个可选操作：

- 在包中的代码设置断点。当程序停在断点处时，当前命令行就已经在这个包的命名空间中。你就能查看这个包的包范围变量和导入变量，以及断点所在文件的文件范围变量。

- 假如在你的应用程序中包含了`foo`包，无论应用程序是否直接使用这个包，它的导出变量都能通过`Package.foo`来使用。例如，应用中安装了`email`这个包，你可以在其他命名空间直接使用`Package.email.Email.send`。

在声明一个函数时，要记住在JavaScript中`function x () {} `只是`var x = function x () {}`的简写形式。参照如下实例：

``` javascript
// This is the same as 'var x = function x () ...'. So x() is
// file-scope and can be called only from within this one file.
function x () { ... }

// No 'var', so x() is package-scope and can be called from
// any file inside this app or package.
x = function () { ... }
```

> 技术上来说，应用程序中的全局变量（相对于包中的全局变量）才是真正的全局变量。这些变量不能在私有范围的应用程序代码中被捕获，否则在浏览器的命令行调试器中将看不到这些变量。也就是说应用程序的全局变量实际上对于包是可见的。如果适当地注意编写包代码，这都不会造成什么问题（应用程序全局变量也应该注意避免使用包中声明的变量名称）。其实开发者不用担心这种巧合，以后Meteor将会对这些变量名称进行检查，并在重名的时候抛出异常。


