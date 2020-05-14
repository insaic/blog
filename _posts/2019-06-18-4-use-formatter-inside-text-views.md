---
layout: default
title: 2.4 如何在文本视图中使用 Formatter
category: swiftui-text-images
---


在程序里某些值更新的时候，可能需要额外的附加方法格式化一下，才能达到我们想要的结果。SwiftUI 的文本视图有一个可选的 `formatter`参数，可以让我们自定义数据在标签内的显示格式。

举个例子，定义一个日期变量，然后显示的时候，必须显示为肉眼可读，如下：

{% highlight swift %}
struct ContentView: View {
    static let taskDateFormat: DateFormatter = {
        let formatter = DateFormatter()
        formatter.dateStyle = .long
        return formatter
    }()

    var dueDate = Date()

    var body: some View {
        Text("Task due date: \(dueDate, formatter: Self.taskDateFormat)")
    }
}
{% endhighlight %}


显示输出：Task due date: June 25 2019

<!--
那如何自定义一个呢？看代码：

{% highlight swift %}

{% endhighlight %}
-->
