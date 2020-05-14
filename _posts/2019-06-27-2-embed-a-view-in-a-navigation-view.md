---
layout: default
title: 8.2 向导航视图插入其它视图
category: swiftui-containers
---

SwiftUI 的导航视图 `NavigationView` 或多或少有点类似于 UIKit 的 `UINavigationController`。它能够处理视图之间的导航，并在屏幕的顶上显示一个导航栏。

举个例子，最简单的插入方式，将文本视图放入导航视图中，如下所示：

{% highlight swift %}
NavigationView {
    Text("This is a great app")
}
{% endhighlight %}

但是在默认情况下，顶部导航栏是空的，所有我们可以使用 `navigationBarTitle() ` 修改器添加一个标题，那这样导航栏就有内容了，如下：

{% highlight swift %}
NavigationView {
    Text("SwiftUI")
        .navigationBarTitle(Text("Welcome"))
}
{% endhighlight %}

默认情况下，导航栏显示的是个左对齐大字号的标题，但是 `navigationBarTitle()` 也提供了自定义选项，如果想常规显示，可以把显示属性设成 `inline` 模式。

{% highlight swift %}
.navigationBarTitle(Text("Welcome"), displayMode: .inline)
{% endhighlight %}

如果想显示大号标题，把显示模式（displayMode）设置成 `.large` 即可。