**Routing in Ember**
====================

在Ember中的一切都从route开始,如果你非常熟悉其他框架下的route机制,那么Ember的也不会给你带来太多麻烦.

注意:不要把下面的代码添加到你的应用中,这里和接下来几章的代码,仅仅是为了演示事情是如何工作的.

**位置API**

首先,我们来了解一下Ember中的route机制.当你改变route时,Ember默认使用浏览器的`hashchange`事件来确认,它自己实现了[HashLocation][1]对象来处理route事件.

使用HashLocation,Ember route将会出现在url中的`#`后面.例如,你的route可能看起来像:

    http://myemberapp.com/#/

    http://myemberapp.com/#/about

    http://myemberapp.com/#/users/1

    http://myemberapp.com/#/users/1/edit
    
你可能并不想要直接从根目录开始使用你的Ember应用,这种情况下,只需要告诉Ember哪一个应该是根目录:

    // app/assets/javascripts/router.js
    App.Router.reopen({
      rootURL: '/some/path/'
    })
    
现在你的users的index route是这样的:

    http://myemberapp.com/some/path/#/users
    
你可能觉的那些hash值看起来很丑,这里有一个方案可以处理,Ember也实现了一个`HistoryLocation`类,他会使用浏览器的history API来处理route.

这里是如果使用HistoryLocation来代替HashLocation:

    // app/assets/javascripts/router.js
    App.Router.reopen({
      location: 'history'
    })

Boom,就是这么简单.

不是所有的浏览器都实现了history API,幸好Ember使用AutoLocation来解决, 如果你的浏览器支持,他就会使用HistoryLocation,否则就使用HashLocation.

    // app/assets/javascripts/router.js
    App.Router.reopen({
      location: 'auto'
    })
    
HashLocation和HistoryLocation都实现了Ember的[Location API][2],如果你要使用除了hash或者history以外的机制,你可以开发你自己的Location类,它只需要实现正确的API就可以了.

**编写Routes**

一组CRUD routes看起来像这样:

    // app/assets/javascripts/router.js
    App.Router.map(function() {
      this.resource('users');
      this.route('user.new', { path: '/users/new' });
      this.resource('user', { path: '/users/:id' }, function () {
        this.route('edit');
      })
    })

这将生成下面的routes:

`/users` (index)

`/users/new` (new)

`/users/:id` (show)

`/users/:id/edit` (edit)

删除和创建可以通过自定义的行为处理,所以它们不需要routes.

这里使用了两个函数`resource`和`route`,它们有很大的不同.

你可以使用`resource`接受一个参数,以便获得一个特定的目录,也可以在`resource`下进行嵌套.

你使用`route`来指定一些新的UI,它们不需要特定的目录.`route`是一个死胡同,你不可以在他下面进行嵌套,它可以不带参数.

`.`是使用camel命名的一个简单替代,`user.new`和`userNew`是一样的,它们都会对应到一系列以UserNew开头的对象.

译者注:该部分内容描写过于简单,不太好理解,详细内容可以参考[定义路由][3].

**嵌套路由意味着嵌套UI**

在Ember中,任何路由的UI是默认可见的,以下面的路由为例:

    // app/assets/javascripts/router.js
    App.Router.map(function() {
      this.resource('posts', { path: '/posts' }, function() {
        this.route('new', { path: '/new' });
      })
    })
    
当你访问`http://localhost:3000/posts/new`时,你会看到`posts`和`posts/new`模板,`posts`模板里面需要一个`outlet`标记来指定`posts/new`显示在哪里.

这和服务器端开发有非常大的不同,在那里每个路由可以有完全不同的UI.如果你在地址栏看到一个Ember的路由,这意味着它是有效的并且可以看到他的UI.这是一个特色,他允许你在其他UI的基础上划分UI,这种模式对于前端来说是非常有意义的.

**查看你的路由**

我建议你在hello world应用中尝试一下router,它放在`app/assets/javascripts/router.js`.添加了路由后,你可以刷新页面并且查看Ember inspector,来看一下Ember已经生成了哪些路由.Ember inspector也会给你显示路由会查找哪些Route, Controller, View, 和Template.这对于确保你的对象名称一致非常有用.

**总结**

如果你想要学习更多的关于路由的知识,我推荐[Ember入门指南][4]关于路由的部分.在[定义路由][5]指南中有两个表格,详细说明了如何使用route和resource来创建理想的路由.

在下一章中我们会仔细查看,当你点击路由时,实际做了什么.


  [1]: http://emberjs.com/api/classes/Ember.HashLocation.html
  [2]: http://emberjs.com/api/classes/Ember.Location.html#toc_location-api
  [3]: http://www.emberjs.cn/guides/routing/defining-your-routes/
  [4]: http://emberjs.com/guides/routing/
  [5]: http://emberjs.com/guides/routing/defining-your-routes/#toc_resources
