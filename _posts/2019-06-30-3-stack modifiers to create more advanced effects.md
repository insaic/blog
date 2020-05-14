---
layout: default
title: 11.3 使用多个修改器创建更高级的效果
category: swiftui-transform
---

视图可以添加多个修改器，同一个修改器也可以多次重复添加。

例如我们可以在文本的周围添加填充（padding）和背景颜色，然后同样的操作再来两次，修改一下颜色，以达到自己想要的效果。如下：

{% highlight swift %}
Text("Forecast: Sun")
    .font(.largeTitle)
    .foregroundColor(.white)
    .padding()
    .background(Color.red)
    .padding()
    .background(Color.orange)
    .padding()
    .background(Color.yellow)
{% endhighlight %}
