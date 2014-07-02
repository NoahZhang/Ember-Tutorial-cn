**Creating the Layout**

我们需要创建我们的布局,这样我们就可以在它里面填充内容.现在开始吧.

**Rails布局**

你的Rails布局开始时是这样的:

    <!-- app/views/layouts/application.html.erb -->
    <!DOCTYPE html>
    <html>
      <head>
        <title>Ember CRM</title>
        <%= stylesheet_link_tag    'application'  %>
        <%= javascript_include_tag 'application' %>
        <%= csrf_meta_tags %>
      </head>
      <body>
        <%= yield %>
      </body>
    </html>
    
Ember默认会将自己插入到body标签结束的地方,这对我们的应用来说是好的.如果你需要特别指定Ember在哪里呈现,你可以执行下面的操作:

首先,创建一个div,id是`ember-app`.

    <body>
      <%= yield %>
      <div id="ember-app"></div>
    </body>
    
现在,打开`application.js`并且告诉Ember `rootElement`是`ember-app`:

    App = Ember.Application.create({rootElement: '#ember-app'})
    
现在,Ember会在`ember-app`div里面呈现所有的东西.

**Rails视图**

你为Hello Workd创建的Rail视图应该还在,但是还是空的.

**Ember布局**

现在,我们面临一个有趣的问题.通常当你在Rails中开发Ember时,你会面临这样的问题,我们应该在Rails或者Ember中建立布局吗?这真的取决于:

如果你的Ember应用是一个单页应用,那么在Rails中建立布局是有意义的,以便于它们可以被非Ember的页面使用.但是如果你的Ember应用实际上有多个页面,并且布局链接到这些Ember页面,那么在Ember中建立布局是有意义的.

在我们的应用中,仅仅只有一个使用Ember的页面,所以我们在Rails中建立布局就可以了.但是,本教程的内容都是关于学习Ember的,所以在Ember中建立布局.

Ember总是渲染`application`模板,所以我们使用它来布局:

    // app/assets/javascripts/templates/application.js.emblem
    header
      article
        .logo
          h1
            a href="#" Ember CRM
    
    section#main
      = outlet
    
    footer
    
现在,如果你刷新,你应该可以看到顶部橙色的Ember CRM横幅.接下来我们会处理获取和显示数据.
