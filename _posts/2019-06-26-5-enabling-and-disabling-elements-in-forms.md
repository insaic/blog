---
layout: default
title: 7.5 启用或禁用表单元素
category: swiftui-forms
---

SwiftUI 可以使用 `disabled()` 修改器来禁用表单视图里的任何元素，这需要由一个布尔值来控制，表单元素的样式会随着其是否禁用而自动改变，例如，禁用状态下的按钮和切换按钮会变成灰色。

这边我们创建一个包含两个分组的表单视图，一个切换按钮，一个普通按钮，但是这个普通按钮在切换按钮没有打开时是禁用的，如下：

{% highlight swift %}
struct ContentView : View {
    @State var agreedToTerms = false

    var body: some View {
        NavigationView {
            Form {
                Section {
                    Toggle(isOn: $agreedToTerms) {
                        Text("Agree to terms and conditions")
                    }
                }

                Section {
                    Button(action: {
                        // show next screen here
                    }) {
                        Text("Continue")
                    }.disabled(!agreedToTerms)
                }
            }.navigationBarTitle(Text("Welcome"))

        }
    }
}
{% endhighlight %}

如代码所示，只需要将 `disabled(!agreedToTerms)` 修改器添加到其按钮代码后面即可。

和其它很多 SwiftUI 修改器一样，`disabled()` 不一定要放在表单元素后面，可以放在 `Form` 里的任何地方，取决于需求。比如可以把 `disabled(!agreedToTerms)` 移到 `Section` 后面，这样可以让整组的元素禁用。如下：

{% highlight swift %}
 Section {
    Button(action: {
        // show next screen here
    }) {
        Text("Continue")
        }
    Button(action: {
        // show next screen here
    }) {
        Text("Continue")
    }
}.disabled(!agreedToTerms)
{% endhighlight %}
