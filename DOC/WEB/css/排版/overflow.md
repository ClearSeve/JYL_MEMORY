# overflow


```
overflow 属性只工作于指定高度的块元素上


visible	默认值。内容不会被修剪，会呈现在元素框之外
hidden  隐藏窗口滚动条,内容会被修剪，并且其余内容是不可见
scroll  内容被修剪，显示滚动条以便查看其余的内容
auto    根据元素尺寸自动增加滚动条
inherit	从父元素继承 overflow 属性



li的3-4会缩放到滚动条
<div style="height: 50px;overflow: auto;">
   <li>1</li>
   <li>2</li>
   <li>3</li>
   <li>4</li>
   <li>5</li>
   <li>6</li>
</div>
<div style="height: 100px;width: 100px;background-color: yellow;">
</div>
```