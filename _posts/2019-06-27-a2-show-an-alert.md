---
layout: default
title: 9.2 弹出一个 Alert 对话框
category: swiftui-alert
---

在 SwiftUI 创建一个 Alert 对话框的代码如下：

{% highlight swift %}
Alert(title: Text("Important message"), message: Text("Wear sunscreen"), dismissButton: .default(Text("Got it!")))
{% endhighlight %}

但是这段代码只有一个取消（dismissButton）的选项，那如何添加一个确认选项，也就是变成一个确认框而不是选择框。看代码如下：

{% highlight swift %}
struct ContentView : View {
    @State var showingAlert = false

    var body: some View {
        Button(action: {
            self.showingAlert = true
        }) {
            Text("Show Alert")
        }
            .presentation($showingAlert) {
                Alert(title: Text("Are you sure you want to delete this?"), message: Text("There is no undo"), primaryButton: .destructive(Text("Delete")) {
                        print("Deleting...")
                }, secondaryButton: .cancel())
            }
    }
}
{% endhighlight %}