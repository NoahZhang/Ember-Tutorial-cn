Ember View
====================

视图是Ember中最强大的对象之一.你可以认为视图是模板的一个包装,它包含可能要在模板上执行的所有的JavaScript,并且管理围绕属性和类名的逻辑.

**与控制器对话**

视图可以通过`this.get('controller')`取得controller,视图在默认情况下,没有当前的model,所以要获得model,你必须调用`this.get('controller.model')`,要获得controller的一个属性,你需要`this.get('controller.myProperty')`.

**视图Hooks**

和路由一样,视图也有你可以使用的一系列的hooks,这里有三个你会常用到的,已找它们调用的顺序.

    App.UserView = Ember.View.extend({
    
      willInsertElement: function() {  },
    
      didInsertElement: function() {  },
    
      willDestroyElement: function() {  }
    })
    
`willInsertElement`在将视图插入DOM前调用.

`didInsertElement`在将视图插入DOM后马上调用.这是你运行特定模板的JavaScript时最常使用的函数,它对调试也有帮助,如果框架不工作,你可以在didInsertElement中放一个`debugger`或者`console.log`,来获得更详细的信息.

`willDestroyElement`当视图将要从DOM中移除时调用,你可以使用它做一些必要的清理工作.

**计算别名**

这个和视图不是特别相关,但是我要使用一个计算别名,所以我解释一下它们是什么.计算别名本质上是从其他对象抓取属性的一个简写.

你只需要将你要查找的属性的名称字符串传递给`Em.computed.alias`(Em.是Ember.的别名).这将会查找并且同时观察这个属性.例如:

    App.MyController = Ember.Controller.extend({
      name: 'Zelda'
    })
    
    App.MyView = Ember.View.extend({
    
      // this is kind of lame
      name: function() {
        this.get('controller.name')
      }.property('controller.name'),
    
      // instead, use a computed alias
      name: Em.computed.alias('controller.name')
    
    })

Ember提供了一整套各种计算函数来方便你创建这种简写的属性.例如,`Em.computed.not`返回一个相反的布尔值.你可以在[Ember API][1]查看完整的清单.

**定义元素**

当我说视图包装模板时,我是非常认真的.视图默认将模板包在一个`div`中,并且产生一个ember id,像ember45.

我们可以非常简单的自定义这个包装元素.

    App.UserView = Ember.View.extend({
      tagName: 'article',
      classNames: ['myClass', 'anotherClass']
    })
    
`tagName`指定视图使用的html元素.

`classNames`让你可以添加你要在元素上使用的任意的css类名称.

如果你真的需要给元素指定一个id,你可以使用`elementId: 'myId'`,但是我建议你如果可能的话不要这样做.视图被设计用来重用,所以可能会有超过一个相同视图的实例出现在页面上,id对应每个单独的元素.

**类绑定**

现在看一些好玩的东西,假如你想要动态的分配类名称,Ember已经支持了.

    App.AnimalView = Ember.View.extend({
      classNameBindings: ['active'],
      active: Em.computed.alias('controller.model.isActive')
    })
    
classNameBindings会寻找并且执行你指定的属性,如果active属性返回true,这时active类会出现,否则不会出现.

但是如果你需要比这个更多的控制,试试这个:

    App.AnimalView = Ember.View.extend({
      classNameBindings: ['soundClass']
    
      soundClass: function() {
        if (this.get('model.kind') == "cat") {
          return "meow"
        } else {
          return "woof"
        }
      }.property('model.kind')
    })

这种情况下,因为返回值不是布尔类型,所以返回值会被做为类名.如果返回值是undefined,则什么都不做.这里,如果model是一个cat,则会应用`meow`类,如果是dog的话,则会应用`woof`.

这里有一个你可以使用的简短的版本:

    App.AnimalView = Ember.View.extend({
      classNameBindings: ['isCat:moew:woof'],
      isCat: Em.computed.equal('model.kind', 'cat')
    })
    
如果属性返回ture,那么冒号后面的第一值会被应用,否则第二个值被应用.注意在第一种情况下,如果只需要应用一个类的话,那么第二个值可以省略.

**特性(Attribute)绑定**

特性绑定也以类似的方式工作,但是有一些不同.假设我们要显示一个图像,并且model有一个特性叫做`src`:

    App.ImageView = Ember.View.extend({
      tagName: 'img',
      attributeBindings: ['src'],
      src: Em.computed.alias('controller.model.src')
    })

这是特性绑定最简单的场景,Ember会创建一个名字为`src`的属性(Property),特性的值是属性的返回值.

通常情况下,你希望属性和特性有不同的名字,这种情况下可以像这样做`propertyName:attributeName`:

    App.ImageView = Ember.View.extend({
      tagName: 'img',
      attributeBindings: ['srcProperty:src'],
      srcProperty: Em.computed.alias('controller.model.src')
    })

现在`src`特性被设置为`srcProperty`的结果.

**指定不同的模板**

`UserView`会查找一个叫做`user`的模板,如果你想要使用一个不同的,你可以指定`templateName`:

    App.UserView = Ember.View.extend({
      templateName: 'someOtherTemplate'
    })
    
**取得当前的元素**

通常你需要取得当前的元素,它做为element提供给你,像这样:

    App.UserView = Ember.View.extend({
    
      didInsertElement: function() {
        console.log(this.get('element'));
      }
    
    })

这将会记录这个普通的元素.

需要获得这个元素,并且在她上面运行一些jQuery,你可以使用 $(this.get('element')), 但是太长了, Emeber给了你一个快捷键: this.$().

    App.UserView = Ember.View.extend({
    
      didInsertElement: function() {
        this.$('.someClass').fadeOut()
      }
    
    })
    
这将会在当前视图中查找`.someClass`,并且让它消失.

**处理事件**

像click,doubleClick和mouseEner这种UI事件可以被视图访问,使用事件名称定义一个函数,当事件发生时,它会被调用:

    App.UserView = Ember.View.extend({
      click: function() { console.log('clicked') },
      mouseEnter: function() { console.log('mouse entered') },
      mouseLeave: function() { console.log('mouse left') }
    })
    
事件监听器会被应用到整个视图,所以点击视图里面的任何地方,都会触发点击函数.

视图事件的完整清单可以在[Ember docs][2]找到.

这就是视图,接下来我会介绍模板,它是我们开始开发应用之前的最后一章,坚持住啊!


  [1]: http://emberjs.com/api/
  [2]: http://emberjs.com/api/classes/Ember.View.html#toc_event-names
