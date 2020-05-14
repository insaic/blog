---
layout: default
title: 2.1 SwiftUI 的基础项目模版
category: swiftui-text-images
---

首先我们先在 Xcode 创建一个 SwiftUI 项目，步骤如下：

步骤一：打开 Xcode（必须11及以上版本） 并点击 **Create a new Xcode project**，或者在菜单栏选择 **File > New > Project**。

![Create a new Xcode project](/files/swiftUI/1.jpg)

步骤二：在模版选择器选择 **Single View App**，然后点击下一步。

![Single View App](/files/swiftUI/2.jpg)

步骤三：给项目命名，然后勾选 **Use SwiftUI** 选项，最后找个路径保存项目。

![Use SwiftUI](/files/swiftUI/3.jpg)

至此一个项目就算有基础模版了，也可以看到对应的文件结构。

![SwiftUI](/files/swiftUI/4.jpg)

下面简单介绍下各个文件的作用：

* 1. AppDelegate.swift 文件名是“应用代理”，主要是负责监听程序的生命周期，比如启动，闲置，进入后台，返回前台，退出应用程序，或者有其它应用程序试图调用本程序等动作。
* 2. SceneDelegate.swift 文件名是“屏幕代理”，主要是负责应用程序的视图显示方式。
* 3. ContentView.swift 这是应用程序加载完成的初始化界面，如果是一个 UIKit 项目，Xcode 则是给一个 ViewController.swift 的文件。
* 4. Assets.xcassets 这是一个静态资源目录，用于放置项目中需要使用到的图片，颜色，数据等资源。
* 5. LaunchScreen.storyboard 打开程序，加载时所显示的内容，国产的应用程序通常用来放置广告。
* 6. Info.plist 一个属性列表文件，比如设置应用的显示名称，需要获取的系统权限（定位，通讯录，摄像机...）。
* 7. Preview Content 包含一个 Preview Assets.xcassets 的资源目录。

SwiftUI 项目的基础结构差不多就这样，文件不多，代码也少。我们要把重点放在 ContentView.swift 上面，因为这是应用程序的主要内容，也是我们接下去要开始尝试各种 SwiftUI 代码的地方。

首先，为什么 ContentView.swift 的内容可以显示在屏幕上？

看上面的文件列表里 SceneDelegate.swift 的描述，我们打开该文件，可以看到如下代码：

{% highlight swift %}
let window = UIWindow(frame: UIScreen.main.bounds)
window.rootViewController = UIHostingController(rootView: ContentView())
self.window = window
window.makeKeyAndVisible()
{% endhighlight %}

这段代码定义里一个叫做 `ContentView` 的实例，并且把它放置在 `window` 里，以便可以在屏幕上显示。也可以理解成应用程序加载完成后就进入了 `ContentView`。

打开 ContentView.swift 文件，可以看到如下代码：

{% highlight swift %}
import SwiftUI

struct ContentView : View {
    var body: some View {
        Text("Hello World")
    }
}

#if DEBUG
struct ContentView_Previews : PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
#endif
{% endhighlight %}

代码不多，首先注意 `ContentView` 是一个结构体（struct），如果是熟悉 UIKit 的开发者对 struct 并不陌生，我们可以为结构体定义属性（常量、变量）和添加方法，从而扩展结构体的功能。

再者， `ContentView` 符合 `View` 的协议（protocol），在 SwiftUI 展示的所有内容都需遵循 `View` 协议。这实际上只意味着一件事：你需要一个名为 `body` 的属性来返回某种View。

这个 `body` 返回的是 `some View`。`some` 这个关键字是 Swift 5.1 的新特性<a href="https://github.com/apple/swift-evolution/blob/master/proposals/0244-opaque-result-types.md" target="_blank">Opaque Result Types</a>。默认场景下 View 协议是没有具体类型信息的，只有类型约束，但是用 some 修饰后，编译器会让 protocol 的实例类型对外透明，。在上述代码中，some 的意思是：它会返回某种 View（Text, Image...），符合 `View` 的协议内容，但是 SwiftUI 不需要知道具体返回的是什么，当然不能返回空，也不能随机返回（上述的代码是有写清楚返回类型）。

最后，在 `ContentView` 下面有一个类似但又不同的结构体 `ContentView_Previews`，它并不符合 `View` 协议，因为它是专门用来在 Xcode 里显示预览视图用的，而不会出现在真实的应用屏幕上。这个结构体被 `#if DEBUG` 和 `#endif` 包围着，意思是这里面的代码只会在调试环境中运行，在成品生产环境中无实际意义。



