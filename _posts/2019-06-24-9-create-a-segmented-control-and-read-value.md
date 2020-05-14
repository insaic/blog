---
layout: default
title: 4.9 创建分段控件（SegmentedControl）并取值
category: swiftui-event
---

SwiftUI 的分段控制器/分段控件（SegmentedControl）和 `UISegmentedControl` 的工作方式类似，但它需要绑定一个状态，并且确保每个段（segment）有个标记，以便识别。段（segment）可以是文本或图片视图，其它类型的会被忽略掉。

例如，我们创建一个分段控件，使用 `favoriteColor` 作为属性，并在下面添加一个文本视图，显示所选的值：

{% highlight swift %}
struct ContentView : View {
    @State private var favoriteColor = 0

    var body: some View {
        VStack {
            SegmentedControl(selection: $favoriteColor) {
                Text("Red").tag(0)
                Text("Green").tag(1)
                Text("Blue").tag(2)
            }
            Text("Value: \(favoriteColor)")
        }
    }
}
{% endhighlight %}

在这个例子里，更好的写法是创建一个数组来储存颜色，然后使用 `ForEach` 来循环创建文本视图。

{% highlight swift %}
struct ContentView : View {
    @State private var favoriteColor = 0
    var colors = ["Red", "Green", "Blue"]

    var body: some View {
        VStack {
            SegmentedControl(selection: $favoriteColor) {
                ForEach(0..<colors.count) { index in
                    Text(self.colors[index]).tag(index)
                }
            }

            Text("Value: \(colors[favoriteColor])")
        }
    }
}
{% endhighlight %}