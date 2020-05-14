---
layout: default
title: 11.14 视图混合

category: swiftui-transform
---

这里视图的混合概念类似于 Photoshop 的图层混合。使用 `ZStack` 将一个视图叠加到另一个视图上，然后在前面的一个图层使用 `blendMode()` 修改器来设置混合（重叠）方式。

{% highlight swift %}
ZStack {
    Image("example-image")
    Image("example-image1")
        .blendMode(.multiply)
}
{% endhighlight %}

完整的 blendMode 可选模式，可以参考文档：<a href="https://developer.apple.com/documentation/swiftui/blendmode" target="_blank">https://developer.apple.com/documentation/swiftui/blendmode</a>
