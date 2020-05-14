---
layout: default
title: 5.1 @State, @ObjectBinding 和 @EnvironmentObject 之间的区别
category: swiftui-advanced-state
---

状态在现代程序中几乎是不可避免的，但是于 SwiftUI，重要的一点是，所有的视图都只是一个状态的一个 “映射”，我们不直接改变视图，而是操作状态来影响视图。

SwiftUI 为我们提供了几种在应用程序中储存状态的方法，但是这几种方法略有不同，为了更好地使用 SwiftUI，我们必须了解它们之间的不同之处。

在之前的所有状态示例中，我们都是使用 `@State` 来创建属性，如下：

{% highlight swift %}
struct ContentView : View {
    @State var score = 0
    // more code
}
{% endhighlight %}

这个做法，会在视图中创建一个属性，但它使用了 `@State` 包装器请求 SwiftUI 来管理内存。这很重要：我们所有的视图都是结构（struct），这意味着它们不能够被改变，那我们如果不能为上述的 score 加 1 的话，会不会太无聊了？

因此，当我们用 `@State` 创建一个属性时，我们将它的控制权交给了 SwiftUI，这样只要这个视图存在，那么这个属性就会持久保存。当这个属性（状态）发生变化的时候，SwiftUI 会自动更新视图，以便映射出状态的最新信息。

`@State` 适合于视图的简单属性，并且永远不会在视图之外的地方使用，因此最好把这些属性设定为私有属性，如下：

{% highlight swift %}
@State private var score = 0
{% endhighlight %}

这样就相当于再次强化了，这些状态只为该视图“服务”，并且不会“越界”。

### 什么是 @ObjectBinding？

对于更复杂的属性，比如你想使用自定义类型，而这个类型可能有多个属性和方法，或者这些属性需要在几个视图之间共享，那么应该使用 `@ObjectBinding`。

和 `@State` 非常相似，除了使用从外部引用的类型，而不是像数字或者字符串这样的简单本地属性。

使用 `@ObjectBinding` 必须符合 BindableObject 协议，所以必须有一个 `didChange` 属性，用来数据发生变更的时候通知视图更新，但也可以不更新。

必须使用 `Combine` 框架，该框架的作用是用来通知视图数据已经更新了。




### 什么是 @EnvironmentObject？

已经了解了如何用 `@State` 声明一个简单的属性，该属性变更时候会自动触发视图更新。以及 如何 `@ObjectBinding` 如何声明外部类型的属性，该属性在变更的时候可能会或者不会导致视图更新，取决于视图里的设定，但 `@ObjectBinding` 可以与其它视图共享。

但还有第三种类型属性可以使用，即 `@EnvironmentObject`。它能够让程序里的任何视图都可以共享读写它。因此，如有程序里有些数据需要在任何视图都可以访问，就可以使用该属性，该属性变更的时候，所以相关视图都会自动更新，不会存在不同步的风险。

### 三者区别

`@State` 用在单个视图，一般都标记为私有属性。

`@ObjectBinding` 复杂一点，可以用在多个视图，无论何时使用该类型，都需要使用 `@ObjectBinding`。

`@EnvironmentObject` 可以理解成一个全局变量，任何视图都可以读写。

上述三者中 `@ObjectBinding` 是最有用和最常用的，如果不确定有哪个，那么就选择用 `ObjectBinding` 吧！

