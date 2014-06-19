Ember Object Flow
====================

(再一次,你不需要将下面的代码添加到你的应用,你可以仅仅拿它测试一下,然后把它删除.)

假设我们有一个路由叫做`about`.

    // app/assets/javascripts/router.js
    App.Router.map(function() {
      this.route('about');
    })
    
当你访问到它是实际会发生什么那？它会实例化一系列和路由的名字相同的对象...像Rails!

当一个路由激活了Ember向下的流程时,从Route到Controller,到View,到Template,此时我们的aboout路由会按照下面的顺序去寻找:`AboutRoute`, `AboutController`, `AboutView`, 和一个叫做`about.js.emblem`的模板 (或者`about.hbs`如果你使用andlebars).

当找到每个对象时,它会调用指定的函数,或者钩住这个对象.稍后我会讲解这些钩子,所以我们现在只在`init`里面放一个控制台日志,这样你就可以看到这些对象被实例化了.

当你访问到about路由时,通过`http://localhost:3000/#/about`,所有的控制台日志会按照这个顺序被调用.

    // app/assets/javascripts/routes/about.js
    App.AboutRoute = Ember.Route.extend({
      init: function() {
        this._super();
        console.log('route called');
      }
    })
    
    // app/assets/javascripts/controllers/about.js
    App.AboutController = Ember.Controller.extend({
      init: function() {
        this._super();
        console.log('controller called');
      }
    })
    
    
    // app/assets/javascripts/views/about.js
    App.AboutView = Ember.View.extend({
      init: function() {
        this._super();
        console.log('view called');
      }
    })
    
    // app/assets/javascripts/templates/about.js.emblem
    h1 Template rendered!
    
如果没有找到这些对象,Ember也不会抱怨,它会在内存中创建它们.所以如果你不需要在AboutController, AboutView, 或者AboutRoute中做什么,那么就不用创建它们.

这就是我说的Ember对象流程,当一个路由被激活时,它向下流动到它关联的对象.

现在你了解了Ember中的对象流程,我会马上对每一个做简单的介绍.
