---
layout: default
title: 9.3 为 Alert 对话框添加确认选项
category: swiftui-alert
---

重温一下上一章讲的对话框代码：

{% highlight swift %}
Alert(title: Text("Important message"), message: Text("Wear sunscreen"), dismissButton: .default(Text("Got it!")))
{% endhighlight %}

它定义了一个标题和提示信息，跟 `UIAlertController` 差不多，然后还有一个默认样式一个文本为 “Got it!” 的确认（或关闭）按钮。

上面只是显示对话框的主要代码，但是还得定义一个条件来触发该对话框。条件满足的时候，即弹出。比如我们定一个叫做 `showingAlert` 的布尔值，当该值为 true 的时候即弹出对话框，如下：

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