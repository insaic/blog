---
layout: default
title: 11.8 视图缩放
category: swiftui-transform
---

使用 `scaleEffect()` 修改器可以放大或者缩小视图，比如放大到常规大小的五倍，如下所示：

{% highlight swift %}
Text("Up we go")
    .scaleEffect(5)
{% endhighlight %}

但是要注意，文字放大五倍是会失真的，是位图放大的方式，而不是矢量图。

也可以设置 X 轴和 Y 轴的放大比例，比如 X 轴放大 1 被，Y 轴放大 5 倍，代码如下：

{% highlight swift %}
Text("Up we go")
    .scaleEffect(x: 1, y: 5)
{% endhighlight %}


默认情况下是从视图中心向四周缩放的，当然也可以设置一个锚点作为缩放的中心，如下：

{% highlight swift %}
Text("Up we go")
    .scaleEffect(2, anchor: UnitPoint(x: 1, y: 1))
{% endhighlight %}

这段代码，意思是从右下角作为缩放起点，放大两倍。