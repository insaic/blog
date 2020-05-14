---
layout: default
title: 4.8 创建一个日期选择器
category: swiftui-event
---

SwiftUI 的 `DatePicker` 跟 `UIDatePicker` 类似，有多种选项可以设置外观样式和工作方式，跟其它取值控件一样，它也需要绑定一个状态。

举个例子，我们创建一个日期选择器，并且把它的值显示到一个视图文本上，代码如下：

{% highlight swift %}
struct ContentView : View {
    var dateFormatter: DateFormatter {
        let formatter = DateFormatter()
        formatter.dateStyle = .long
        return formatter
    }

    @State var birthDate = Date()

    var body: some View {
        VStack {
            DatePicker(
                $birthDate,
                maximumDate: Date(),
                displayedComponents: .date
            )

            Text("Date is \(birthDate, formatter: dateFormatter)")
        }
    }
}
{% endhighlight %}

可以在上述代码看到 `displayedComponents` 这个属性被设置成 `.data`，这是日期选择，如果要时间选择，可以把这个属性设置成 `.hourAndMinute`。同时也设置了 `maximumDate` 属性，做一个最大日期的限制，当然也可以设置成 `minimumDate` 来限制最小日期。


