---
name: swift-style-guide
description: Swift代码风格指南，当用户在需要审查Swift代码风格或者编辑.swift文件时使用此技能，助力写出清晰、简洁、优雅的Swift代码
user-invocable: false
license: MIT
---

# Swift代码风格指南
- 在设计API时参考[API 设计指南](api_design_fuidelines.md)
- 使用美式英语拼写以匹配 Apple 的 API
- 使用扩展将代码组织成逻辑功能块。每个扩展都应通过`// MARK: -`注释来分隔，以保持良好的结构。
- 静态方法和类型属性的使用方式与全局函数和全局变量类似，应谨慎使用
- 优先使用简洁的代码风格，让编译器为单个实例的常量或变量推断类型，必要时，指定特定类型，如 `CGFloat` 或 `Int16`
- 对对象生命周期使用延迟初始化
- 对于空数组和字典，使用类型注解
- 优先使用类型声明的简写形式，而非完整的泛型语法
- 自由函数（不依附于类或类型的函数）应谨慎使用，优先使用方法。
- 不应创建引用循环，使用值类型（`struct`、`enum`）来彻底杜绝循环，或者使用`weak`和`unowned`来避免强循环
- 使用`[weak self]`和`guard let self else { return }`来延长对象的生命周期。在无法立即确定`self`的生命周期长于闭包时，优先选择`[weak self]`而非`[unowned self]`。显式延长生命周期的方式优于可选链写法。
- 优先使用`private`而非`fileprivate`；仅当编译器要求时才使用`fileprivate`。仅在需要完整的访问控制规范时，才明确使用 `open`、`public` 和 `internal`。
- 将访问控制用作主要的属性说明符。在访问控制之前唯一应该出现的是`static`说明符或`@IBAction`、`@IBOutlet`以及`@discardableResult`这类属性。
- 优先使用 `for-in` 风格的 `for` 循环，而非 `while-condition-increment` 风格。
- 三元运算符 `?:` 仅应在其能提升代码清晰度或整洁性时使用
- 不需要在代码中的每条语句后加分号
- 条件语句周围的括号应当省略
- 在编写长字符串字面量时，使用多行字符串字面量语法
- 不要在项目中使用表情符号
- 不使用 `#imageLiteral` 或 `#colorLiteral`
- 在设计代理、委托时参考[委托设计指南](delegate.md)

## 泛型
泛型类型参数应使用描述性的大驼峰命名法命名。当类型名称没有实际意义或特定作用时，可使用传统的单个大写字母，如`T`、`U`或`V`，参考示例：
```swift
struct Stack<Element> { ... }
func write<Target: OutputStream>(to target: inout Target)
func swap<T>(_ a: inout T, _ b: inout T)
```

## 协议一致性
在为模型添加协议一致性时，优先为协议方法单独创建一个扩展。这样能将相关方法与协议组织在一起，也能简化“为类及其关联方法添加协议”的操作说明，参考示例：
```swift
class MyViewController: UIViewController {
    // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewController: UITableViewDataSource {
    // table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewController: UIScrollViewDelegate {
    // scroll view delegate methods
}
```
由于编译器不允许在派生类中重新声明协议遵循，因此并非总是需要复制基类的扩展组。如果派生类是最终类且仅重写少量方法，这种情况尤其如此。是否保留扩展组由作者自行决定。
对于 UIKit 视图控制器，建议将生命周期方法、自定义存取方法和 IBAction 分组到单独的类扩展中。

## 未使用的代码

应删除未使用的（无效的）代码，包括 Xcode 模板代码和占位符注释。但教程或书籍明确要求用户使用注释代码的情况除外。
与教程无直接关联且实现中仅调用父类的抽象方法也应予以移除。这包括所有空的/未使用的 UIApplicationDelegate 方法。

## 最小导入

只导入源文件所需的模块。例如，当导入 `UIKit` 就足够时，不要导入 `Foundation`。同样，如果你必须导入 `UIKit`，就不要导入 `Foundation`

## 间距
- 使用4个空格而非制表符进行缩进，以节省空间并帮助避免换行
- 方法的花括号以及其他花括号（if/else/switch/while 等）始终与语句位于同一行开头，而在新的一行结尾。
- 方法之间应保留一个空行，类型声明之间最多保留一个空行，以提升视觉清晰度和代码组织性。方法内部的空白应用于分隔不同功能模块，但如果一个方法中包含过多功能区块，通常意味着应将其重构为多个方法。
- 左大括号之后和右大括号之前不应有空白行。
- 右括号不应单独出现在一行中。
- 冒号左侧始终不加空格，右侧加一个空格。例外情况包括三元运算符`? :`、空字典`[:]`以及`#selector`语法`addTarget(_:action:)`。
- 长代码行应在约70个字符处换行。
- 避免行尾出现尾随空格。
- 在每个文件末尾添加一个换行符。

## 注释
- 使用详细的注释来解释类、函数、属性、或者某段代码的具体作用和行为，在修改代码时必须保持最新注释。
- 避免在代码行内使用块注释，因为代码应尽可能具备自文档化特性。例外：这不适用于用于生成文档的注释。
- 避免使用 C 语言风格的注释`（/* ... */）`。优先使用双斜杠或三斜杠注释。

