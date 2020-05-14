---
layout: default
title: 7.2 从表单视图的设计开始
category: swiftui-forms
---

SwiftUI 的表单视图就像一个容器，和 `HStack` 和 `VStack` 一样，可以根据需要添加其它视图。但是，它会自动调整某些控件的行为和外观样式，以便它可以更好地适应表单环境。

举个例子，我们来创建一个带有切换按钮，分段控件和按钮的表单，如下：

{% highlight swift %}
struct ContentView : View {
    @State var enableLogging = false

    @State var selectedColor = 0
    @State var colors = ["Red", "Green", "Blue"]

    var body: some View {
        NavigationView {
            Form {
                SegmentedControl(selection: $selectedColor) {
                    ForEach(0 ..< colors.count) {
                        Text(self.colors[$0]).tag($0)
                    }
                }

                Toggle(isOn: $enableLogging) {
                    Text("Enable Logging")
                }

                Button(action: {
                // activate theme!
                }) {
                    Text("Save changes")
                }
            }.navigationBarTitle(Text("Settings"))
        }
    }
}
{% endhighlight %}

注意一点，Xcode 11 至少需要 betea 2 (2019-06-18 release) 及以上版本才支持 `Form` 视图容器。

运行该代码时，可以看到两个对表单行为比较重要的事情：

* 在 iOS 平台上，表单视图会自动采用分组列表的的样式，因此用户可以看到这个表单是可以滚动的。

* 按钮视图它是左对齐，文字为蓝色，但是注意一点，整个单元格都是可以点击的。

可以根据需要设定多个单元格，但是如果超过了 10 个，就需要采用分组模式了。


