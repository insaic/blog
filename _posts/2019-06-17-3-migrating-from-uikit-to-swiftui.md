---
layout: default
title: 1.3 从 UIKit 迁移到 SwiftUI
category: swiftui-introduction
---

如果你之前使用的是 UIKit，那么你所知的某些类（class）只要删除前面的 `UI` 前缀，就是对应的 SwiftUI 了😂。但这并不表示这二者是完全相同的，只能说二者有相同或者相似的功能。

看下面这个列表，UIKit 类名跟对应的 SwiftUI：

* `UITableView`: `List`
* `UICollectionView`:  没有对应的 SwiftUI
* `UILabel`: `Text`
* `UITextField`: `TextField`
* `UITextField` 与 `isSecureTextEntry` 为 true: `SecureField`
* `UITextView`: 没有对应的 SwiftUI
* `UISwitch`: `Toggle`
* `UISlider`: `Slider`
* `UIButton`: `Button`
* `UINavigationController`: `NavigationView`
* `UIAlertController`（.alert 类型）: `Alert`
* `UIAlertController`（.actionSheet 类型）: `ActionSheet`
* `UIStackView`（横轴 horizontal axis）: `HStack`
* `UIStackView with`（竖轴 vertical axis） : `VStack`
* `UIImageView`: `Image`
* `UISegmentedControl`: `SegmentedControl`
* `UIStepper`: `Stepper`
* `UIDatePicker`: `DatePicker`
* `NSAttributedString`: 与 SwiftUI 不兼容，使用 `Text` 代替

还有许多其它 SwiftUI 独有的组件，例如堆栈视图（stack view）。


