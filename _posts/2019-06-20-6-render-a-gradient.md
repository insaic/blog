---
layout: default
title: 2.6 绘制渐变和简单图形
category: swiftui-text-images
---

SwiftUI为我们提供了各种渐变选项，举个例子，可以建一个由白色到黑色的线性渐变背景的文本视图，如下所示：

{% highlight swift %}
Text("Hello World")
    .padding()
    .foregroundColor(.white)
    .background(LinearGradient(gradient: Gradient(colors: [.white, .black]), startPoint: .top, endPoint: .bottom), cornerRadius: 0)
{% endhighlight %}

这个渐变颜色被放在一个数组里 `[.white, .black]`，这意味着可以在这个数组里放入多种颜色，默认情况下 SwiftUI 会平均分配，所以可以先从白色渐变红色再渐变黑色， `[.white, .red, .black]`。

另外参数有，`startPoint` 表示渐变的起点，`endPoint` 表示渐变终点，如果是水平渐变则使用 `.leading` 和 `.trailing`。如下：

{% highlight swift %}
Text("Hello World")
    .padding()
    .foregroundColor(.white)
    .background(LinearGradient(gradient: Gradient(colors: [.white, .red, .black]), startPoint: .leading, endPoint: .trailing), cornerRadius: 0)
{% endhighlight %}

详细文档，<a href="https://developer.apple.com/documentation/swiftui/gradient" target="_blank">SwiftUI Gradient</a>。

如果想在应用程序里使用一些简单图形，可以直接创建，然后根据需要进行着色和定位。例如创建一个 200 x 200 的红色矩形，如下：

{% highlight swift %}
Rectangle()
    .fill(Color.red)
    .frame(width: 200, height: 200)
{% endhighlight %}

相同的，如果需要创建一个 50 x 50 的圆形，则如下：

{% highlight swift %}
Circle()
    .fill(Color.blue)
    .frame(width: 50, height: 50)
{% endhighlight %}

详细文档，<a href="https://developer.apple.com/documentation/swiftui/drawing_and_animation" target="_blank">Drawing and Animation</a>。

