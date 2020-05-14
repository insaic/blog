---
layout: default
title: 9.4 弹出一个 Action Sheet 对话框
category: swiftui-alert
---

跟 Alert 对话框不一样，Action Sheet 是从屏幕底部弹出来的，可以翻译成动作菜单/动作面板/行动列表。除了标题和内容，也允许有多种选项。

SwiftUI 为我们提供了一个 `ActionSheet` 视图来创建 Action Sheet 对话框。但是有一点要注意的是，如果你选择取消了对话框，必须把相关的状态重新设置成 false，否则无法再次弹出。

首先，我们需要定义一个属性来确定是否显示 Action Sheet：

{% highlight swift %}
@State var showingSheet = false
{% endhighlight %}

接着，需要创建属性来储存 Action Sheet 对话框，包括标题，正文，一组对话按钮。重要一点还是当 Action Sheet 取消的时候，需要重置 `showingSheet` 这个属性，视图代码如下：

{% highlight swift %}
var sheet: ActionSheet {
    ActionSheet(title: Text("Action"), message: Text("Quote mark"), buttons: [.default(Text("Show Sheet"), onTrigger: {
        self.showingSheet = false
    })])
}
{% endhighlight %}

最后，需要根据 `showingSheet` 属性来确定是否显示 Action Sheet 对话框，一个三元表达式，如下：

{% highlight swift %}
.presentation(showingSheet ? sheet : nil)
{% endhighlight %}

将上面代码全面整合如下：

{% highlight swift %}
struct ContentView : View {
    @State var showingSheet = false

    var sheet: ActionSheet {
        ActionSheet(title: Text("Action"), message: Text("Quote mark"), buttons: [.default(Text("Woo"), onTrigger: {
            self.showingSheet = false
        })])
    }

    var body: some View {
        Button(action: {
            self.showingSheet = true
        }) {
            Text("Woo")
        }
            .presentation(showingSheet ? sheet : nil)
    }
}
{% endhighlight %}