---
layout: default
title: 3.2 使用 padding 来设置视图的填充
category: swiftui-view-layout
---

SwiftUI 允许我们使用 `padding()` 修改器来设置视图的填充（padding），在没有参数的情况下使用，它会有一个默认值，如下所示：

{% highlight swift %}
VStack {
    Text("SwiftUI")
        .padding(100)
    Text("rocks")
}
{% endhighlight %}

除了默认值，还有默认四边都填充，如需单边填充，如下：

{% highlight swift %}
Text("SwiftUI")
    .padding(.bottom)
{% endhighlight %}

或者也可以两者一起使用，在特定一侧添加指定数量的填充，如下：

{% highlight swift %}
Text("SwiftUI")
    .padding(.bottom, 100)
{% endhighlight %}

那如果两边，三边呢？ 可以使用 `.init()` 参数。如下：

{% highlight swift %}
Text("SwiftUI")
    .padding(.init(top: 10, leading: 20, bottom: 30, trailing: 40))
{% endhighlight %}
