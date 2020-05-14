---
layout: default
title: 4.4 获取输入框的值并添加边框与占位符
category: swiftui-event
---

SwiftUI 的 `TextField` 跟 `UITextField` 类似，在默认情况下看起来还是有点不同的，并且非常依赖于状态的绑定。


### 创建输入框

举个例子，创建一个输入框和一个文本视图，将输入框的输入值同步显示在文本视图里，这也涉及了状态绑定，如下：

{% highlight swift %}
struct ContentView : View {
    @State var name: String = "Tim"

    var body: some View {
        VStack {
            TextField($name)
            Text("Hello, \(name)!")
        }
    }
}
{% endhighlight %}

运行的时候，就可以达到我们想要的效果了。但是有两个问题，首先，这个输入框没有默认边框，除了默认值几个字看不到其它任何东西，所以只能在大致范围内点击输入。再者，你可能会发现在 Xcode 的预览栏里是无法输入的，那么就按 cmd + R 让它在模拟器里运行。

### 为输入框添加边框

既然 `TextField` 在默认状况下毫无样式，很明显这不符合我们的需求，至少得加个框吧！如果你想使用 `UITextField` 常见的圆角矩形效果，可以使用 `.textFieldStyle（.roundedBorder）` 修改器，如下：

{% highlight swift %}
TextField($yourBindingHere)
    .textFieldStyle(.roundedBorder)
{% endhighlight %}

如果想自定义，使用 `.init()` 修改器。

### 为输入框添加占位符文本 placeholder

占位符文本，当文本输入框值为空时显示在文本输入框的灰色文本，一般使用提示（“输入密码”），或者显示一些示例数据。很简单，只需多传入一个 `placeholder` 参数，如下：

{% highlight swift %}
struct ContentView : View {
    @State var emailAddress = ""

    var body: some View {
        TextField($emailAddress, placeholder: Text("johnnyappleseed@apple.com"))
            .textFieldStyle(.roundedBorder)
            .padding()
    }
}
{% endhighlight %}

输入框为空的情况下会显示，否则隐藏。