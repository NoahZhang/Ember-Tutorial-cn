Showing a Lead
==========

当我们点击一个lead时,我们可以看到他显示在右边,我们来实现它.

**添加资源**

首先我们需要添加一个资源来显示一个特定的lead.因为我们想要当我们显示一个lead时,leads清单仍旧保留在页面上,所以这个资源应该嵌套在leads资源下面.

    // app/assets/javascripts/router.js
    App.Router.map(function() {
      this.resource('leads', { path: '/' }, function() {
        this.resource('lead', { path: '/leads/:id' });
      })
    })
    
**创建一个路由对象**

我们需要一个路由对象来获取特定的lead.

    // app/assets/javascripts/routes/lead.js
    App.LeadRoute = Ember.Route.extend({
    
      model: function(params) { return this.store.find('lead', params.id) }
    
    })
    
我们通过`params`参数来访问`id`.

`this.store.find`函数通过id来取得记录,它实际上会返回一个[promise][1],Ember尝试去解析.

**创建模板**

我们的模板会显示lead相关的信息.

    // app/assets/javascripts/templates/lead.js.emblem
    article#lead
      h1= model.fullName
    
      p
        ' Name:
        model.fullName
    
      p
        ' Email:
        model.email
    
      p
        ' Phone:
        model.phone
        
单引号`'`会保留该行尾随的空格.

**链接到每个Lead**

打开你的leads模板,你可以创建每个lead的链接:

    // app/assets/javascripts/templates/leads.js.emblem
    article#leads
      h1 Leads
      ul
        each lead in controller
          link-to 'lead' lead tagName="li"
            lead.fullName
    
    outlet
    
这里发生了两件事情.

第一,我们的`li`变成`link-to`.我们传递给它`tagName="li"`,这样html元素就会是一个`li`.当你使用视图helper事,你可以按照同样的方式设置视图中的任何属性`propertyName="value"`.

第二,我们在模板结束的地方放置了一个`outlet`标记.因为lead路由嵌套在leads下面,它的内容会显示在这个outlet里面.这种情况下标记在DOM中的同一级别上,所以不会有问题.如果有一种情况你需要输出内容到页面上的其他地方,你可以使用路由中的[renderTemplate][2]方法来指定outlet到别的地方.

link-to的好处是当你在连接指向的路由时,它会自动添加一个active类到元素,我们的样式表利用了这一点,来突出显示当前选择的lead.

现在刷新页面并且点击一个lead,你应该可以看到lead的信息显示在右面,应该是很时髦的.

接下来我们会创建表单元素并且保存数据.


  [1]: http://emberjs.com/api/classes/Ember.RSVP.Promise.html
  [2]: http://emberjs.com/api/classes/Ember.Route.html#method_renderTemplate
