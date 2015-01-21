数据和安全
------------------------

Meteor使得编写分布式客户端代码变得像访问本地数据库一样简单。简洁且安全，不用再部署单独的RPC（远程过程调用）终端，不用再手动配置客户端数据缓存来避免访问缓慢，不用再担心当数据发生改变时要小心地验证每个客户端的非法信息。

在Meteor中，客户端和服务器端共享同样的数据库API。同样的程序代码——如校验和计算部分——大多可以同时在客户端和服务器端运行。但是，运行在服务器端的代码可以直接访问数据库，运行在客户端的代码却不能。这里的差别是Meteor数据安全模型的基础。

>在默认情况下，一个新的Meteor应用包含了`autopublish`和`insecure`这两个包，都是用来模拟使用效果让每个客户端都拥有完全读/写数据库的权限。这两个包都是很有用的原型调试工具，但是并不适用于产品级的应用。当你准备发布产品级应用时，只需要`remove`掉这两个包就行了。

每个Meteor客户端都包含一个内存中的数据库缓存。为了管理每个客户端的缓存，服务器端会发布包含JSON文件的数据集，客户端直接`subscribes`这些数据集。当数据集中的文件发生变化时，服务器端会向每个客户端发送更改数据库缓存信息。

大多数的Meteor应用都采用MongoDB作为数据库，因为目前MongoDB在Meteor中获得最好的支持，对于其他数据库的支持都会在将来陆续发布。`Mongo.Collection`用来声明并处理Mongo集合，通过Meteor的客户端Mongo模拟器——minimongo，我们可以同时在客户端和服务器端调用`Mongo.Collection`。

```   javascript
// declare collections
// this code should be included in both the client and the server
Rooms = new Mongo.Collection("rooms");
Messages = new Mongo.Collection("messages");
Parties = new Mongo.Collection("parties");

// server: populate collections with some initial documents
Rooms.insert({name: "Conference Room A"});
var myRooms = Rooms.find({}).fetch();
Messages.insert({text: "Hello world", room: myRooms[0]._id});
Parties.insert({name: "Super Bowl Party"});
```

`Publish`函数可以发布不同的数据集给每个客户端。在之前的实例中，一个已登陆的用户只能看到公共的，或者自己的，或者被邀请参与的`Party`文件。

一旦客户端获取到数据集，则将缓存作为本地数据库，极大地简化了客户端代码。任何读操作都不会消耗时间访问服务器，并且读取内容也限制在本地缓存的内容：在客户端对集合中的文件进行查询都只会返回服务器发布给该客户端的内容。

``` javascript
// client: start a parties subscription
Meteor.subscribe("parties");

// client: return array of Parties this client can read
return Parties.find().fetch(); // synchronous!
```

