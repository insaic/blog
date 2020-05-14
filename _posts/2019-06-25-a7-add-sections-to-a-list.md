---
layout: default
title: 6.7 如何向列表视图添加分组（Section）
category: swiftui-lists
---

SwiftUI 内置的列表视图支持分组（Section）和分组页眉（header），类似于 UIKit 的 `UITableView`。其实实现起来也很简单。

举个例子，下面是一个叫做任务的单元格视图：

{% highlight swift %}
struct TaskRow: View {
    var body: some View {
        Text("Task data goes here")
    }
}
{% endhighlight %}

我们要创建一个包含两个分组的列表视图，一个是重要任务，一个是不怎么重要的任务，下面是代码：

{% highlight swift %}


< How to enable editing on a list using EditButton	 	How to make a grouped list >
How to add sections to a list
SwiftUI’s list view has built-in support for sections and section headers, just like UITableView in UIKit. To add a section around some cells, start by placing a Section around it, optionally also adding a header and footer.

As an example, here’s a row that holds task data for a reminders app:

struct TaskRow: View {
    var body: some View {
        Text("Task data goes here")
    }
}
What we want to do is create a list view that has two sections: one for important tasks and one for less important tasks. Here’s how that looks:

struct ContentView : View {
    var body: some View {
        List {
            Section(header: Text("Important tasks")) {
                TaskRow()
                TaskRow()
                TaskRow()
            }

            Section(header: Text("Other tasks")) {
                TaskRow()
                TaskRow()
                TaskRow()
            }
        }
    }
}
{% endhighlight %}

如果要添加页脚的方式如下：

{% highlight swift %}
Section(header: Text("Other tasks"), footer: Text("End")) {
    TaskRow()
    TaskRow()
    TaskRow()
}
{% endhighlight %}