## 类与结构
- 记住，结构体具有值语义。将结构体用于没有标识的事物。
- 类具有引用语义。应将类用于那些拥有标识或特定生命周期的事物。
- 有些本应是结构体的内容需要遵循 `AnyObject` 协议，或者历史上已经被建模为类（例如 `NSDate`、`NSSet`）。请尽量严格遵循这些指导原则。

## Self 的使用
- 为简洁起见，避免使用 `self`
- 仅在编译器要求时使用 `self`

## 计算属性
- 如果某个计算属性是只读的，则省略 get 子句

## 函数声明
将简短的函数声明写在同一行，包括左大括号，示例：
```swift
func reticulateSplines(spline: [Double]) -> Bool {
    // reticulate code goes here
}
```

对于签名较长的函数，将每个参数单独放在一行，后续行需额外缩进，示例：
```swift
func reticulateSplines(
    spline: [Double], 
    adjustmentFactor: Double,
    translateConstant: Int, 
    comment: String
) -> Bool {
}
```

不要使用 `(Void)` 表示输入为空；直接使用`()`。闭包和函数输出请使用 `Void` 而非 `()`。

## 函数调用

在调用点处镜像函数声明的风格。适合单行显示的调用应按如下方式编写：
```swift
let success = reticulateSplines(splines)
```

如果调用点必须换行，则将每个参数单独放在一行，并额外缩进一级：
```swift
let success = reticulateSplines(
    spline: splines,
    adjustmentFactor: 1.3,
    translateConstant: 2,
    comment: "normalize the display"
)
```

## 闭包表达式
- 仅当参数列表末尾仅有一个闭包表达式参数时，才使用尾随闭包语法。为闭包参数指定具有描述性的名称。
- 对于上下文清晰的单表达式闭包，使用隐式返回
- 使用尾随闭包的链式方法在语境中应清晰易读。关于空格、换行符的使用，以及何时使用命名参数与匿名参数，自行决定。

## 类型
- 在可用时始终使用 Swift 的原生类型和表达式。Swift 提供了与 Objective-C 的桥接功能，因此你仍可根据需要使用全部方法。
- 在绘制代码中，如果能通过避免过多转换让代码更简洁，请使用 `CGFloat`

## 常量
- 常量使用 `let` 关键字定义，变量使用 `var` 关键字定义。如果变量的值不会发生变化，请始终使用 `let` 而非 `var`
- 使用类型属性在类型上而非该类型的实例上定义常量。要将类型属性声明为常量，只需使用`static let`

## 可选类型
- 在允许使用 `?` 值的地方，使用 `?` 将变量和函数返回类型声明为可选类型
- 仅对那些你知道会在使用前完成初始化的实例变量（例如将在 `viewDidLoad()` 中设置的子视图）使用用 `!` 声明的隐式解包类型。在大多数其他情况下，优先使用可选绑定而非隐式解包可选类型。
- 访问可选值时，如果该值仅被访问一次，或链式调用中存在多个可选值，请使用可选链
- 当只需解包一次并执行多项操作会更便捷时，请使用可选绑定
- 为可选变量和属性命名时，避免使用`optionalString`或`maybeView`这类命名方式
- 对于可选绑定，应尽可能遮蔽原始名称，而不是使用 `unwrappedView` 或 `actualLabel` 这类名称

## 黄金路径
- 编写带条件判断的代码时，代码的左侧边距应遵循“黄金路径”或“快乐路径”原则。也就是说，不要嵌套 `if` 语句。多个返回语句是可行的，优先选择 `guard`语句。
- 当使用 `guard` 或 `if let` 解包多个可选值时，应尽可能使用复合版本以减少嵌套。在复合版本中，将 `guard` 单独放在一行，然后将每个条件缩进至各自的行。`else` 子句的缩进应与 `guard` 本身一致，如下所示。示例：
  ```swift
  guard 
    let number1 = number1,
    let number2 = number2,
    let number3 = number3 
  else {
    fatalError("impossible")
  }
  ```
- 守卫语句必须以某种方式退出，避免使用大型代码块。如果多个退出点需要清理代码，可考虑使用 defer 块来避免清理代码重复。

## 视图属性定义
在类中定义视图属性时，采用懒加载和匿名函数的方式来实现，匿名函数中生成的用于返回的视图实例使用`tmp`，所有属性定义在匿名函数中实现，示例：
```swift
private lazy var loginButton: UIButton = {
    let tmp = UIButton()
    tmp.setTitleColor(.black, for: .normal)
    tmp.setTitleColor(.white, for: .selected)
    tmp.backgroundColor = .white
    tmp.titleLabel?.font = UIFont.systemFont(ofSize: 15, weight: .semibold)
    tmp.isSelected = true
    tmp.layer.cornerRadius = 4
    tmp.isUserInteractionEnabled = false
    return tmp
}()
```

## 布局
- 优先采用`UIStackView`来实现，尤其是存在需要动态显示隐藏视图的时候，以便直接通过`isHidden`属性来控制，不需要修改约束；
- 项目中需要设置约束时，如果引入了`SnapKit`库，则优先采用`SnapKit`来设置约束，并参考[SnapKit布局指南](snapkit_layout.md)