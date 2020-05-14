---
layout: default
title: 11.10 调整视图的透明度
category: swiftui-transform
---

 跟 UIKit 中 UIView 的 alpha 属性一样。SwiftUI 可以使用 `opacity()` 来调整视图的透明度，接受的参数 0（完全透明）至 1（完全不透明）之间的值。

例如创建一个红色背景的文本视图，然后设成 30% 的透明度。如下：

{% highlight swift %}
Text("Now you see me")
    .padding()
    .background(Color.red)
    .opacity(0.3)
{% endhighlight %}