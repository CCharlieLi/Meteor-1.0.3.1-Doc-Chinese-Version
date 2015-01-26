实时HTML模板
-------------------

HTML模板功能是一个web应用框架的重要内容。有了Blaze——Meteor的实时页面更新技术，你可以做到“响应式”地渲染页面，也就是说，它会追踪页面数据的变化并实时更新页面。

Meteor使你可以简单地将自己喜欢的模板语言与Meteor实时页面更新技术结合起来。你只需要像开发其他项目一样写出模板来，Meteor会负责实时更新模板内容。

Meteor内置的模板语言是[Spacebars](https://github.com/meteor/meteor/blob/devel/packages/spacebars/README.md)，是基于[Handlebars](http://handlebarsjs.com/)开发，并保留了部分Handlebars的特征和语法，但是在编译时被定制用来生成响应式Meteor模板。

> 目前，Meteor内置的模板系统只有Spacebars，但是Meteor社区已经开发出了很多支持其他模板语言的包，如[Jade](https://atmospherejs.com/mquandalle/jade)。

为了定义模板，在项目目录中新建一个扩展名为`.html`的文件，在文件中添加一个`<template>`标签并且在标签中加入`name`属性，在标签内部加入模板内容。Meteor会重新编译模板，使其能在客户端进行渲染，并且能被全局的`Template`对象调用。

当应用被加载时，会自动渲染特殊模板`<body>`，这里使用的是`<body>`元素而不是`<template>`。要在一个模板中插入另一个模板需要使用`{{> inclusion}}`。

要把数据添加到模板中，最简单的方法就是在Javascript文件中定义helper函数。用`Template.templateName.helpers({ ... })`来定义helpers。结合起来就是：


``` html
<!-- in myapp.html -->
<body>
  <h1>Today's weather!</h1>
  {{> forecast}}
</body>

<template name="forecast">
  <div>It'll be {{prediction}} tonight</div>
</template>
```

``` javascript
// in client/myapp.js: reactive helper function
Template.forecast.helpers({
  prediction: function () {
    return Session.get("weather");
  }
});


// in the JavaScript console
> Session.set("weather", "cloudy");
> document.body.innerHTML
 => "<h1>Today's weather!</h1> <div>It'll be cloudy tonight</div>"

> Session.set("weather", "cool and dry");
> document.body.innerHTML
 => "<h1>Today's weather!</h1> <div>It'll be cool and dry tonight</div>"

```

如果要遍历数组或是数据库，使用`{{#each}}`：

``` html
<!-- in myapp.html -->
<template name="players">
  {{#each topScorers}}
    <div>{{name}}</div>
  {{/each}}
</template>
```

``` javascript
// in myapp.js
Template.players.helpers({
  topScorers: function () {
    return Users.find({score: {$gt: 100}}, {sort: {score: -1}});
  }
});
```

在这个实例中，数据来自对数据库的查询操作。当数据库的指针被传递给`{{#each}}`，有新查询的数据时，它会连接所有的机制来高效地添加和移动DOM节点。

Helpers可以接受参数，并且接收当前模板上下文数据在`this`中。注意有的模板块的helpers会改变当前上下文（比如`{{#each}}` 和` {{#with}}`）：

``` javascript
// in a JavaScript file
Template.players.helpers({
  leagueIs: function (league) {
    return this.league === league;
  }
});
```

``` html
<!-- in a HTML file -->
<template name="players">
  {{#each topScorers}}
    {{#if leagueIs "junior"}}
      <div>Junior: {{name}}</div>
    {{/if}}
    {{#if leagueIs "senior"}}
      <div>Senior: {{name}}</div>
    {{/if}}
  {{/each}}
</template>
```

Helpers同样也可以用来传递持久性数据。

``` javascript
// Works fine with {{#each sections}}
Template.report.helpers({
  sections: ["Situation", "Complication", "Resolution"]
});
```

最后一点，你也可以通过使用`events`函数为一个模板添加事件处理。被传递到`events`函数的对象都记录在`Event Maps`中，事件处理函数中的`this`参数则是触发该事件的元素的上下文。

``` html
<!-- myapp.html -->
<template name="scores">
  {{#each player}}
    {{> playerScore}}
  {{/each}}
</template>

<template name="playerScore">
  <div>{{name}}: {{score}}
    <span class="give-points">Give points</span>
  </div>
</template>
```

``` javascript
// myapp.js
Template.playerScore.events({
  'click .give-points': function () {
    Users.update(this._id, {$inc: {score: 2}});
  }
});
```

要了解更多关于Spacebars的内容，请阅读[the Spacebars README](https://github.com/meteor/meteor/blob/devel/packages/spacebars/README.md)。

