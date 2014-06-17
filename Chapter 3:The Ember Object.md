**Ember Concepts**
====================

我们马上可以开始编码了,但是我觉的如果你在之前还没有看到过Ember风格的JavaScript,代码可能会不太好理解.所以在我们开始构建应用之前,我会介绍一些Ember的核心概念以及一些我们马上会遇到的Ember对象.

如果你讨厌看代码,可以直接跳到[Our App][1]章节,但是如果你想知道接下来会发生什么,我建议你坚持.

接下来的章节我们将会开始Ember Objects, Routing, Routes, Controllers, Views,和 Templates的旋风之旅.我们只会介绍一些基础知识,以便于我们可以尽快开始编码.在你完成本教程后,你可以通过[Ember Guides][2]进行深入的学习.

**Ember对象**

Ember实现了自己的对象体系,最基础的对象是Ember.Object,在Ember中的所有其他对象都继承自Ember.Object.

大部分时间你都会使用像Ember.Controller或者Ember.View这种继承自Ember.Object的对象,你也可以使用Ember.Object.一个Ember.Object的使用场景是,创建处理一些特殊逻辑的service对象.

如果你在Hello World应用中打开浏览器的控制台,你就可以跟踪这些代码,但是你需要把CoffeeScript转换到JavaScript,或者点击页面上方的开关.

你可以这样实例化一个简单的对象:

    var user = Ember.Object.create();
    
实例化时可以将属性传入create:

    var user = Ember.Object.create({ firstName: 'Sam', lastName: 'Smith' });
    
你可以通过调用对象的`.get`并且传入对象的属性名,获取对象的属性:

    var user = Ember.Object.create({ firstName: 'Sam', lastName: 'Smith' });
    user.get('firstName') == 'Sam' //=> true
    user.get('lastName') == 'Smith' //=> true

通过`.toString()`查看对象,你会看到它只是Ember.Object.

    var user = Ember.Object.create();
    user.toString() //=> <Ember.Object:ember{objectId}>

**定义对象**

到目前为止,我们一直在使用Ember.Object,你可以使用`extend`创建一个Ember.Object的子类.我们想要创建一个user对象的类:

    App.User = Ember.Object.extend();
    
现在我们可以使用create实例化一个user:

    var user = App.User.create();
    
请注意,我把这个User对象放在了`App`里面,Ember需要将所有的应用数据放到一个变量里,Ember开发人员通常使用`App`.所以当你在Ember里定义对象时,总是要把他们放在`App`上面.

现在我们很好的完成了所有的事情,我们可能想要添加一些东西到我们的User对象.

对象里面可以拥有三种类型的成员: properties, functions, 和observers.接下来我们会介绍每一个成员.

**Properties**

这里是你如何定义一个属性:

    App.User = Ember.Object.extend({
      isHuman: true,
      temperature: 98.6,
      favoriteDirector: 'Tarantino'
    })

`isHuman`, `temperature`, 和`favoriteDirector`现在可以通过`.get`获取:

    var user = App.User.create();
    
    user.isHuman; //=> true
    user.favoriteDirector; //=> "Tarantino"
    user.temperature; //=> 98.6

这是Ember属性的一个简单的版本,我们也可以创建一个计算属性,里面做一些工作并且调用其他属性:

    App.User = Ember.Object.extend({
    
      fullName: function() {
        return this.get('firstName') + ' ' + this.get('lastName')
      }.property('firstName', 'lastName')
    
    })
    
我们仔细分析一下这个属性.

首先,我们指定了一个属性名称`fullName`.

第二,我们传给它一个返回我们想要的值的function,我们可以通过`this.get('propertyName')`取得对象中的其他属性,或者在CoffeeScript中使用`@get('propertyName')`.

第三,我们在我们的function中调用`.property()`来告诉Ember这是一个计算属性.

最后,我们必须告诉`property()`这个属性依赖哪些属性,他可以接受一个属性名字的字符串列表.在这种情况下,任何时候`firstName`或者`lastName`的改变都会改变`fullName`,所以我们需要观察他们两个.

**函数**

函数是非常简单的:

    App.User = Ember.Object.extend({
    
      showMessage: function(message) { alert(message) },
    
      showName: function() { this.showMessage(this.get('fullName')) }
    
    })
    
我们`showMessage`函数接受一个参数:需要弹出的消息.`showName`函数调用`showMessage`函数,使用我们user对象的`fullName`属性(假设我们已经实现了`fullName`属性).

这里是我们如何调用一个function:

    var user = App.User.create();
    user.showMessage('it works');

**Observers**

Observers函数在它们观察的任何事物有任何改变时触发,他们看起来像属性,但是在结束时使用`observes()`代替`property()`.

    App.User = Ember.Object.extend({
    
      weightChanged: function() {
        if (this.get('weight') > 400) alert('yikes')
      }.observes('weight')
    
    })
    
每当weight属性改变并且大于400时,触发上面的代码,弹出 “yikes”,你可以观察你想要观察的任何事情:

    App.User = Ember.Object.extend({
    
      bodyObserver: function() {
        alert("You've changed. I feel like I don't even know you anymore.");
      }.observes('weight', 'height')
    
    })

每当`weight`或者`height`改变时,都会触发.

**继承已经存在的对象**

Ember的对象系统是很棒的,它让编写模块化可重用的代码变的简单.

你可以像这样继承任何应用中已经存在的对象:

    App.Animal = Ember.Object.extend({
      likesFood: true
    })
    
    App.Human = App.Animal.extend()
    
    var human = App.Human.create()
    
    human.get('likesFood') //=> true
    
在子对象中的properties, functions, 和observers,会覆盖父对象中的:

    App.Animal = Ember.Object.extend({ likesFood: true })
    
    App.Bird = App.Animal.extend({ likesFood: false })
    
    var bird = App.Bird.create()
    
    bird.get('likesFood') //=> false
    
你甚至可以继承多个对象:

    App.One = Ember.Object.extend()
    App.Two = Ember.Object.extend()
    
    App.Three = App.One.extend(App.Two)
    // or
    App.Three = Ember.Object.extend(App.One, App.Two)
    
继承对象时你在开发Ember时,总是会用到的一个模式,你可以使用它提取常用的功能或者从其他对象获得功能.

**初始化**

所有的ember对象在首次初始化时调用`init`,你可以使用它做一些准备工作.

    App.Human = Ember.Object.extend({
    
      init: function() { alert("I think, therefore I am"); }
    
    })
    
这对一些简单的Ember对象来说是好的,但是如果你使用一些特殊的对象,例如Route或者Controller,那你应该避免使用init,使用Ember的其他约定替代.如果你必须要使用它,确保在`init`函数中调用了`this._super()`,否则你可能会破坏一些事情.

**Reopening对象**

你可以通过调用对象的`reopen`在对象上添加更多的properties, functions, 和observers:

    App.Human = Ember.Object.extend();
    
    App.Human.reopen({ name: 'Señor Bacon' });
    
    var francis = App.Human.create();
    francis.get('name') //=> 'Señor Bacon'
    
如你所见,父对象可以和其他语言中的类的作用一样,你可以通过调用对象的`reopenClass`定义一系列的类方法.

    App.Human = Ember.Object.extend();
    
    App.Human.reopenClass({
      sayUncle: function() { alert("uncle"); }
    })
    
然后调用在对象本身定义的类方法:

    App.Human.sayUncle();
    
Ember对象为我们提供了编写面向对象JavaScript的能力,这是Ember最好的特性之一.



  [1]: http://ember.vicramon.com/our-app
  [2]: http://emberjs.com/guides/
