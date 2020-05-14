---
layout: default
title: 4.10 为视图添加单击或双击的手势
category: swiftui-event
---

任何的 SwiftUI 视图都可以添加点击手势，并且可以设定要接收多少次点击才会触发操作。

举个例子，为一个文本视图添加一个点击手势，点完的时候会打印出信息：

{% highlight swift %}
Text("Tap me!")
    .tapAction {
        print("Tapped!")
    }
{% endhighlight %}

那如果要双击手势呢？添加一个 `count` 参数，如下：

{% highlight swift %}
Text("Tap me!")
    .tapAction(count: 2) {
        print("Double tapped!")
    }
{% endhighlight %}