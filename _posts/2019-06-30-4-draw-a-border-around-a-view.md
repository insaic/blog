---
layout: default
title: 11.4 给视图绘制边框
category: swiftui-transform
---

对于视图的边框，SwiftUI 也专门提供了一个 `border()` 修改器来绘制边框。可以用来设置边框的颜色，宽度，圆角等。

下面举个例子，在一个视图周围绘上一个宽度为 1 的黑色边框：如下：

{% highlight swift %}
Text("Hacking with Swift")
    .border(Color.black)
{% endhighlight %}

默认的边框宽度是 1，但是正常情况下视图尺寸的大小就是文字内容的大小，所以还需要添加一点填充：

{% highlight swift %}
Text("Hacking with Swift")
    .padding()
    .border(Color.black)
{% endhighlight %}

再设置一个宽度，变个颜色：

{% highlight swift %}
Text("Hacking with Swift")
    .padding()
    .border(Color.red, width: 4)
{% endhighlight %}

再添加个圆角：

{% highlight swift %}
Text("Hacking with Swift")
    .padding()
    .border(Color.red, width: 4, cornerRadius: 16)
{% endhighlight %}
