Ember Controller
====================

Controllers用来处理和特定的UI相关的非持久逻辑,他们通常包含一个模型或者模型数组.你可以在controller中放置函数,属性以及observers,它们的功能很像正常的Ember对象,但是有一些例外.

首先我们来看一下三种不同类型的controller.

**三个Controller的故事**

Ember提供了三种不同类型的controller:`ObjectController`, `ArrayController`, 和`Controller`.当路由读取一个单独的model时,使用`ObjectController`.当路由读取一组model时,使用`ArrayController`.当路由没有获取任何model时,使用`Controller`.

**属性,Observers和函数**

当进入controller时没有需要调用的特定的hooks.如果你真的需要的话,可以使用`init`,但是你不应该这样做,controller所有的设置工作应该在route中完成,通常在route的`setupController` hook.

还剩下什么?属性,observers和函数,这些都是你的基本材料.

    // app/assets/javascripts/controllers/user.js
    App.UserController = ObjectController.extend({
    
      someFunction: function() { alert('so functional') },
    
      someProperty: function() {
        if (this.get('model.firstName') == "Gregory") {
          return "Hey Gregory"
        } else {
          return "Hey, you're not Gregory"
        }
      }.property('model.firstName'),
    
      someObserver: function() {
        alert("You changed your name? I don't really see you as a " + this.get('model.firstName'));
      }.observes('model.firstName')
    
    })

你可以把你喜欢的尽可能多的放在你的controller,任何controller的属性都可以被模板和视图使用.

**Mixins**

如果你发现一些重复的逻辑,你可以把他们提取到一个`Ember.Mixin`,你的controller像这样继承它:

    // app/assets/javascripts/mixins/excited.js
    App.Excited = Ember.Mixin.create({
      levelOfExcitement: "I'm so freaking excited right now!!!"
    })
    
    // app/assets/javascripts/controllers/user.js
    App.UserController = ObjectController.extend(App.Excited, {
      // your controller code
    })
    
现在`UserController`有一个`levelOfExcitement`属性,创建mixin使用`.create`而不是`.extend`.

**执行模板的Action**

controller的另一个主要用途是处理模板的action.例如,一个模板可能由一个提交按钮和一个删除链接.它们的action会像这样被处理:

    // app/assets/javascripts/controllers/user.js
    App.UserController = ObjectController.extend({
    
      actions: {
    
        deleteUser: function() { this.get('model').destroyRecord() },
    
        saveChanges: function() { this.get('model').save() }
    
        }
    })
    
在controller中所有action的处理程序都要放在`actions`对象中,这是一个保持你的代码结构的Ember的惯例.


