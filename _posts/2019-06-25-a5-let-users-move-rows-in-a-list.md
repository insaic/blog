---
layout: default
title: 6.5 列表如何支持单元格拖动排序
category: swiftui-lists
---

SwiftUI 也同样给了开发者一个简单的钩子可以让用户移动单元格，在 WWDC 中演示的某些功能在当前的测试版本中还无法使用，因此我们需要一个解决方法。

首先我们在单元格的代码中附加一个 `onMove(perform:)` 修改器，当移动操作生效的时候，可以调用方法，该方法需要 `IndexSet` 类型的 source 参数和 `Int` 类型的 destination 参数，如下：

{% highlight swift %}
func move(from source: IndexSet, to destination: Int) {
{% endhighlight %}

当移动多个单元格时，最好先移动后面的单元格，这样可以避免移动其它单元格而引起索引混乱。

举个例子，我们在 `ContentView` 创建三个用户名的单元格，并在 `move()` 调用的时候更新列表数组，为了启用移动模式，在右上角添加了一个编辑按钮，点击之后可以进入移动模式，代码如下：

{% highlight swift %}
struct ContentView : View {
    @State var users = ["Paul", "Taylor", "Adele"]

    var body: some View {
        NavigationView {
            List {
                ForEach(users.identified(by: \.self)) { user in
                    Text(user)
                }
                .onMove(perform: move)
            }
            .navigationBarItems(trailing: EditButton())
        }
    }

    func move(from source: IndexSet, to destination: Int) {
        // sort the indexes low to high
        let reversedSource = source.sorted()

        // then loop from the back to avoid reordering problems
        for index in reversedSource.reversed() { 
            // for each item, remove it and insert it at the destination
            users.insert(users.remove(at: index), at: destination)
        }
    }
}
{% endhighlight %}

在 WWDC 的演示中，他们使用 `move()` 这个方法，所以一行代码就够了，但是目前 Swift 还不支持该方法，所以只能希望它尽快实现了。