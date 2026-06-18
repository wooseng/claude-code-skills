# 委托设计指南

## 自定义委托
创建自定义委托方法时，未命名的第一个参数应作为委托源，参考示例：
```swift
func namePickerView(_ namePickerView: NamePickerView, didSelectName name: String)
func namePickerViewShouldReload(_ namePickerView: NamePickerView) -> Bool
```