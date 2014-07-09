Editing a Lead Part II
==========

现在我们来结束编辑的下半部分.在这个场景中用户点击 “edit” 链接,看到lead的可以编辑的栏位姓名,电话和email.

这个UI需要替换显示lead的UI,所以我们要做一些特别的工作来处理它.

**添加路由**

和平常一样,首先添加一个路由,我们在已经存在的`lead`资源的下面放置一个`edit`路由,`lead`资源处理lead的抓取,如果我们不需要,不应该重复这项工作.

    // app/assets/javascripts/router.js
    this.resource('lead', { path: 'leads/:id' }, function() {
      this.route('edit')
    })
    
这个路由会去寻找`LeadEdit`控制器,视图和模板.

**创建模板**

我们要先添加模板,因为它会告诉我们什么行为需要在控制器中处理.

由于这个`route`嵌套在`resource`里面,Ember期望模板放置在和resource同名的目录中,所以模板应该放在`app/assets/javascripts/templates/lead/edit.js.emblem`.

如果你不确定在哪里放置模板,只需要看一下Ember Inspector的“Routes”页签:

    // app/assets/javascripts/templates/lead/edit.js.emblem
    article#lead
      h1
        model.fullName
    
      form
        fieldset
          dl
            dt: label First Name:
            dd: view Ember.TextField value=model.firstName
    
          dl
            dt: label Last Name:
            dd: view Ember.TextField value=model.lastName
    
          dl
            dt: label Email:
            dd: view Ember.TextField value=model.email
    
          dl
            dt: label Phone:
            dd: view Ember.TextField value=model.phone
    
        fieldset.actions
          input type='submit' value='Save Changes' click="saveChanges"
          a.cancel href="#" click="cancel" cancel
          
在Emblem中你可以使用冒号`:`嵌套在同一行的元素.

Ember提供了呈现文本框的`Ember.TextField`视图,只需要将你想要绑定的属性赋值给`value`.

`form`标签实际上没有做什么,只是一个标记.

这个模板有两个操作:`saveChanges`和`cancel`.现在我们来实现他们.

**创建控制器**

    // app/assets/javascripts/controllers/lead_edit.js
    App.LeadEditController = Ember.ObjectController.extend({
    
      actions: {
    
        saveChanges: function() {
          var self = this;
          this.get('model').save().then(function() {
            self.transitionToRoute('lead');
          })
        },
    
        cancel: function() {
          this.get('model').rollback();
          this.transitionToRoute('lead');
        }
    
      }
    
    })
    
这个部分相当简单,然而你可能不熟悉`.then`.`save()`返回一个`Promise`对象,当promise resolved时,我们可以调用`.then`来执行代码.基本上这意味着,直到服务器确认已经保存model之前,`transitionToRoute`不会被调用.

**添加Edit链接**

我们需要一个方式访问新的路由,在`lead`模板中的`h1`标签中添加一个链接,别担心,样式表会让它看起来很漂亮的.

    // app/assets/javascripts/templates/lead.js.emblem
    h1
      model.fullName
      link-to 'edit' 'lead.edit' model classNames='edit'
      
第一个参数'edit',链接的文本,第二个'lead.edit',路由的名称.

**试一下**

打开浏览器,尝试点击edit链接.你应该可以看到URL改变,但是什么也没有发生.你能找出原因吗?

Outlets,别忘了它们.如果一个模板没有出现,总是要问自己:**我添加outlet了吗?**如果你像我一样,会在一些点上忘记一个,并且非常生气模板没有出现.

我们的`edit`路由嵌套在`lead`下面,所以Ember尝试渲染模板到`lead`模板中的`outlet`标签.因为找不到它,所以没有发生任何事情.

添加一个outlet在`lead`模板的顶部:

    // app/assets/javascripts/templates/lead.js.emblem
    outlet
    
现在试一下,它应该可以工作了,但是现在我们有一个新的问题:显示lead的UI仍然存在.这是因为**嵌套路由意味着嵌套UI**.因为`lead`资源仍然是激活的,所以UI也是激活的.

这有一个简单的解决方法,当渲染是我们只需要隐藏显示的UI.

**我听说你喜欢编辑**

现在我们会在`lead`控制器中设置一个`isEditing`属性,当它是true时,隐藏我们不想看到的UI.

首先添加`unless isEditing`到模板,并且缩进所有显示的UI在他下面:

    // app/assets/javascripts/templates/lead.js.emblem
    outlet
    
    unless isEditing
      article#lead
        h1
          fullName
          link-to 'edit' 'lead.edit' model classNames='edit'
      // etc...
      
现在每当我们访问edit路由时,都需要设置`isEditing`为true.我们可以在`LeadEdit`路由中来做,我们还没有开始做过,所以现在开始:

    // app/assets/javascripts/routes/lead_edit.js
    App.LeadEditRoute = Ember.Route.extend({
    
      activate:   function() { this.controllerFor('lead').set('isEditing', true) },
      deactivate: function() { this.controllerFor('lead').set('isEditing', false) }
    
    })
    
现在我们看到那些路由的hooks派上了用场!在`activate`中我们获取`LeadController`并且设置`isEditing`为true.在`deactivate`中我们做相反的.Boom,我们已经完成了.

为了更加的明了,我们可以做最后一件事,添加`isEditing`到`LeadController`,并且让它默认为false.

    // app/assets/javascripts/controllers/lead.js
    App.LeadController = Ember.ObjectController.extend({
    
      isEditing: false,
    
      // etc...
    
    })
    
这样未来的程序员(或者我们自己)会知道在这个控制器中我们有一个isEditing属性,它的默认值为`false`.我们并不是必须要这样做,但是我认为这是一个良好的风格.

现在我们可以编辑leads的一切,我会向你展示如何删除它们.
