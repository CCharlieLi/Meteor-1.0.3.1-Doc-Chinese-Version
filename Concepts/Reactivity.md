响应
-------------------------

Meteor很注重[响应式编程](http://en.wikipedia.org/wiki/Reactive_programming)的概念，也就是说你只需要关心写代码，当代码中的数据发生变化时，运行结果会自动重新执行。

``` javascript
Tracker.autorun(function () {
  Meteor.subscribe("messages", Session.get("currentRoomId"));
});
```

这个例子（取自一个聊天室客户端）建立了一个根据session变量`currentRoomId`的数据获取方法。无论什么原因，一旦`Session.get("currentRoomId")`的值发生变化，这个函数会自动重新运行，获取新的数据来代替以前的数据。




