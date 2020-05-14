---
layout: default
title: 8.3 向导航栏插入视图
category: swiftui-containers
---

在常见的应用程序中，导航栏除了标题，一般在左右两侧还会有操作按钮。意思就是可以在导航栏的两侧添加任何视图。 `navigationBarItems()` 修改器可以帮我们实现这一需求。

例如我们在导航栏的后端添加一个 “帮助” 按钮，代码如下：

{% highlight swift %}
var body: some View {
    NavigationView {
        Text("SwiftUI")
            .navigationBarTitle(Text("Welcome"))
            .navigationBarItems(trailing:
                Button(action: {
                    print("Help tapped!")
                }) {
                    Text("Help")
                })
    }
}
{% endhighlight %}