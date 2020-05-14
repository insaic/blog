---
layout: default
title: 10.3 使用 presentationlink 呈现（present）视图
category: swiftui-presenting
---

之前两篇讲的是在导航栏 push 入新视图，还有一种方式呈现视图，present ，比如弹出对话框，弹出一个视图，再关闭。跟 `UIViewController ` 的 `present()` 方法对应的，SwiftUI 有一个 `PresentationLink`。

**提示**: 在 beta 1 和 2 的 Xocde  它叫做 `PresentationButton`。

举个例子，比如你有一个详情视图要展示：

{% highlight swift %}
struct DetailView: View {
    var body: some View {
        Text("Detail")
    }
}
{% endhighlight %}

然后 present 实现方式：

{% highlight swift %}
struct ContentView : View {
    var body: some View {
        PresentationLink("Click to show", destination: DetailView())
    }
}
{% endhighlight %}

跟 `NavigationLink` 不同的是，`PresentationLink` 不需要导航视图（NavigationView）。
