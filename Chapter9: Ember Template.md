Ember Template
====================

Ember的模板就是[Handlebars][1]文件,我会介绍一些Handlebars的基础知识,以及Ember提供的一些特有的Handlebars helper.

在我们的应用中,我们将会使用[Emblem][2],它们会被编译到Handlebars,为了帮助你学习,我会使用Handlebars和Emblem来演示一些范例.

**输出Controller属性**

视图可以直接访问controller的属性,只要把他们放在:

Handlebars:

    Name: {{name}}
    
Emblem:

    | Name:
    name
    
Emblem期望每一行的第一个单词,要么是一个controller的属性,一个Handlebars helper,要么是一个html标记,所以如果你要直接输出一个文本,那就使用一个管道: |.
Emblem解析管道后的任何内容为字符串.

**输出视图属性**

模板也可以直接访问视图的属性,但是你必须使用`view`前缀来调用它们。例如:

Handlebars:

    Name: {{view.someViewProperty}}
    
Emblem:

    | Name:
    view.someViewProperty

**If,Else,Unless**

Handlebars给你提供了if,else,和unless.它们只接受一个参数:

Handlebars:

    {{#if isBirthday}}
      <div class="celebrate">Happy Birthday!</div>
    {{else}}
      <div class="too_bad">Nope.</div>
    {{/if}}
    
Emblem:

    if isBirthday
      .celebrate Happy Birthday!
    else
      .too_bad Nope.
      
在Handlebars中不允许使用and或者or,它被设计为包含零应用程序逻辑,如果你需要一些布尔类型的组合,那你需要再controller中处理.

你可以使用嵌套的if表达式,但是它们会很快变的丑陋.

上面的例子真实的显示了为什么我更喜欢Emblem,Handlebars的版本需要112个字符,Emblem只需要57个,几乎是前者代码的一半!更少的代码,只要它是可读的,在我的书中总是会赢.

**循环**

Handlebars提供了两种方式来处理循环,第一种可以让你使用`this`访问当前的对象:

Handlebars:

    {{#each users}}
      {{this.name}}
    {{/each}}
    
在Emblem中隐含会去调用`this`,然而,如果你需要的话,仍然可以访问`this`:

    each users
      name
      
第二种处理循环的方式是命名对象:

Handlebars:

    {{#each user in users}}
      {{user.name}}
    {{/each}}
    
Emblem:

    each user in users
      user.name
      
如果你想要遍历一个`ArrayController`的记录,也是一样的,只要传递它的`controller`:

Emblem:

    each user in controller
      user.name
      
我想到现在你可能对Handlebars有了一些概念,所以我接下来就仅仅展示Emblem.

**渲染,视图,Partial Helpers**

模板有一些辅助方法,允许你渲染其他controller,视图和模板.这有助于你重用和划分逻辑.

`render`辅助方法调用一个controller:

    render 'user'
    
这将会寻找`UserController`,并且实例化它.这个controller然后会寻找`UserView`和`user`模板,和平常的Ember对象流一样.

你可以选择向`render`方法传递一个模型对象:

    render 'user' model
    
`view`辅助方法调用一个view:

    view 'user'
    
这将会寻找`UserView`,然后寻找一个叫做`user`的模板.使用`view`辅助方法意味着Ember不会实例化一个controller,会跳过它.

`partial`辅助方法只能调用模板:

    partial 'user'
    
这将会在当前模板中渲染user模板,它不会使用控制器或者视图.不像Rails,Ember不会期望你的模板使用下划线做为前缀.

正如你所见,`render`,`view`和`partial`辅助方法,是你能够尽可能多的按照你的需要来划分代码.如果你只是需要显示一些额外的标记,只需要使用`partial`.如果你需要有一些JavaScript的标记,使用`view`.如果你需要有权访问自己的属性和操作的标记,那你需要使用`render`.

Ember文档提供一个极好的[比较表格][3],有助于解释这些辅助方法的不同.

**操作**

你可以在模板中的任何元素上指定操作,操作会调用controller中的同名function:

    h1 click="tickle" Tickle Me
    
当用户点击h1时,将会调用controller中的`tickle`方法,这个方法必须定义在actions对象中:

    App.MyController = Ember.Controller.extend({
    
      actions: {
        tickle: function() { alert('hahaha') }
      }
    
    })
    
**Link-To辅助方法**

Ember提供了一个`link-to`辅助方法,它会帮你跳转到不同的路由,你将路由的名称和任何你需要发送的模型传递给它.

假设你有一个array controller,给你一个用户清单,你想要给每一个提供一个连接,这里是你将如何做:

    each userRecord in controller
      link-to 'user' userRecord
        userRecord.name
        
好了,现在我们已经涵盖了路线,控制器,视图和模板,我们可以开始实际构建应用了.


  [1]: http://handlebarsjs.com/
  [2]: http://emblemjs.com/
  [3]: http://emberjs.com/guides/templates/rendering-with-helpers/#toc_comparison-table
