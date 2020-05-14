---
layout: default
title: 4.2 创建一个用状态管理的开关
category: swiftui-event
---

SwiftUI 的 `Toggle` 可以让用户在某个状态的 true 跟 false 之间互相切换，类似于 UIKit 的 `UISwitch`。

举个例子，我们先创建一个开关，然后开关为开的时候显示一个文本视图。当然我们不需要手动去监听这个状态的变化，因为在 SwiftUI 里是可以自动的。做法是我们先定义一个 `@State` 的布尔属性，该属性用来记录开关按钮是开还是关，再根据它还显示或隐藏文本视图。代码如下：

{% highlight swift %}
struct ContentView : View {
    @State var showGreeting = true

    var body: some View {
        VStack {
            Toggle(isOn: $showGreeting) {
                Text("Show welcome message")
            }.padding()

            if showGreeting {
                Text("Hello World!")
            }
        }
    }
}
{% endhighlight %}

在上述代码中，只有 `showGreeting` 为 true 的时候才会返回文本视图，当为 false 的时候 `VStack` 会变小，因为里面没有第二个视图了。


