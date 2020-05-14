---
layout: default
title: 3.1 使用堆栈视图 VStack 和 HStack
category: swiftui-view-layout
---

SwiftUI 里必须有一个视图作为根视图显示在屏幕上，但是这个根视图可以有多个子视图，那就需要来定义这些子视图要如何排版，于是就有了堆栈视图（Stack View）的概念。

跟 UIKit 里的 `UIStackView` 一样，有三个排版模式：横向 `HStack `，纵向 `VStack `，Z轴（从后到前）`ZStack`。

先从单个视图开始，比如一个文本视图：

{% highlight swift %}
Text("SwiftUI")
{% endhighlight %}

那如果我们想在该文本视图下再排版一个视图：

{% highlight swift %}
var body: some View {
    Text("SwiftUI")
    Text("rocks")
}
{% endhighlight %}

这样写是错的，因为在 `body` 里只能返回一个视图，如果有两个或多个视图，必须把它们 “封装” 成一个视图。如下：

{% highlight swift %}
var body: some View {
    VStack {
        Text("SwiftUI")
        Text("rocks")
    }
}
{% endhighlight %}

以上返回的是一个 `VStack`，可以从屏幕上看到两个文本垂直排列，并且居中于屏幕中间，两个视图之间有一定的默认空隙。那如果要水平排列，只需将 `VStack` 换成 `HStack` 即可。如下：


{% highlight swift %}
HStack {
    Text("SwiftUI")
    Text("rocks")
}
{% endhighlight %}
 
刚说到，视图之间有一个默认空隙，那如何设置这个默认值呢？ 可以添加 `spacing` 参数，方法如下：

{% highlight swift %}
VStack(spacing: 50) {
    Text("SwiftUI")
    Text("rocks")
}
{% endhighlight %}

`spacing` 为 `0` 即为默认间隙，如果要变得更小，可以使用负数为参数 `VStack(spacing: -10)`。如果想在两个视图之间做一个更好的视觉区分，可以在两个视图之间加入一个分隔符（Divider）视图，如下：

{% highlight swift %}
VStack {
    Text("SwiftUI")
    Divider()
    Text("rocks")
}
{% endhighlight %}

在默认情况下，堆栈视图都是居中对齐的，那如果想调整对齐方式，可以添加 `alignment` 参数。如下：

{% highlight swift %}
VStack(alignment: .leading) {
    Text("SwiftUI")
    Text("rocks")
}
{% endhighlight %}

也可以和间距同时使用：`VStack(alignment: .leading, spacing: 20)`。此时 “SwiftUI” 和 “rocks” 两个文本视图将是左边对齐，但是它们能是在屏幕中间，这是因为堆栈视图所占的空间宽度就这么大，宽度等于大的子视图。如果是要居于屏幕左边对齐，那么可以添加堆栈视图的 `.frame()` 修改器。