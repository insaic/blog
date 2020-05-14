---
layout: default
title: 4.5 使用 SecureField 创建一个密码输入框
category: swiftui-event
---

SwiftUI 的 `SecureField` 跟平常的输入框 `TextField ` 其实没什么两样，区别就是 `SecureField` 输入的字符会变成小圆点，变得不可识别，但这只是针对肉眼，程序里依然是普通字符串。大多的使用场景都是用在密码输入的时候。代码如下：

{% highlight swift %}
struct ContentView : View {
    @State private var password: String = ""

    var body: some View {
        VStack {
            SecureField($password)
            Text("You entered: \(password)")
        }
    }
}
{% endhighlight %}


