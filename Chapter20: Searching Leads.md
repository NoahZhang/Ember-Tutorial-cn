Searching Leads
==========

我们创建一个搜索框,当我们输入时,会根据名字即时搜索leads.

巧妙只之处是,它可以通过一个非常简洁的方式,使用令人惊奇的少量的代码来完成.我们朋友,这就是Ember的美丽之处.

**添加搜索栏位**

在leads列表的上方添加一个文本框视图,就在`h1`的下面:

    // app/assets/javascripts/templates/leads.js.emblem
    article#leads
      h1
        | Leads
        link-to 'leads.new' | New Lead
      view Ember.TextField value=search placeholder="search" classNames="search"
      ul
      # etc ...
      
我将文本框的值绑定到叫做search的属性.

**改变循环**

现在我们使用`each lead in controller`.我们需要手动修改leads列表,把它改成`each lead in leads`,我们需要在控制器中创建一个`leads`属性.

    // app/assets/javascripts/templates/leads.js.emblem
    ul
      each lead in leads
      # etc ...
      
**筛选Leads**

打开`LeadsController`,添加下面两个属性:

    // app/assets/javascripts/controllers/leads.js
    leads: function() {
      return this.get('search') ? this.get('searchedLeads') : this
    }.property('search', 'searchedLeads'),
    
    searchedLeads: function() {
      var search = this.get('search').toLowerCase()
      return this.filter(function(lead) {
        return lead.get('fullName').toLowerCase().indexOf(search) != -1
      })
    }.property('search', 'this.@each.fullName')
    
`leads`属性查看是否有一个搜寻字符串,如果有的话,返回`searchedLeads`.如果没有,返回`this`.`this`在`ArrayController`中引用它包装的模型数组.

`searchedLeads`取得搜寻字符串,将它变成小写字母.然后它在`this`上面运行`filter`,它是leads的列表,返回姓名中包含搜寻字符串的leads.

`searchedLeads`需要依赖'this.@each.fullName',这意味着每当任何lead的姓名改变时,属性都会被更新.

**试一下**

现在它应该可以正常工作,注意这里有两个很酷的事情.

第一,当你搜索列表中的姓名时,它们保持按照姓名排序,这是`sortProperties`在起作用.

第二,尝试点击一个lead,然后输入一个和它不匹配的搜寻字符串.接着,编辑lead的姓名为匹配的内容,会看到它出现在搜寻列表中.这真是太酷了,它是将所有的东西绑定在一起的一个很好的范例.

