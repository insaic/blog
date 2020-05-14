---
layout: default
title: 11.12 给视图添加蒙版（mask）

category: swiftui-transform
---

SwiftUI 提供了一个 `mask()` 修改器，可以用来实现一些蒙版功能，比如在一个视图里，只想展示我们需要的形状。多说无益，看图，有一张图，它的尺寸如蓝框大小写，但是我只想展示 SWIFT！ 这个形状：

![mask](/files/swiftUI/mask.png)

首先我们创建一个 300 x 300 的图片视图，然后使用文本 SWIFT! 对其进行遮照，以便裁剪成字母的形状，如下：

{% highlight swift %}
Image("example-image")
    .resizable()
    .frame(width: 300, height: 300)
    .mask(Text("SWIFT!")
        .font(Font.system(size: 72).weight(.black)))
{% endhighlight %}