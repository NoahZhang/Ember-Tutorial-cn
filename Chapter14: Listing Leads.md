Listing Leads
==========

Ember端我们要做的第一件事情是列出我们所有lead的清单.

**添加路由**

所有的事情都从路由开始,打开你的Ember Router并且创建一个leads资源,设为根目录:

    // app/assets/javascripts/router.js
    App.Router.map(function() {
      this.resource('leads', { path: '/' })
    })
    
**创建一个路由对象**

下面我们要抓取所有的lead记录,我们创建一个`LeadsRoute`:

    // app/assets/javascripts/routes/leads.js
    App.LeadsRoute = Ember.Route.extend({
      model: function() { return this.store.find('lead') }
    })
    
记住`model`是一个hook,当每次进入路由时都会被调用.model函数的结果可以用于controller, view, 和template.

可以确定这是可以正常工作的,访问你的跟路由并且在Ember Inspector中查看"Data"页签,你应该可以看到所有的leads.

**创建模板**

现在我们有需要展示的leads,我们创建一个模板:

    // app/assets/javascripts/templates/leads.js.emblem
    article#leads
      h1 Leads
      ul
        each lead in controller
          li= lead.firstName
      
现在打开页面,你应该可以看到左侧的leads清单.酷毙了,对吧!?

**显示全名**

我们创建一个属性,用来显示lead的全名,打开lead模型并且添加:

    // app/assets/javascripts/models/lead.js
    fullName: function() {
      return this.get('firstName') + ' ' + this.get('lastName')
    }.property('firstName', 'lastName')

然后修改模板:

    li= lead.fullName
    
**Leads排序**

我们的leads没有任何特定的顺序.这太乱了!我们按照姓名对它们排序.

你可以在控制器中指定`sortProperties`来排序模型数组,但是我们还没有控制器,我们创建一个:

    // app/assets/javascripts/controllers/leads.js
    App.LeadsController = Ember.ArrayController.extend({
      sortProperties: ['firstName', 'lastName']
    })
    
sortProperties接受一个字符串数组,这些字符串就是你想要按照最高优先级排序的属性.

注意我把这个控制器做为`ArrayController`,如果你还记得控制器章节,这是因为控制器包含了一个leads数组.Ember希望你这样做,因为`ArrayController`定义了像`sortProperties`这样的内容,这些在常规的控制器实例是不可用的.

刷新页面,对你排序后的leads感到惊奇吧.

下面当你点击它时,会显示每个lead的数据.
