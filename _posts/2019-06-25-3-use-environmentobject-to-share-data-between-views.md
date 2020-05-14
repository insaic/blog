---
layout: default
title: 5.3 使用 @EnvironmentObject 在各视图之间共享数据
category: swiftui-advanced-state
---

在应用程序中，数据全局共享是一个很常见的场景，比如登陆用户的数据，可能会很多地方需要访问到。于是 SwiftUI 为我们提供了 `@EnvironmentObject`，可以在任何视图访问到它共享的数据，并且可以在这些数据更新的时候，视图也会同步更新。

相比于在多个视图之间使用 `@ObjectBinding`，全局的 `@EnvironmentObject` 显得更智能。而不是在视图 A 创建数据，然后传给视图 B ，再传给视图 C，最后再 视图 D 使用它，😂😂。如果使用了环境对象，你可以在某个视图创建数据，并把该数据放到全局环境里。

环境对象必须由根视图提供，如果 SwiftUI 无法找到正确类型的环境对象，会导致程序崩溃，也会导致 Xcode 的预览崩溃。

这是一个存储用户设置的可绑定对象：

{% highlight swift %}
class UserSettings: BindableObject {
    var didChange = PassthroughSubject<Void, Never>()

    var score = 0 {
        didSet {
            didChange.send(())
        }
    }
}
{% endhighlight %}

它只存了一个值，但是没关系，重要的是当这个值变更的时候，`PassthroughSubject` 会告诉所有订阅它的视图更新。因为这个用户设置是需要在程序的任何位置被共享的，所以当我们程序首次启动的时候，我们将要创建一个 `UserSettings` 的实例。

如果你打开 SceneDelegate.swift 这个文件，会在 `scene(_:willConnectTo:options:)` 这个方法里看到下面这两行代码：

{% highlight swift %}
let window = UIWindow(frame: UIScreen.main.bounds)
window.rootViewController = UIHostingController(rootView: ContentView())
{% endhighlight %}

注意下第二行代码，这段的作用是用于创建初始内容视图并将其呈现在屏幕上，这也是我们需要传入环境对象的地方，以便 SwiftUI 可以在 `ContentView` 中使用它们，也可以在任何其它视图使用它们。

首先在 `SceneDelegate` 添加一个属性：

{% highlight swift %}
var settings = UserSettings()
{% endhighlight %}

然后将 `settings` 属性传给 `ContentView`:

{% highlight swift %}
window.rootViewController = UIHostingController(rootView: ContentView().environmentObject(settings))
{% endhighlight %}

完成后，共享的 `UserSettings` 实例就可以在各视图之间使用。需要做的就是使用 `@EnvironmentObject` 属性包装器创建一个属性，如下：

{% highlight swift %}
@EnvironmentObject var settings: UserSettings
{% endhighlight %}

这不需要使用默认值进行初始化，因为它会自动从全局环境中读取。所以我们可以使用 `ContentView` 视图来增加 `score` 设置，并且在 `DetailView` 显示分数，但是这些不需要创建或传递 `UserSettings` 实例，它都在环境里。如下：

{% highlight swift %}
struct ContentView : View {
    @EnvironmentObject var settings: UserSettings

    var body: some View {
        NavigationView {
            VStack {
                // A button that writes to the environment settings
                Button(action: {
                    self.settings.score += 1
                }) {
                    Text("Increase Score")
                }

                NavigationButton(destination: DetailView()) {
                    Text("Show Detail View")
                }
            }
        }
    }
}

struct DetailView: View {
    @EnvironmentObject var settings: UserSettings

    var body: some View {
        // A text view that reads from the environment settings
        Text("Score: \(settings.score)")
    }
}
{% endhighlight %}

如上代码所示，我们不需要将两个视图中的 `UserSettings` 相关联，SwiftUI 会自动运用 `UserSettings` 的实例。
