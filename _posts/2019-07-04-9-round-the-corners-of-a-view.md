---
layout: default
title: 11.9 如何将视图设成圆角
category: swiftui-transform
---

之前说过，通过 `clipShape()` 修改器的裁剪方式可以让视图达到圆角效果，这有点相当于间接方式。这次需要讲的是 `cornerRadius()` 修改器，它会直接将视图设成圆角，直接传入一个值就可以，适用于任何视图。

比如创建一个 25 点圆角的文本视图，如下：

{% highlight swift %}
Text("Round Me")
    .padding()
    .background(Color.red)
    .cornerRadius(25)
{% endhighlight %}