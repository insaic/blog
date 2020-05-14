---
layout: default
title: 4.6 创建滑块输入条（Slider）并取值
category: swiftui-event
---

SwiftUI 的 `Slider` 工作方式跟 `UISlider` 一样，也是绑定到某个状态，以便可以储存当前值。

当你创建它的时候，最需要关心的参数是：

* **value**：用一个 `Double` 值去绑定。
* **from & through**：也就是滑块最左及最右的取值范围。
* **by**：单次滑动可增加或减少的值。

举个例子，下列代码创建了一个输入滑块，并且绑定到了 `celsius` 属性，然后会在滑块移动的时候更新文本视图，以便同时显示摄氏度（Celsius）和华摄氏度（Fahrenheit）：

{% highlight swift %}
struct ContentView : View {
    @State var celsius: Double = 0

    var body: some View {
        VStack {
            Slider(value: $celsius, from: -100, through: 100, by: 0.1)
            Text("\(celsius) Celsius is \(celsius * 9 / 5 + 32) Fahrenheit")
        }
    }
}
{% endhighlight %}


