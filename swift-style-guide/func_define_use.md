# 函数声明与调用

## 函数声明
将简短的函数声明写在同一行，包括左大括号，示例：
```swift
func reticulateSplines(spline: [Double]) -> Bool {
    // reticulate code goes here
}
```

对于参数>=3个或者签名字符长度大于80的函数，将每个参数单独放在一行，后续行需额外缩进，示例：
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

对于适合单行显示的简短函数调用应按如下方式编写：
```swift
let success = reticulateSplines(splines)
```

对于参数>=3个或者签名字符长度大于80的函数，将每个参数单独放在一行，后续行需额外缩进，示例：
```swift
let success = reticulateSplines(
    spline: splines,
    adjustmentFactor: 1.3,
    translateConstant: 2,
    comment: "normalize the display"
)
```