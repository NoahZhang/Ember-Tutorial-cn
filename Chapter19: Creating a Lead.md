Creating a Lead
==========

**添加路由**

到现在你知道训练...

    // app/assets/javascripts/router.js
    this.resource('leads', { path: '/' }, function() {
      this.route('new');
    });
    
新建lead将会是一个`route`而不是一个`resource`,因为它不需要在我们的系统中加载一个现有的模型.

**设置栏位**

我们要创建一个`fields`属性,它是一个普通的javascript对象,我们将使用它保存新的lead的特性(attribute),直到我们准备创建它.

我们在路由中设置`fields`为一个空的对象,以便每次我们访问新建lead路由时重置它.

    // app/assets/javascripts/routes/leads_new.js
    App.LeadsNewRoute = Ember.Route.extend({
    
      setupController: function(controller) {
        controller.set('fields', {})
      }
    
    });
    
**创建模板**

这里是我们新的lead模板,注意它放在`templates/leads/`里面,因为它是一个嵌套在`leads`资源下面的`route`.

    // app/assets/javascripts/templates/leads/new.js.emblem
    article#lead
      h1 New Lead
    
      form
        fieldset
          dl
            dt: label First Name:
            dd: view Ember.TextField value=fields.firstName
    
          dl
            dt: label Last Name:
            dd: view Ember.TextField value=fields.lastName
    
          dl
            dt: label Email:
            dd: view Ember.TextField value=fields.email
    
          dl
            dt: label Phone:
            dd: view Ember.TextField value=fields.phone
    
        fieldset.actions
          input type='submit' value='Create Lead' click="createLead"

正如你所见,我已经绑定所有的input到`fields`属性.

submit有一个点击操作叫做`createLead`,现在我们开始处理它.

**处理操作**

创建一个控制器来处理`createLead`操作:

    // app/assets/javascripts/controllers/leads_new.js
    App.LeadsNewController = Ember.Controller.extend({
    
      actions: {
        createLead: function() {
          var self = this;
          var lead = this.store.createRecord('lead', this.get('fields'));
          lead.save().then(function() {
            self.transitionToRoute('lead', lead);
          });
        }
      }
    
    });

这个操作首先调用`createRecord`,我们传入模型的字符串名称和一个包含我们要给新模型的特性(attribute)的对象.因为我们绑定`fields`到所有的特性(attribute),所以我们可以只使用它.如果你打印`fields`,你会看到像这样的`{ firstName: 'Sam', lastName: 'Smith', email: 'sam@example.com', phone: '123-456-7890' }`.

一旦我们保存它,记录被创建,就会跳转到显示lead路由.

还有一种常见的方式来创建记录,我也没有使用过.我可以在路由中创建一个新的记录,并且将记录绑定到输入栏位,我并没有这样做的原因是因为我不想要在用户按下“Create Lead”前,记录显示在leads列表的左边.通过保持`fields`中的特性(attribute),一直到`createRecord`被调用来创建记录,在用户选择创建它之前,记录不会出现在列表中.

**添加链接**

在leads模板中,添加一个链接到新的lead路由:

    // app/assets/javascripts/templates/leads.js.emblem
    article#leads
      h1
        | Leads
        link-to 'leads.new' | New Lead
        
现在刷新试一下,一切都可以工作,但是它并不完美.你可以创建leads,每个attribute都为null,我们应该在这里做一个验证,来防止这种情况.

**执行验证**

Ember的验证库是存在的,如果你想要使用的话,我推荐[Dockyard’s Ember Validations][1].然而,鉴于Ember的强大的对象系统和给客户端的大量的控制它的方法，你不一定非得需要一个类库.

我们要手动实现这些验证,有很多不同的方式来做验证,这只是其中之一.

首先,我们在Leas类中定义一个`valid`方法,我们假设我们只关心显示的姓名:

    // app/assets/javascripts/models/lead.js
    App.Lead.reopenClass({
    
      valid: function(fields) {
        return fields.firstName && fields.lastName
      }
    
    });
    
我们传给这个方法一个对象,包含我们要赋值给lead的attributes,它会告诉我们这个集合中的attributes是否有效或者无效.

现在我们需要修改在新的lead控制器中的`createLead`方法:

    // app/assets/javascripts/controllers/leads_new.js
    createLead: function() {
    
      var self = this;
      var fields = this.get('fields')
    
      if (App.Lead.valid(fields)) {
        var lead = this.store.createRecord('lead', fields)
        lead.save().then(function(lead) {
          self.transitionToRoute('lead', lead)
        });
      } else {
        this.set('showError', true)
      }
    }

我们检查这些字段是否有效,如果有效,创建记录.如果无效,设置`showError`属性为true.现在我们可以使用`showError`属性来给用户显示一个信息:

    // app/assets/javascripts/templates/leads/new.js.emblem
    article#lead
      h1 New Lead
    
      if showError
        .error Leads must have a first and last name.
        
最后一件事:控制器实例保持有效,所以如果你创建时出错,跳转到其他route,然后回来,`showError`属性仍然为true.我们不想这样,所以我们需要在`setupController`的路由中将它默认为false:

// app/assets/javascripts/routes/leads_new.js
setupController: function(controller) {
   // etc...
   controller.set('showError', false)
}

现在如果你在创建时出错,离开,然后回来,表单应该会完全重置.

这就是添加leads.这一章感觉有点枯燥,但是通过创建新的记录,你可以做更多可能的事情,所以这是值得高兴的.

下一章会更有趣:我们马上要做即时搜索leads.



  [1]: https://github.com/dockyard/ember-validations
