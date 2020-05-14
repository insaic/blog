---
layout: default
title: 2.3 如何使用设置文本视图的样式
category: swiftui-text-images
---

上一节只讲了文本的行数控制，本章节讲下文本的颜色，字体，行间距等样式设置。

默认情况下 `Text` 视图的样式是 *正文*（类似于 word 里的正文），但是可以通过 `.font()` 来改变字体大小和粗细，如下所示：

{% highlight swift %}
Text("This is an extremely long string that will never fit even the widest of Phones")
    .lineLimit(nil)
    .font(.largeTitle)
{% endhighlight %}

在这段代码我们可以看到 `Text` 下有两个修改器（modifier），换行对齐，这样的写法无误，主要是为了方便阅读。我们也可以使用 `.foregroundColor()` 修改器来设置文本颜色，如下：

{% highlight swift %}
Text("The best laid plans")
    .foregroundColor(Color.red)
{% endhighlight %}

你也可以设置背景颜色，使用  `.background()` 修改器，不是只能设置纯颜色，也可以设置渐变之类的，下面的代码是设置一个纯的黄色，如下：

{% highlight swift %}
Text("The best laid plans")
    .background(Color.yellow)
    .foregroundColor(Color.red)
{% endhighlight %}

还有其它更多的修改器，比如可以调整文本间距，默认值是 0 ，修改器 `. lineSpacing()` 使用方法如下：

{% highlight swift %}
Text("This is an extremely long string that will never fit even the widest of Phones")
    .lineLimit(nil)
    .font(.largeTitle)
    .lineSpacing(50)
{% endhighlight %}


更多修改器请参考 Apple 开发文档：<a href="https://developer.apple.com/documentation/swiftui/text" target="_blank">https://developer.apple.com/documentation/swiftui/text</a>
