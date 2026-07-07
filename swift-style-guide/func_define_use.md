# 函数声明与调用

## 基础
- 参数个数>=5或者函数签名字符长度>80的函数是复杂函数，其余的是简单函数；

## 函数声明
简单函数写在同一行，包括左大括号，示例：
```swift
func reticulateSplines(spline: [Double]) -> Bool {
    // reticulate code goes here
}
```

复杂函数需要将每个参数单独放在一行，后续行需额外缩进，示例：
```swift
func reticulateSplines(
    spline: [Double], 
    adjustmentFactor: Double,
    translateConstant: Int, 
    comment: String,
    status: Int
) -> Bool {
}
```

不要使用 `(Void)` 表示输入为空；直接使用`()`。闭包和函数输出请使用 `Void` 而非 `()`。

## 函数调用
简单函数应该写在同一行，示例：
```swift
let success = reticulateSplines(splines)
```

复杂函数需要将每个参数单独放在一行，后续行需额外缩进，示例：
```swift
let success = reticulateSplines(
    spline: splines,
    adjustmentFactor: 1.3,
    translateConstant: 2,
    comment: "normalize the display",
    status: 1
)
```