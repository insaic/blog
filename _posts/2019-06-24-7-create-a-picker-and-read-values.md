---
layout: default
title: 4.7 创建选择器（Picker）并取值
category: swiftui-event
---

SwiftUI 的 `Picker ` 设法将 `UIPicker` 和 `UITableView` 结合在了一起，同时也适应其它的操作系统（Apple 系）样式，最棒的是我们不必关系它是如何工作的，SwiftUI 可以很好地自动适应环境。

和其它大多数控件一样，也必须把选择器绑定到某个状态，以便监听选择器变化。例如，这边有一个储存着颜色名称的字符串数组，然后跟选择器结合在一起，以便让用户选择颜色：

{% highlight swift %}
struct ContentView : View {
   var colors = ["Red", "Green", "Blue", "Tartan"]
   @State private var selectedColor = 0

   var body: some View {
      VStack {
         Picker(selection: $selectedColor, label: Text("Please choose a color")) {
            ForEach(0 ..< colors.count) {
               Text(self.colors[$0]).tag($0)
            }
         }
         Text("You selected: \(colors[selectedColor])")
      }
   }
}
{% endhighlight %}


