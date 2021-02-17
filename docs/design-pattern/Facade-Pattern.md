# 外观模式

之前介绍过装饰者模式和适配器模式，我们知道适配器模式是如何将一个类的接口转换成另一个符合客户期望的接口的。但 Java 中要实现这一点，必须将一个不兼容接口的对象包装起来，变成兼容的对象。

- 装饰模式：不改变接口，但加入责任
- 适配器模式：将一个接口转换为另一个接口
- 外观模式：让接口更简单



## 问题

假设你必须在代码中使用某个复杂的库或框架中的众多对象。 正常情况下，你需要负责所有对象的初始化工作、 管理其依赖关系并按正确的顺序执行方法等。

最终， 程序中类的业务逻辑将与第三方类的实现细节紧密耦合， 使得理解和维护代码的工作很难进行。 



外观类为包含许多活动部件的复杂子系统提供一个简单的接口。 与直接调用子系统相比， 外观提供的功能可能比较有限， 但它却包含了客户端真正关心的功能。

如果你的程序需要与包含几十种功能的复杂库整合， 但只需使用其中非常少的功能， 那么使用外观模式会非常方便。



## 真实世界类比

当你通过电话给商店下达订单时， 接线员就是该商店的所有服务和部门的外观。 接线员为你提供了一个同购物系统、 支付网关和各种送货服务进行互动的简单语音接口。

![电话购物的示例](https://refactoringguru.cn/images/patterns/diagrams/facade/live-example-zh.png)





## 外观模式结构

![外观设计模式的结构](https://refactoringguru.cn/images/patterns/diagrams/facade/structure.png)

- **外观** （Facade） 提供了一种访问特定子系统功能的便捷方式， 其了解如何重定向客户端请求， 知晓如何操作一切活动部件。

- 创建**附加外观** （Additional Facade） 类可以避免多种不相关的功能污染单一外观， 使其变成又一个复杂结构。 客户端和其他外观都可使用附加外观。

- **复杂子系统** （Complex Subsystem） 由数十个不同对象构成。 如果要用这些对象完成有意义的工作， 你必须深入了解子系统的实现细节， 比如按照正确顺序初始化对象和为其提供正确格式的数据。

  子系统类不会意识到外观的存在， 它们在系统内运作并且相互之间可直接进行交互。

- **客户端** （Client） 使用外观代替对子系统对象的直接调用。



#### Coding




## 外观模式适合应用场景

如果你需要一个指向复杂子系统的直接接口， 且该接口的功能有限， 则可以使用外观模式。

子系统通常会随着时间的推进变得越来越复杂。 即便是应用了设计模式， 通常你也会创建更多的类。 尽管在多种情形中子系统可能是更灵活或易于复用的， 但其所需的配置和样板代码数量将会增长得更快。 为了解决这个问题， 外观将会提供指向子系统中最常用功能的快捷方式， 能够满足客户端的大部分需求。

如果需要将子系统组织为多层结构， 可以使用外观。

创建外观来定义子系统中各层次的入口。 你可以要求子系统仅使用外观来进行交互， 以减少子系统之间的耦合。

让我们回到视频转换框架的例子。 该框架可以拆分为两个层次： 音频相关和视频相关。 你可以为每个层次创建一个外观， 然后要求各层的类必须通过这些外观进行交互。 这种方式看上去与[中介者](https://refactoringguru.cn/design-patterns/mediator)模式非常相似。



## 再来认识外观模式

> 看到外观模式的实现，可能有朋友会说，这他么不就是把原来客户端的代码搬到了 Facade 里面吗，没什么大变化

### 外观模式目的

外观模式相当于屏蔽了外部客户端和系统内部模块的交互

外观模式的目的不是给系统添加新的功能接口，而是为了让外部减少与子系统内多个模块的交互，松散耦合，从而让外部能更简单的使用子系统。

当然即使有了外观，如果需要的话，我们也可以直接调用具体模块功能。

## 实现方式

1. 考虑能否在现有子系统的基础上提供一个更简单的接口。 如果该接口能让客户端代码独立于众多子系统类， 那么你的方向就是正确的。
2. 在一个新的外观类中声明并实现该接口。 外观应将客户端代码的调用重定向到子系统中的相应对象处。 如果客户端代码没有对子系统进行初始化， 也没有对其后续生命周期进行管理， 那么外观必须完成此类工作。
3. 如果要充分发挥这一模式的优势， 你必须确保所有客户端代码仅通过外观来与子系统进行交互。 此后客户端代码将不会受到任何由子系统代码修改而造成的影响， 比如子系统升级后， 你只需修改外观中的代码即可。
4. 如果外观变得过于臃肿， 你可以考虑将其部分行为抽取为一个新的专用外观类。



## 外观模式优缺点

-  你可以让自己的代码独立于复杂子系统。

-  外观可能成为与程序中所有类都耦合的上帝对象。



## 与其他模式的关系

- 外观模式为现有对象定义了一个新接口， [适配器模式](https://refactoringguru.cn/design-patterns/adapter)则会试图运用已有的接口。 *适配器*通常只封装一个对象， *外观*通常会作用于整个对象子系统上。
- 当只需对客户端代码隐藏子系统创建对象的方式时， 你可以使用[抽象工厂模式](https://refactoringguru.cn/design-patterns/abstract-factory)来代替[外观](https://refactoringguru.cn/design-patterns/facade)。
- [享元模式](https://refactoringguru.cn/design-patterns/flyweight)展示了如何生成大量的小型对象， [外观](https://refactoringguru.cn/design-patterns/facade)则展示了如何用一个对象来代表整个子系统。
- [外观](https://refactoringguru.cn/design-patterns/facade)和[中介者模式](https://refactoringguru.cn/design-patterns/mediator)的职责类似： 它们都尝试在大量紧密耦合的类中组织起合作。
  - *外观*为子系统中的所有对象定义了一个简单接口， 但是它不提供任何新功能。 子系统本身不会意识到外观的存在。 子系统中的对象可以直接进行交流。
  - *中介者*将系统中组件的沟通行为中心化。 各组件只知道中介者对象， 无法直接相互交流。
- [外观](https://refactoringguru.cn/design-patterns/facade)类通常可以转换为[单例模式](https://refactoringguru.cn/design-patterns/singleton)类， 因为在大部分情况下一个外观对象就足够了。
- [外观](https://refactoringguru.cn/design-patterns/facade)与[代理模式](https://refactoringguru.cn/design-patterns/proxy)的相似之处在于它们都缓存了一个复杂实体并自行对其进行初始化。 *代理*与其服务对象遵循同一接口， 使得自己和服务对象可以互换， 在这一点上它与*外观*不同。



## 参考与感谢
《图解 Java 设计模式》
《Head First设计模式》
https://refactoringguru.cn/design-patterns/
https://blog.csdn.net/lu__peng/article/details/79117894
https://juejin.im/post/5ba28986f265da0abc2b6084#heading-12