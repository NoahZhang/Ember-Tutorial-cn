Deleteing a Lead
==========

这可能是最短的章节,在Ember中删除通常由一个操作来处理.

**添加删除链接**

打开lead模板,并且在提交按钮的正上方添加一个删除链接:

    // app/assets/javascripts/templates/lead.js.emblem
    p
      a.delete href='#' click="delete" delete
      input type='submit' value='Save Changes' click="saveChanges"
      # etc ...
      
**创建控制器操作**

在控制器中处理模板的行为,所以打开`LeadController`,添加一个`delete`操作:

    // app/assets/javascripts/controllers/lead.js
    actions: {
    
      delete: function() {
        var self = this;
        this.get('model').destroyRecord().then(function() {
          self.transitionToRoute('leads');
        });
      }
    
    }

我们调用模型的`destroyRecord()`,发送一个`DELETE`请求到服务器.删除记录后,我们需要跳转会`leads`路由.

这就是它!

接下来,我们会介绍添加一个新的lead.
