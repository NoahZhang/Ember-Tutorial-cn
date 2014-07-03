Modeling Leads
==========

我们会使用[Ember Data][1]来处理数据,在Hello World应用中我们已经安装了它,所以我们不需要做任何事情就可以开始使用它了.虽然Ember有一些不同的数据适配器,但是Ember Data是标准.

在Rails中我们有一个Lead模型,Ember也需要知道Lead,要让Ember也知道,我们要在Ember中创建一个Lead模型.Ember Data给了我们需要我们扩展的DS.Model对象:

    // app/assets/javascripts/models/lead.js
    App.Lead = DS.Model.extend({
      firstName: DS.attr('string'),
      lastName: DS.attr('string'),
      email: DS.attr('string'),
      phone: DS.attr('string'),
      status: DS.attr('string', { defaultValue: 'new' }),
      notes: DS.attr('string'),
    })

Ember会自动读取json api中的`first_name`到`firstName`,以及其他剩余的特性(attribute).在这里我们只使用了`string`数据类型,其他你可以使用的还有`number`, `boolean`, 和`date`.

**关于DS.Model**

[DS.Model][2]提供了各种有用的方法和属性,这里有一些你经常会用到的方法:

    model.save() // save changes to the database
    model.rollback() // wipe clean any unsaved changes
    model.destroyRecord() // delete a record from the database
    
注意DS.Model继承自Ember.Object,所以所有你学习的Ember对象相关的内容都可以用在 DS.Model.例如,你可以像这样设定DS.Model实例的任意的属性:

    model.set('myProperty', 'hello!');
    
DS.Model实例和常规的Ember对象实例最大的不同是,你不能以同样的方式创建它们.创建的一个新的DS.Model实例必须通过`store`:

    this.store.createRecord('modelName', { firstName: 'John', lastName: 'Snow' });
    
这是因为`store`封装了你的应用中所有活动的DS.Model实例的内容,并且当你创建模型实例时`store`去做一些额外的工作.


  [1]: https://github.com/emberjs/data
  [2]: http://emberjs.com/api/data/classes/DS.Model.html
