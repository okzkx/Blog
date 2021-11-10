# OnGUI

纯使用代码绘制 GUI，相比 UGUI

好处是 工作量少，移植方便，兼容性好。

缺点是 没法所见即所得，不美观，没法实现复杂的功能。

## 四个绘制控件工具类

这四个类都能绘制控件，绘制的控件类型，环境有所区别。

GUI

GUILayout

EditorGUI

EditorGUILayout

#### 名词解释

Editor

只能运行在编辑器中，不加可运行于任何时

Layout

自定义布局，可以省去参数定位布局位置

### 总结

从上到下限定越来越严格

下部分依赖于上部分实现

所以一般来说 EditorGUILayout 是最好用的，可用区域最小的。

所以绘制工具类选择的优先级为从下到上

## 所有控件

所有控件包括

scrollView

Scrollbar

Slider

window

toggle

button

textArea

textField

label

box

## 绘制样式

获取当前的 GUI 所有控件样式

`GUI.Skin`

也可以获取 Unity 内置的 OnGUI 样式

`EditorStyles`

样式表 `GUIStyle`

每种控件的样式表设置都不一样

可以在绘制控件时单独指定样式表，

也可以统一设置某种控件的样式表。

## 控件内容

#### GUIContent

一个控件内可以有一个控件内容 GUIContent，控件会自动布局 GUIConetent ，可以由三部分组成 text,image,tooltip。

一般在绘制控件的 API 中，可以简化 CUIContent 的实例化，直接传入 text，内部自动将其实例化为 GUIContent

## 布局辅助类

#### GUILayoutOption

让 GUILayout 可以进行一些自定义的布局。

从 GUILayout 设置一些当前控件对布局的影响即可， 获取 GUILayoutOption 即可
