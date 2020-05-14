---
layout: default
title: 6.6 列表如何开启编辑模式
category: swiftui-lists
---

之前介绍的删除单元格方法，理论上是实时处在编辑模式，毕竟一滑动就是可以删除了。但是还可以进阶一点，可以添加一个编辑按钮，让列表可以进入编辑模式。

举个例子，在 `ContentView` 定义一个用户数组，单元格附加了 `onDelete()` 方法，导航栏上再添加一个编辑按钮：

{% highlight swift %}
struct ContentView : View {
    @State var users = ["Paul", "Taylor", "Adele"]

    var body: some View {
        NavigationView {
            List {
                ForEach(users.identified(by: \.self)) { user in
                    Text(user)
                }
                .onDelete(perform: delete)
            }
            .navigationBarItems(trailing: EditButton())
        }
    }

    func delete(at offsets: IndexSet) {
        if let first = offsets.first {
            users.remove(at: first)
        }
    }
}
{% endhighlight %}

代码运行的时候，可以点击编辑按钮，让列表进入或禁用编辑模式。
