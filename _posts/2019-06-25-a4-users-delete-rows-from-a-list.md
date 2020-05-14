---
layout: default
title: 6.4 让用户可以手动删除单元格
category: swiftui-lists
---

这里的删除操作，类似于微信的删除，向左滑动单元格会出现删除按钮。

SwiftUI 实现起来同样很简单，使用 `onDelete(perform:)` 处理方法，该方法需要一个 `IndexSet` 类型的索引集合，该集合包含要删除的多个索引，如下所示：

{% highlight swift %}
func delete(at offsets: IndexSet) { }
{% endhighlight %}

你可以循环这个索引集合，如果你只想删除一个，只需读取该集合的第一个元素。

注意：Apple 在 WWDC 演示的时候使用了一个名为 `remove(atOffsets:) ` 的数组方法来完成删除，但该方法在 Swift 中并不存在，希望以后的版本会支持该方法。

举个例子，有个三个单元格的列表，然后添加了 `onDelete(perform:) ` 修改器用来支持删除：

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
        }
    }

    func delete(at offsets: IndexSet) {
        if let first = offsets.first {
            users.remove(at: first)
        }
    }
}
{% endhighlight %}

运行代码时，滑动单元格，可以看到删除操作。