---
layout: default
title: 11.5 使用 clipShape 裁剪视图
category: swiftui-transform
---

使用 SwiftUI 的 `clipShape()` 修改器，可以裁剪视图，以达到自己想要的形状。 

比如我们先创建一个按钮，然后适当填充并且增加背景颜色，最后裁剪成一个圆形，代码如下：

{% highlight swift %}
Button(action: {
    print("Button tapped")
}) {
    Image(systemName: "bolt.fill")
        .foregroundColor(.white)
        .padding()
        .background(Color.green)
        .clipShape(Circle())
}
{% endhighlight %}

注意下 `Circle()` 这个参数，不管视图原本的形状是否为正方形或者其它举行，它都被裁成一个标准的圆形，但是这个圆形的直径取决于长宽二者的最小值，有导致内容被裁掉的可能。

除了圆形  `Circle()`，还有圆角矩形 `Capsule `，如下：

{% highlight swift %}
Button(action: {
    print("Button tapped")
}) {
    Image(systemName: "bolt.fill")
        .foregroundColor(.white)
        .padding(EdgeInsets(top: 10, leading: 20, bottom: 10, trailing: 20))
        .background(Color.green)
        .clipShape(Capsule())
}
{% endhighlight %}

这样的操作方式，就不会导致内容被裁剪了。