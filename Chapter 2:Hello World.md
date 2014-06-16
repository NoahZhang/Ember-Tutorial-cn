.**Hello World!**
====================

首先我们来构建一个Hello World应用,我们需要确保已经安装好了开发环境,并且对Ember如何工作有大概的了解。

**创建一个新的Rails应用**

在本教程中,我们将要构建的是一个CRM应用.当我们开始时,我们可以重用这个hello world应用的代码,我们的Rails应用的名字是ember-crm.

首先创建一个新的rvm gemset去放置我们的gem:

    rvm gemset create ember-crm
    rvm gemset use ember-crm
    gem install rails
    
现在生成Rails应用:

    rails new ember-crm -d postgresql
    cd ember-crm
    bundle
    rake db:create
    
如果你运行`rails s`并且访问localhost:3000,应该可以看到Rails的欢迎页面.

**移除Turbolinks**

你需要移除Turbolinks,因为它和Ember有冲突.确保从下面的位置移除它:

 - Gemfile Application Javascript
 - (app/assets/javascripts/application.js) Layout
 - (app/views/layouts/application.html.erb)

**添加Ember Rails**

在本教程中我们会使用[Ember Rails][1],它非常的稳定和好用.我认为[Ember Appkit Rails][2]最终会替换Ember Rails,做为Rails/Ember默认集成的gem,但是它现在是pre 1.0的版本,所以在本教程中我们将会使用Ember Rails.

添加下面的gem到Gemfile:

    gem 'ember-rails'
    gem 'ember-source'
    gem 'emblem-rails'
    
添加bundle:

    bundle
    
Ember Rails提供了一个生成器,可以为我们生成Ember应用的框架,-n标记告诉我们会将Ember应用命名为`App`,这是一个惯例.

JavaScript:

    rails g ember:bootstrap -n App --javascript-engine js
    
如果你想要生成的文件使用CoffeeScript,那么添加一个标记:

    rails g ember:bootstrap -n App --javascript-engine coffee
    
Ember Rails有一个默认的Ember版本,但是我们需要安装 Ember 1.5.0和Ember Data 1.0.0 beta 7,所以确保你的版本和我的一样.他们将会被安装到`vendor/assets/ember`.

    rails g ember:install --tag=v1.5.0 --ember
    rails g ember:install --tag=v1.0.0-beta.7 --ember-data
    
添加下面的行到你的environment文件,它们会告诉Ember Rails在每个环境使用哪个版本的ember.js.production版本是压缩的并且没有日志,development版本没有压缩并且打开日志.

    # config/environments/test.rb
    config.ember.variant = :development
    
    # config/environments/development.rb
    config.ember.variant = :development
    
    # config/environments/production.rb
    config.ember.variant = :production
    
Ember Rails为我们生成了一个`application.js.coffee`,我们使用它,删除`application.js`.生成的 `application.js.coffee`需要 `jquery`,但是不需要 `jquery_ujs`,所以他必须在`jquery`的下面:

    # app/assets/javascripts/application.js.coffee
    #= require jquery_ujs

**让Ember工作**

我们需要一个基本的Rails controller和view,以便于我们可以使用Ember输出一些东西.我们创建一个controller叫做`HomeController` ,包含一个index view做为他的root path.

添加route:

    # config/routes.rb
    root to: 'home#index'
    
创建controller:

    # app/controllers/home_controller.rb
    class HomeController < ApplicationController
    end

创建一个index view,它是空白的:

    <!-- app/views/home/index.html.erb -->
    
最后但是同样重要的,我们需要为Ember创建一个需要去呈现的template,Ember默认回去寻找一个application template,所以我们需要创建它:

    // app/assets/javascripts/templates/application.js.emblem
    h1 Hello World
    
重启你的server并且访问http://localhost:3000,你会看到屏幕闪显示‘Hello World’.如果你看到它了,那么恭喜!你离Embereño又近了一步,Yes, Embereño is a thing, though I kind of like Emberista. 

如果你没有看到‘Hello World’,你应该复制我的hello world,并且看一下有什么不同.

**非常基础的调试**

如果你打开console,应该可以看到下面这样的输出:

    DEBUG: ------------------------------- ember.js?body=1:3522
    DEBUG: Ember      : 1.5.0 ember.js?body=1:3522
    DEBUG: Handlebars : 1.3.0 ember.js?body=1:3522
    DEBUG: jQuery     : 1.11.0 ember.js?body=1:3522
    DEBUG: -------------------------------
    
如果没有看到,那么你的Ember没有正常的加载.

这里有一篇[debugging Ember][3]的指导,我建议你如果有困难的话看一下.

如果不能工作的话,我做的第一件事情是在代码中放置一个`debugger`,并且打开Chrome开发者工具,如果这些没有帮助,那么下一步,我会log我的route transitions,或的更详细的信息.

    # app/assets/javascripts/application.js.coffee
    window.App = Ember.Application.create
      LOG_TRANSITIONS: true
      LOG_TRANSITIONS_INTERNAL: true
      LOG_VIEW_LOOKUPS: true
      
除了这些以外,我建议阅读这篇这篇指导,获得更详细的调试技巧,当然也要使用Ember Inspector…

**Ember Inspector**

Ember Inspector非常重要的调试Ember的工具,它是一个Chrome的扩展,帮助我们了解应用中发生的事情,你可以从[这里][4]下载它.

安装完成后刷新你的浏览器,并且打开Chrome开发者工具.你应该可以看到一个tab标题是**Ember**,里面有各种各样有用的工具.**View Tree**会精确的显示已经呈现的内容以及他们来自哪里.**Routes**会显示你的应用中所有的routes,以及每一个寻找的其他对象,当前的route会显示为粗体.Data会显示你的应用中所有的active records,你可以点击一笔记录查看它的attributes.

我没有办法说出Ember Inspector所有有用的东西,它使得应用内部的工作变得变得清晰可见.

**总结**

正如你所看到的,我们Ember应用运行起来并没有花费太多的时间,大部分的工作都是在准备Rails应用.

如果你还是存在问题,请见问题贴到下面的评论里.如果你想了解这个hello world应用,可以在[GitHub][5]上找到它.


  [1]: https://github.com/emberjs/ember-rails
  [2]: https://github.com/dockyard/ember-appkit-rails
  [3]: http://emberjs.com/guides/understanding-ember/debugging/
  [4]: https://chrome.google.com/webstore/detail/ember-inspector/bmdblncegkenkacieihfhpjfppoconhi
  [5]: https://github.com/vicramon/ember-hello-world
