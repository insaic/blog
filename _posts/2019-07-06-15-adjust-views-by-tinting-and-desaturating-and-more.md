---
layout: default
title: 11.15 调整视图的亮度、色彩、色相，饱和度等

category: swiftui-transform
---

SwiftUI 可以使用各种修改器来调整视图的亮度（brightness），色彩（tint），色相（hue），饱和度（saturation）等等。

例如，创建一个视图，并把视图的调整为红色，如下：

{% highlight swift %}
Image("example-image")
    .colorMultiply(.red)
{% endhighlight %}

任何视图都可以调整饱和度，饱和度为 0 则整个视图是黑白灰的，为 1 则是正常色彩，如下：

{% highlight swift %}
Image("example-image")
    .saturation(0.5)
{% endhighlight %}

也可以使用 `contrast()` 修改器来调整视图的对比度，范围值也是从 0 至 1。为 0 的时候就是一副灰色图片，1 的时候则是原始不变。示范一下：

{% highlight swift %}
Image("example-image1")
    .contrast(0.5)
{% endhighlight %}
