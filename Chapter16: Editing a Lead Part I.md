Editing a Lead Part I
==========

在应用中有两个地方可以编辑lead,在lead显示时我们会马上看到状态和备注,我们也可以点击“edit”链接,这样我们就可以编辑lead的基本信息,例如姓名,email和电话.

我们从备注和状态开始,这些会添加到我们已经存在的lead模板.

**添加状态下拉选单**

首先我们需要告诉应用一个lead拥有哪些不同的状态.`Lead`模型应该知道这些,和实例的知识相比它应该更多的属于类的知识,所以我们`reopenClass`把它添加到类.

添加如下代码到`Lead`模型文件的最后:

    // app/assets/javascripts/models/lead.js
    App.Lead.reopenClass({
      STATUSES: ['new', 'in progress', 'closed', 'bad']
    });

现在你可以通过`App.Lead.STATUSES`来获得这个数组.

打开Lead模板添加下面代码到底部,和其他p标签在同一行:

    // app/assets/javascripts/templates/lead.js.emblem
    p
      label Status:
      '
      view Ember.Select content=App.Lead.STATUSES value=model.status
      
我们可以使用Ember提供的内建的下拉选单视图,我将`content`指定为状态数组,并且指定`value`到lead的`status`,现在这个下拉选单自动绑定到lead模型的status.

**添加备注**

添加下面的代码到状态下拉选单的后面:

    p
      label Notes:
      br
      view Ember.TextArea value=model.notes

这里我们使用Ember的textarea视图并且把它绑定到`model.notes`.

**添加提交按钮**

在备注栏位的下面添加一个提交按钮:

    p
      input type='submit' value='Save Changes' click='saveChanges'
      
当点击按钮时,`click`特性(attribute)指定的函数会被调用,Ember期望控制器定义这个函数.

**保存记录**

现在除了提交按钮以外,所有的都在我们的表单中.

因为我们还在lead对象流程中,所以创建一个`LeadController`来处理`saveChanges`操作:

    // app/assets/javascripts/controllers/lead.js
    App.LeadController = Ember.ObjectController.extend({
    
      actions: {
        saveChanges: function() {
          this.get('model').save();
        }
      }
    
    });
    
注意这个控制器是一个`ObjectController`,应为它包含一个单独的lead模型.

我们的提交按钮会调用`saveChanges`函数,由于它会被模板的操作调用,所以这个函数必须放在`actions`中,这是一个帮助你保持代码结构的Ember惯例.

`save()`实际上会发送一个ajax put请求到我们的Rails API,并且所有的应该是可以工作的.在你的浏览器试一下,编辑一个记录,点击“Save Changes”,并且查看Chrom控制台的Network页签,你应该可以看到一个到`api/v1/leads/(id of record)`的put请求,如果你刷新页面应该可以看到你的修改.

保存就是这么简单!只需要绑定并且调用`save()`.这是好玩和稳健的.

Ember在保存时总是会发送一个API请求,即使记录没有修改,你可以使用简单的if来阻止它:

    saveChanges: function() {
      if (this.get('model.isDirty')) this.get('model').save();
    }
    
**显示有用的反馈**

现在我们来点花样,在保存时给用户一些反馈,当我们有未保存的变更时,会显示一个信息"unsaved changes",当记录正在保存时会显示"saving…".

马上添加这些信息到提交按钮的下面:

    p
      input type='submit' value='Save Changes' click="saveChanges"
      if isDirty
        .unsaved unsaved changes
      if isSaving
        .saving saving...
        
Ember会在控制中查找`isDirty`和`isSaving`.如果控制器没有找到它们,那么会到模型中去找.`isDirty`和`isSaving`都来自内建的`DS.Model`,所以不会有问题.

在浏览器中测试一下,如果添加了任何的备注或者改变lead的状态,应该可以看到红色的“unsaved changes”显示在保存按钮的旁边.点击“save changes” 会看到绿色的 “saving…”.它们们可能只会很快的出现,因为你在开发阶段,服务器是很快的.

**美化反馈**

这里我们有一个问题,当你保存记录时“unsaved changes”还可以看到.如果你在leads API控制器的`update`操作的最上面添加`sleep 1`,应该可以很容易的看到,这是应为直到保存完成,记录都是脏的,和你期望的一样.我们做一些工作在保存时隐藏这个文本.

我们定义一个属性叫做`showUnsavedMessage`,使用`showUnsavedMessage`替换模板中的`isDirty`:

    if showUnsavedMessage
      .unsaved unsaved changes
      
打开控制添加属性:

    // app/assets/javascripts/controllers/lead.js
    App.LeadController = Ember.ObjectController.extend({
    
      showUnsavedMessage: function() {
        return this.get('isDirty') && !this.get('isSaving')
      }.property('isDirty', 'isSaving'),
    
      // actions, etc...
    
    })

当记录在保存中时,我们新的逻辑会返回false,给我们想要的结果.

这就是编辑的第一部分.勇往直前,天天向上!
