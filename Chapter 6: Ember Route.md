Ember Route
====================

我们将会涉及的第一个对象是Ember路由对象,它和Router是不同的.**Router**用来创建命名的url路由,路由(**Route**)对象是Ember对象中的一个特殊类型,当你访问url路由时,它可以帮助你设置和管理会发生什么.

假设我们要显示一个用户列表,首先我们要在router中添加一行:

    App.Router.map(function() {
      this.resource('users');
    })
    
现在当你访问`/users`时,Ember会去寻找一个`UsersRoute`对象,它可能是这样:

    App.UsersRoute = Ember.Route.extend({
    
      model: function() { this.store.find('user') }
    
    })
    
`model`是一个函数hook,当进入一个路由时它会被调用,model函数的结果可以被其他对象访问.

`store`是Ember的一个数据结构,你通过它处理记录的持久化.`find`用来抓取你传给它的类型的记录,他返回一个promise,当你调用它的`then`方法时,会返回一个`DS.RecordArray`对象,`DS.RecordArray`本质上是一个对象数组.

**Route Hooks**

Route对象的真正意义是使用hooks来准备数据,并且设置controller需要执行的行为.当你进入一个路由时,`model`仅仅是一系列被调用的hooks中的一个.这里有一些你需要知道的,以他们调用的顺序排列.

    App.UsersRoute = Ember.Route.extend({
    
      beforeModel: function(transition) { },
    
      model: function(params, transition) { },
    
      afterModel: function(model, transition) { },
    
      activate: function() { },
    
      setupController: function(controller, model) {
        controller.set('model', model)
        // or this._super(arguments...)
      },
    
      deactivate: function() { }
    
    })
    
`beforeModel`在`model`被调用之前调用.

`model`我们跳过.

`afterModel`在`model`完成后调用(也就是说,从服务器拉下来或者从store中取出数据).

`activate`在所有的model hooks完成后调用,意味着现在路由被激活了.

`setupController`是你会做一些controller设置的地方.你可以访问controller,它自身就是一个参数.注意,如果你实现了`setupController`, 你需要将controller的`model`属性设置为`model`参数,因为这个hook覆盖了父类的实现.如果你没有这样做,那么controller将不会有model属性,你也可以调用@_super(arguments...)完成相同的事情.

`deactive`当你退出路由时调用,如果你仅仅是改变model,但仍然停留在相同的路由,它是不会被调用的.例如,如果你从`/users/1`跳转到`/users/2`,`deactive`不会被调用.

**跳转对象**

`transition`参数被传入路由的model hooks,可以用来做中止跳转.例如,假设我们有一个HouseRoute,但是你不喜欢红色的房子:

    App.HouseRoute = Ember.Route.extend({
    
      afterModel: function(model, transition) {
        if (model.get('color') == 'red') {  transition.abort() }
      }
    
    })
    
这里我们使用`afterModel`,是应为此时model已经取得了,我可以访问它的color属性.如果你中止,Ember会返回到你之前的路由.这对于错误检查,确认对话框和根据状态锁定应用程序的某些部分提供了便利.

**抓取其他对象**

路由是一个让你可以遍历你的应用的地方,通常你需要提供它需要的controller的信息.

`this.modelFor('routeName')`会返回路由的model.

`this.controllerFor('controllerName')`会取得controller对象,有时你可能想要获得controller的model,这时你可以调用`this.controllerFor('controllerName').get('model')`.

路由对象是你的朋友,当你开始尝试在你的应用中做一些好玩的事情的时候,你会发现所有的这些hooks非常的方便.




 



