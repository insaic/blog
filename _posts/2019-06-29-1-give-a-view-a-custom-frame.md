---
layout: default
title: 11.1 如何自定义视图的 frame
category: swiftui-transform
---

在 SwiftUI 里默认情况下，一下视图的大小，只取决于它内容的多少。但是如果你要自定义一个视图的尺寸，可以使用 `frame()` 修改器来自定义尺寸。

举个例子，创建一个可点击面积为 200 x 200 的按钮。如下：

{% highlight swift %}
Button(action: {
    print("Button tapped")
}) {
    Text("Welcome")
        .frame(minWidth: 0, maxWidth: 200, minHeight: 0, maxHeight: 200)
        .font(.largeTitle)
	    .background(Color.red)
}
{% endhighlight %}

或者可以定义一个 frame 来占满整个屏幕，把宽高的最小值定为 0，并且把最大值定为无穷大，如下：

{% highlight swift %}
Text("Please log in")
.frame(minWidth: 0, maxWidth: .infinity, minHeight: 0, maxHeight: .infinity)
    .font(.largeTitle)
    .foregroundColor(.white)
    .background(Color.red)
{% endhighlight %}
