---
layout: default
title: 8.4 如何将视图组合在一起
category: swiftui-containers
---

如果你想将多个视图合成一个视图，那某些情况下应该使用 SwiftUI 的 `Group` 视图来先分组，这一点非常重要。因为出于潜在的技术原因，如果使用 `VStack ` 或 `HStack` 一次最多只能向父视图添加10个子视图。

为了证明这一点，这里有一个包含10个文本的 `VStack`：

{% highlight swift %}
VStack {
    Text("Line")
    Text("Line")
    Text("Line")
    Text("Line")
    Text("Line")
    Text("Line")
    Text("Line")
    Text("Line")
    Text("Line")
    Text("Line")
}
{% endhighlight %}

这是没问题的，但是如果你再添加一个文本视图，那么会出现报错，报错原因类似如下：

{% highlight swift %}
ambiguous reference to member 'buildBlock()'
{% endhighlight %}

具体的报错内容为：

{% highlight swift %}
SwiftUI.ViewBuilder:3:24: note: found this candidate
    public static func buildBlock<C0, C1, C2, C3, C4, C5, C6, C7, C8, C9>(_ c0: C0, _ c1: C1, _ c2: C2, _ c3: C3, _ c4: C4, _ c5: C5, _ c6: C6, _ c7: C7, _ c8: C8, _ c9: C9) -> TupleView<(C0, C1, C2, C3, C4, C5, C6, C7, C8, C9)> where C0 : View, C1 : View, C2 : View, C3 : View, C4 : View, C5 : View, C6 : View, C7 : View, C8 : View, C9 : View
{% endhighlight %}

看上面的报错，出现多次的 C0 …… C9，这是因为 SwiftUI 的视图构建系统会用一个虚拟编号来标示要添加的子视图，但是最多只能 10 个，超过 10 个就报错。

很幸运的是，可以用 `Group` 来解决这个问题：

{% highlight swift %}
var body: some View {
    VStack {
        Group {
            Text("Line")
            Text("Line")
            Text("Line")
            Text("Line")
            Text("Line")
            Text("Line")
        }

        Group {
            Text("Line")
            Text("Line")
            Text("Line")
            Text("Line")
            Text("Line")
        }
    }
}
{% endhighlight %}

这样就突破了 10 个子视图的限制，因为 `VStack` 只包含了两个 `Group` 视图。不过要注意一点 `Group` 视图也有 10 个的限制。