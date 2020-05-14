---
layout: default
title: 3.3 使用 ZStack 进行重叠排版
category: swiftui-view-layout
---

SwiftUI 除了横向纵向两种堆栈视图类型，还有一个 Z轴的重叠类型，比如在图片视图上放置一个文本视图，还是挺有用的。这个叫做 `ZStack`，它跟前面介绍的两种堆栈类型工作方式基本相同。

就来举一个在图片上添加文字的例子，代码如下：

{% highlight swift %}
ZStack() {
    Image("example-image")
    Text("Hacking with Swift")
        .font(.largeTitle)
        .background(Color.black)
        .foregroundColor(.white)
}
{% endhighlight %}

跟其它的堆栈类型一样，`ZStack` 可以使用对齐的方式创建，所以也不一定都是要居中。

{% highlight swift %}
ZStack(alignment: .leading) {
    Image("example-image")
    Text("Hacking with Swift")
        .font(.largeTitle)
        .background(Color.black)
        .foregroundColor(.white)
}
{% endhighlight %}

当然了它没有 `spacing` 属性，因为无实际意义。