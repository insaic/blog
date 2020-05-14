---
layout: default
title: 7.4 在表单视图使用 Picker 选择器
category: swiftui-forms
---

SwiftUI 的选择器视图（Picker）会自动在不同的平台进行自适应调整。在 iOS 的表现令人印象深刻，它会把当前值显示在单个表单单元格上面，点击了选择图标，它会导航到一个新的列表视图，该列表包含所有选项，点击单个选项后会自动返回表单视图，然后更新当前值。

多说无益，看例子，用一个数组创建一个选择器，如下：

{% highlight swift %}
struct ContentView : View {
    var strengths = ["Mild", "Medium", "Mature"]

    @State var selectedStrength = 0

    var body: some View {
        NavigationView {
            Form {
                Section {
                    Picker(selection: $selectedStrength, label: Text("Strength")) {
                        ForEach(0 ..< strengths.count) {
                            Text(self.strengths[$0]).tag($0)

                        }
                    }
                }
            }.navigationBarTitle(Text("Select your cheese"))

        }
    }
}
{% endhighlight %}

如果要禁用此交互模式，可以使用 ` .pickerStyle(.wheel) ` 修改器，让其显示为常规模式，如下：

{% highlight swift %}
Picker(selection: $selectedStrength, label: Text("Strength")) {
    ForEach(0 ..< strengths.count) {
        Text(self.strengths[$0]).tag($0)

    }
}.pickerStyle(.wheel)
{% endhighlight %}
