---
layout: default
title: 6.8 如何把列表视图分组设置成 grouped 模式
category: swiftui-lists
---

跟 `UITableView` 一样，SwiftUI 的分组也支持 grouped 样式，默认是的 `plain` 样式 。如果要改成 grouped 样式只需在列表添加一个 `.listStyle(.grouped)` 修改器即可。

代码如下：

{% highlight swift %}
struct ExampleRow: View {
    var body: some View {
        Text("Example Row")
    }
}

struct ContentView : View {
    var body: some View {
        List {
            Section(header: Text("Examples")) {
                ExampleRow()
                ExampleRow()
                ExampleRow()
            }
        }.listStyle(.grouped)
    }
}
{% endhighlight %}

