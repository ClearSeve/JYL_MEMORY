# Float

## Float

```
使元素向左或向右移动，其周围的元素也会重新排列
left,right,none,inherit
该属性使块元素变成内联元素

<div>
  <!-- 固定显示在右侧 -->
  <div style="float: right; height:100px;width: 100px;background-color: yellow;"></div>
  <!-- 固定显示在左侧 -->
  <div style="float: left;; height:100px;width: 100px;background-color: red;"></div>
  <!-- 显示在红黄方块之间 -->
  <div style="width: 500px;height: 20px;background-color: blue;">1234567890</div>
  <!-- 显示在红黄方块之间 -->
  <div style="width: 150px;height: 20px;background-color: blueviolet;">12345</div>
  <!-- div宽度不足以显示完文字内容，内容会换下一行 -->
  <div style="width: 150px;height: 20px;background-color: green;">1234567890</div>
</div>
```

## clear

```
元素浮动之后，为避免周围元素重新排列，使用 clear
clear 属性指定元素两侧不能出现浮动元素
left,right,both,none,inherit

<div style="float: left; height:100px;width: 100px;background-color: yellow;"></div>
<div style="float: left; height:100px;width: 100px;background-color: red;"></div>
<div style="float: left; height:100px;width: 100px;background-color: yellow;"></div>
<div style="float: left; height:100px;width: 100px;background-color: red;"></div>
<div style="float: left; height:100px;width: 100px;background-color: yellow;"></div>

<!-- 加入clear属性，可以让该元素分开块浮动区域 -->
<h3 style="clear: both;">中间文本</h3> 

<div style="float: left; height:100px;width: 100px;background-color: red;"></div>
<div style="float: left; height:100px;width: 100px;background-color: yellow;"></div>
<div style="float: left; height:100px;width: 100px;background-color: red;"></div>
<div style="float: left; height:100px;width: 100px;background-color: yellow;"></div>
<div style="float: left; height:100px;width: 100px;background-color: red;"></div>
```