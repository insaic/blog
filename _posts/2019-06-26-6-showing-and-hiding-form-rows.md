---
layout: default
title: 7.6 显示或隐藏表单元素
category: swiftui-forms
---

SwiftUI 可以让我们在表单里显示或隐藏某些表单元素，这还是挺常见的场景，比如 A 选项选择了某个值，那么就会出现 B 选项。

我们举个例子，首先有一个切换按钮它叫 “更多选项”，如果这个按钮处于开启状态，那么会再出现一个 “启用日志” 的按钮，如果 “更多选项” 不开启则这个 “启用日志” 会隐藏， 代码如下：

{% highlight swift %}
struct ContentView : View {
    @State var showingAdvancedOptions = false
    @State var enableLogging = false

    var body: some View {
        Form {
            Section {
                Toggle(isOn: $showingAdvancedOptions) {
                    Text("Show advanced options")
                }

                if showingAdvancedOptions {
                    Toggle(isOn: $enableLogging) {
                        Text("Enable logging")
                    }
                }
            }
        }
    }
}
{% endhighlight %}

同其它的绑定方式一样，可以 SwiftUI 在隐藏显示的切换过程中使用动画，如下： 

{% highlight swift %}
Toggle(isOn: $showingAdvancedOptions.animation()) {
    Text("Show advanced options")
}
{% endhighlight %}

但是目前的测试版，貌似存在一个 bug，如果把表单视图放在导航视图（NavigationView）里，这个动画会不起作用，以后应该会修正。