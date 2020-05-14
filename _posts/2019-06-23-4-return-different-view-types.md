---
layout: default
title: 3.4 如何返回不同的视图类型
category: swiftui-view-layout
---

在主视图的 `body` 里我们返回的是 `some View`，Swift 知道这是一个视图，并且有特定的返回类型。例如，有一个翻硬币的随机程序，正面显示图片视图 “你赢了”，背面显示文本视图 “下次好运”，那这种情况我们是不能这么写的：

{% highlight swift %}
var body: some View {
    if Bool.random() {
        Image("example-image")
    } else {
        Text("Better luck next time")
    }
}
{% endhighlight %}

从代码字面，`body` 将有可能返回 `Image` 或 `Text` 视图，但是这是错误的，不允许，因为 Swift 不知道返回的是什么类型。有两种方法可以解决这个问题，第一种方法是把这段判断逻辑放在一个组（group）视图里面，因为无论是图片还是文本，最后返回的都是一个 `Group`。

{% highlight swift %}
var body: some View {
    Group {
        if Bool.random() {
            Image("example-image")
        } else {
            Text("Better luck next time")
        }
    }
}
{% endhighlight %}

或者使用 SwiftUI 为我们提供的一个 “类型擦除包装器（ type-erased wrapper）“ 叫做 `AnyView `，我们可以做如下操作：

{% highlight swift %}
var body: some View {
    if Bool.random() {
        return AnyView(Image("example-image"))
    } else {
        return AnyView(Text("Better luck next time"))
    }
}
{% endhighlight %}

尽管 `Group` 和 `AnyView` 都为布局实现了相同的结果，而且 `AnyView` 这个方法看起来好像更宽泛，但是还得考虑一点，那就是性能成本。所以通常最好使用 `Group`，因为它对 SwiftUI 更高效。