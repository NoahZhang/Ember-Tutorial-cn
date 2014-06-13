**前言**
====================
欢迎来的我的Ember教程!

我写这个教程的目的是,当你读完该教程时,可以有足够的知识使用Ember编写你自己的app.我写作的目标是简单,易读和简洁.如果你有任何建议的话,请毫不犹豫的[反馈][1]给我,我很乐意获得反馈并改进这个教程.

**什么是Ember JS**

Ember是一个前端Javascript框架.你可以使用它编写复杂,重量级的web app.它提供了组织良好的架构,约定和内建的方式去处理困难的事情.和Angular,Backbone,Knockout一样,Ember也可以帮助开发者构建伟大的前端应用程序,并且维护一个整洁的代码库.

**为什么选择Ember**

我不能说出所有框架的好处,但是我可以告诉你Ember好在哪里.

**合理的代码组织**.如果你有Rails背景的话,你会非常喜欢Ember的命名约定.例如一个Users Controller对应一个Users View和一个Users Template.

**简单的持久化机制**.一旦你要和后台交互,保存一笔记录,你可以简单的调用user.save().想要删除这个user?调用user.destroyRecord().

**自动更新的模板**.在Handlerbars模板中使用{{ myProperty }}创建和显示一个属性,该属性的任何更新都会及时的呈现. Handlerbars可能并不太优美,如果你使用[Emblem][2](一个像Slim的Handlerbars模板语言)的话,你的模板看起来会相当的舒服.

**非常有用的对象API**.Ember实现了一组特有的对象,每一个都有非常友好的API.例如Ember有一个Array对象,拥有像contains,filterBy和sortBy这样的方法,这些都非常的方便.

如果你发现你需要构建一个复杂的前端应用,我觉得Ember绝对是值得学习的.Ember可能看起来有些庞大,但是它其实一点都不复杂.一旦你学会如何与各种Ember的对象交互,并且掌握了基本的API,你一会就可以编写出极好的前端应用.

**Ember是有趣的**
我使用Ember工作的结论是,他真的非常有趣.对于开发令人着迷的前端应用,并且梦想可以维护一个简洁易读的代码库的开发者,它开创了一个新的可能.而且,Ember核心团队在持续的发布bug修复和改进方面做的非常好,维护了一个稳定的代码库.

在最后,我觉的如果你要选择一个前端框架去学习,选择Ember,你绝对不会失望.

**教程要求**
本教程适合有Javascript和Ruby on Rails基础的开发人员.如果你想跟着该教程学习,应该在你的电脑上安装一个Rails的开发环境.

**JavaScript和CoffeeScript版本**
我原本使用CoffeeScript编写本教程,因为我使用CoffeeScript编写Ember.我觉的它可以产生简洁的代码.如果你要使用Ember做全职开发,我觉的使用CoffeeScript会更适合。

然而,我知道很多人不知道或者不喜欢CoffeeScript,所以我也做了Javascript的版本,使用页面右上方的开关可以切换语言.

做为一个CoffeeScript学习者的先驱,[JS2Coffee.rog][3]可以转换CoffeeScript代码到JavaScript,也可以反过来。

**测试**
我通常使用TDD,并且没有测试的话不发布代码,有一些原因我在本教程中并没有覆盖到测试.

第一,这里有很多东西需要学习,如果测试再掺杂进来,会使教程更加的困难.

第二,你可以使用Cucumber(或者Rspec Intergration)编写整合测试,这比使用Ember内建的测试辅助会更好,因为你可以测试持久化.不管怎样,我觉得你仍然应该为你的Ember代码编写单元测试,我推荐使用[Ember Qunit][4].这里有一个[Ember测试指导][5]，你可以在学习教程后阅读.

如果有很多关于测试的需求,我会写另外的文章来介绍.

**问题和勘误**
如果你有问题或者发现了错误，请发邮件给我[vic@vicramon.com][6],或者提交一个[pull request][7]. 


  [1]: mailto:vic@viramon.com
  [2]: http://emblemjs.com/
  [3]: http://js2coffee.org/
  [4]: https://github.com/rpflorence/ember-qunit
  [5]: http://emberjs.com/guides/testing/
  [6]: mailto:vic@vicramon.com
  [7]: http://www.github.com/vicramon/ember-tutorial
