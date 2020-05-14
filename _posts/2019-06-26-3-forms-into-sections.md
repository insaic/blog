---
layout: default
title: 7.3 给表单视图进行分组
category: swiftui-forms
---

SwiftUI 的表单通常需要分组（Section），就如 iOS 里的设置一样，网络设置一组，通知设置一组….. 

`Form` 的分组跟 `List` 的分组有点不同，可以在两个地方重复使用相同的代码，可以给分组添加页眉和页脚，每个分组之间有个较大的距离间隙。

举个例子，我们创建一个两个分组的表单视图，第一组是一个分段控件和一个切换按钮，第二组是一个保存按钮，如下：

{% highlight swift %}
struct ContentView : View {
    @State var enableLogging = false

    @State var selectedColor = 0
    @State var colors = ["Red", "Green", "Blue"]

    var body: some View {
        NavigationView {
            Form {
                Section(footer: Text("Note: Enabling logging may slow down the app")) {
                    SegmentedControl(selection: $selectedColor) {
                        ForEach(0 ..< colors.count) {
                            Text(self.colors[$0]).tag($0)
                        }
                    }

                    Toggle(isOn: $enableLogging) {
                        Text("Enable Logging")
                    }
                }

                Section {
                    Button(action: {
                    // activate theme!
                    }) {
                        Text("Save changes")
                    }
                }
            }.navigationBarTitle(Text("Settings"))
        }
    }
}
{% endhighlight %}
