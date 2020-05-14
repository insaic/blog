---
layout: default
title: 2.7 如果使用图片作为视图的背景
category: swiftui-text-images
---

除了可以使用纯颜色，渐变颜色来渲染背景之外，在 `background()` 修改器里同样可以使用图片来渲染。

举个例子，在一个文本视图里放置一张背景图片，代码如下：

{% highlight swift %}
Text("Hello World")
    .font(.largeTitle)
    .background(
        Image("example-image")
            .resizable()
            .frame(width: 100, height: 100))
{% endhighlight %}

在 SwiftUI 里除了可以用图片来作为背景，还可以使用其它视图来作为背景，举个例子，比如在背景里放置一个圆形图案，如下：

{% highlight swift %}
Text("Hello World")
    .font(.largeTitle)
    .background(Circle()
        .fill(Color.red)
        .frame(width: 200, height: 200))
{% endhighlight %}

在默认情况下，背景视图会根据自己的尺寸占用所有空间，也就是可能会溢出父视图的范围，在这种情况下，可以使用 `clipped()` 修改器进行裁剪，使背景不溢出父视图。

{% highlight swift %}
Text("Hello World")
    .font(.largeTitle)
    .background(Circle()
        .fill(Color.red)
        .frame(width: 200, height: 200))
        .clipped()
{% endhighlight %}