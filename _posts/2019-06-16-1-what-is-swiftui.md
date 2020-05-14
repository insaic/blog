---
layout: default
title: 1.1 什么是 SwiftUI ?
category: swiftui-introduction
---

在 iOS 13 之前，使用的是命令式语法来定义 UI，也就是 `UIKit`。SwiftUI 是一个可以让开发者使用声明式语法（Declarative Syntax）来定义 UI 的工具，开发者可以简单地定义用户界面该长得怎么样及相应的逻辑关系。

跟声明式语法对应的是命令式语法（Imperative Syntax），那这二者有什么区别？简单来讲命令式的意思是，开发者需要一步步来写清楚程序需要如何做什么（How to do What），不仅要关注结果，还有过程。声明式的意思是，开发者不需要一步步告诉程序如何做，只需要告诉程序在哪些地方做什么（Where to do What），只需注结果，无需要知道过程。跟命令式比起来，声明式通常处理一些总结性、总览性的工作，不适合做顺序相关的细节相关的底层工作。

举例一个场景：一个输入框（textField），一个视图（UILabel），输入框输入的值同步显示在视图上。使用命令式的话，先需要精确定义两个 UI，然后要写一个函数，在这个函数里，我们需要先读取输入框的值，然后把这个值手动显示到视图上，最后给输入框加一个监听，回调这个函数。也就是我们需要根据输入框的变动情况来改动视图。而声明式则不需要，只需声明视图的值等于输入框的值，至于如何实现不需关心。来看下代码：

### 命令式语法 UIKit

{% highlight swift %}
import UIKit

class ViewController: UIViewController {
    
  let label: UILabel = {
    let label = UILabel()
    label.frame = CGRect(x: 0, y: 0, width: 300, height: 21)
    label.center = CGPoint(x: 160, y: 285)
    return label
  }()
  
  let textField: UITextField = {
    let textField = UITextField()
    textField.frame = CGRect(x: 20, y: 100, width: 300, height: 40)
    textField.addTarget(self, action: #selector(textFieldDidChange(_:)), for: .editingChanged)
    return textField
  }()

  override func viewDidLoad() {
    super.viewDidLoad()
    self.view.addSubview(textField)
    self.view.addSubview(label)
  }
  
  @objc func textFieldDidChange(_ textField: UITextField) {
    label.text = textField.text
  }

}
{% endhighlight %}


### 声明式语法 SwiftUI

{% highlight swift %}
import SwiftUI

struct ContentView : View {
    
  @State var name: String = ""
    
  var body: some View {
    VStack {
      TextField($name)
      Text(name)
    }
  }

}
{% endhighlight %}

命令式 UI 是需要围绕着状态（state）的改变来改变视图，这里的状态可以理解成 “在代码中储存的值/变量”，这会导致很多问题。假如我们有一个视图，它的显示内容是根据一个布尔值来决定的，那么只有两种状态，布尔值的 `true` 跟 `false`，如果我们有两个布尔值 A 和 B，那么则会有四种状态：

A: false, B: false<br>
A: true,  B: false<br>
A: false, B: true<br>
A: true,  B: true

那如果有三个布尔值呢？四个呢？或者其它整数，字符串，日期..... 围绕着这些状态做逻辑判断，只会导致情况会越来越复杂。相比之下，声明式可以让我们立即告诉应用程序所有可能的状态，需要遵循的所有规则，还有确保 SwiftUI 可以强制执行这些规则。

但是 SwiftUI 的有点不只有这些，它还可以跨平台使用，目前可以在iOS，macOS，tvOS 甚至 watchOS 上运行。这意味着你现在只需学习一种语言和一种布局框架，然后在任何地方部署你的代码（不同平台代码可能略有差别）。