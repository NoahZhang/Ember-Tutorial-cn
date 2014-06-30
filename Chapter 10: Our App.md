Our App
==========

(现在我们要真的开编码了.)

我们要开发一个CRM.这是一个适合使用Ember的项目,因为我们要查询,列表,显示,创建和编辑记录.如果你可以开发这个应用,并且知道它是如何工作的,那你就可以很好的按照自己的方式去开发复杂的前端应用了.

你可以看看应用已经[完成的版本][1],了解一下我们要开发什么.假如你需要参考一下,代码也放在GitHub上: [CoffeeScript Code][2], [Javascript Code][3].

**lead**是这个系统中的主要对象,你可能不知道这个词,lead和潜在客户是同样的意思.

**准备应用**

你可以使用我们之前开发的Hello World应用作为这个应用的基础,我会给你提供样式,这样你就可以专注在Ember的开发.

下载这个[样式表][4],将它保存在`app/assets/stylesheets/application.css`,替换掉原本的`application.css`.

**Vim Projections**

如果你使用Vim,那你就走运了!Vim projections对于快速的打开和创建Ember文件是非常有用的.

这有一个Gist,里面有我使用的projections:[Vim Projections for Ember][5],只需要把它复制到`config/projections.json`,并且重启Vim.

这些projections对Ember相关的内容都使用`Rj`做为前缀,所以有`Rjmodel`、 `Rjcontroller`、 `Rjview`等等.`Rjini`会打开你的Ember的router,这和使用`Rini`打开Rails的Router是一致的.

如果你不使用Vim,考虑学习一下.它会改善你的编程体验,你可以打开命令行并且输入`vimtutor`,获得一个即时的教程.你也可以看看[VimGenius][6],我开发的一个帮助记住命令的小应用.我还写了一本小的[Vim口袋书][7],来帮助你达到中级的水平.

**准备路由**

我们使用AutoLocation API来帮助我们摆脱url中的哈希值,我们也需要将rootURL设置到`'/'`,这Ember才知道从哪里开始解析url.

打开你的router,在顶部添加如下内容:

    // app/assets/javascripts/router.js
    App.Router.reopen({
      location: 'auto',
      rootURL: '/'
    })
    
我们也需呀创建一个全部捕获的Rails路由,来处理我们在Ember中创建的任意的路由,否则当我们尝试在Ember的子路由重新加载页面时,会得到一个404.

    # config/routes.rb
    
    # add this to the bottom of your Rails routes
    get '*path', to: 'home#index'
    

  [1]: http://embercrm.herokuapp.com/
  [2]: https://github.com/vicramon/ember-crm
  [3]: https://github.com/vicramon/ember-crm-js
  [4]: https://gist.githubusercontent.com/vicramon/a6cc84a06cf92f4aa191/raw/be5d148eaaf1b28d56d6e57e1653ff3f43158192/application.css
  [5]: https://gist.github.com/vicramon/6488603
  [6]: http://vimgenius.com/
  [7]: https://github.com/vicramon/vim-tutorial/blob/master/vim-tutorial.txt

