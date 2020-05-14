---
layout: default
title: 5.2 使用 @ObjectBinding 创建对象绑定
category: swiftui-advanced-state
---

当使用对象绑定（object binding）的时候有两件事情需要注意：1，`BindableObject` 协议可以和某种数据储存的类一起使用。2, `@ ObjectBinding` 属性包装器是在视图中用于储存可绑定对象的实例。

举个例子，这边有一个 `UserSettings` 的类，它符合 `BindableObject` 协议：

{% highlight swift %}
class UserSettings: BindableObject {
    var didChange = PassthroughSubject<Void, Never>()

    var score = 0 {
        didSet {
            didChange.send(())
        }
    }
}
{% endhighlight %}

首先，`didChange` 是一个 `PassthroughSubject` 的实例，它来自于 Combine 框架，所以你要先引入该框架 `import Combine` 才能使用该类。`PassthroughSubject` 的作用，可以理解成：每当我们想告诉外部我们的对象已经发生变化了，我们就会请求 `PassthroughSubject` 来做这个事情，它会把值传递给任何正在监听变化的视图，继而视图自动更新。

第二，这段代码里的 `PassthroughSubject` 有两个参数，`Void` 和 `Never`。第一个参数 `Void`，意思是 “我不会传值过去”，在本文这个 SwiftUI 例子中，我们不需要传值过去就可以更新视图了，因为 SwiftUI 会自动从 `@ObjectBinding` 绑定的状态中获取最新的状态值。 第二个参数 `Never `，意思是 “我永远不会抛出错误”。当然了，如果需要，也可以自定义一个类似于 `NetworkError ` 的错误类型传过去。

第三，在 `UserSettings` 里我们有一个 `didSet` 观察者（observer）属性附加到 `score ` 属性里。这段里的意思当 `score` 变化时里面的代码就会运行。在这个例子里，`score` 变化的时候都会调用 `didChange.send(())`，它会告诉 `didChange` 发布者（publisher）我们的数据已经更新了，继而触发它的订阅者更新视图。

那在一个视图里，`UserSettings ` 插入的方式如下：

{% highlight swift %}
struct ContentView : View {
    @ObjectBinding var settings = UserSettings()

    var body: some View {
        VStack {
            Text("Your score is \(settings.score)")
            Button(action: {
                self.settings.score += 1
            }) {
                Text("Increase Score")
            }
        }
    }
}
{% endhighlight %}

如代码所示，在 `settings ` 前面我们使用了 `@ObjectBinding` 属性包装器，其它的都大同小异。但是有一点要特别注意的，`settings ` 属性不是被声明成私有属性，这是因为绑定的对象能够在多个视图被使用，所以通常会公开共享。

**注意：**当你使用发布者（publisher）宣布对象已经发生改变时，这必须在主线程上完成。


