# SnapKit布局指南

## 基础
在使用`SnapKit`设置约束时遵循以下规则：
- 顶部和底部安全区域应该相对于目标视图的`snp.topMargin`和`snp.bottomMargin`
- 设置约束时不需要显示声明闭包内的参数名，直接使用`$0`
- 左右方向优先采用`leading`和`trailing`
- 如果要设置基于父视图同方向的偏移值，直接采用`equalTo(10)`这种方式，不要使用`equalToSuperview().offset(10)`
- 如果要设置相对指定视图指定距离，采用`offset`而不是`inset`
示例：
```swift
listView.snp.makeConstraints {
    $0.leading.equalTo(10)
    $0.trailing.equalTo(rightView.snp.leading).offset(-10)
    $0.top.equalTo(view.snp.topMargin)
    $0.bottom.equalTo(view.snp.bottomMargin)
}
```