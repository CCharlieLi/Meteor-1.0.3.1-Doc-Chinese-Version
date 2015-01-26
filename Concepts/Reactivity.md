响应
-------------------------

Meteor很注重[响应式编程](http://en.wikipedia.org/wiki/Reactive_programming)的概念，也就是说你只需要关心写代码，当代码中的数据发生变化时，运行结果会自动重新执行。

``` javascript
Tracker.autorun(function () {
  Meteor.subscribe("messages", Session.get("currentRoomId"));
});
```

这个例子（取自一个聊天室客户端）建立了一个根据session变量`currentRoomId`的数据获取方法。无论什么原因，一旦`Session.get("currentRoomId")`的值发生变化，这个函数会自动重新运行，获取新的数据来代替以前的数据。

这种自动重新执行的功能是由`Session`和`Tracker.autorun`共同实现的。`Tracker.autorun`扮演了一个随机的”响应式计算“，这个函数包含的数据依赖项都会被”追踪“，并在必要的时候重新执行函数中的方法参数。另一方面，数据提供方Session会记下它们在哪里被调用，被请求了哪些数据，并准备在数据改变时发送一个无效的信号给”Tracker.autorun“。

这种简单的模式（响应式计算+响应式数据源）具有广泛的适用性。通过上面的方式，开发者不用再编写”取消订阅/再订阅“方法并确保它们在正确的时候被调用。总的来说，Meteor可以避免所有因逻辑错误阻塞程序的数据传递代码。

以下Meteor函数会以响应式计算的形式运行代码：

- Templates
- Tracker.autorun
- Blaze.render 和 Blaze.renderWithData

可以触发数据变化的响应式数据源包括：

- Session variables
- Database queries on Collections
- Meteor.status
- The ready() method on a subscription handle
- Meteor.user
- Meteor.userId
- Meteor.loggingIn

除此之外，以下函数会返回一个包含`stop`方法的对象，如果在“响应式计算”中调用了`stop`方法，那么当“响应式计算”返回或停止时，以下函数都会停止运行。

- Tracker.autorun(nested)
- Meteor.subscribe
- 光标的observe() 和 observeChanges() 事件







