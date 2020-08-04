# jQuery

## 基础语法

```
$(selector).action()
$("p").hide() - 隐藏所有段落
```

## 元素选择器

```
$("p") 选取 <p> 元素
$("p.intro") 选取所有 class="intro" 的 <p> 元素
$("p#demo") 选取所有 id="demo" 的 <p> 元素
$("p[xx='#']") 选取所有 xx="#" 的 <p> 元素
```

## 属性选择器

```
$("[xx]") 选取所有带有 xx属性的元素
$("[xx='#']") 选取所有带有 xx值等于 "#" 的元素
$("[xx!='#']") 选取所有带有 xx值不等于 "#" 的元素
$("[xx$='.jpg']") 选取所有 xx值以 ".jpg" 结尾的元素
```

## CSS选择器

```
$("p").css("background-color","red");
所有 p 元素的背景颜色更改为红色
```

## 初始化调用

```
$(document).ready(
function(){...} 或  fun()
);
文档加载完成时进行调用,该函数可以写多个
```

## 按钮绑定函数

```
$("#btn1").click(function(){alert("xx");});  -设置按钮响应函数
只能使用以下方法绑定：
$(document).ready(
 function(){
     $("button").click(function(){
          btnFun();
          return false;//拦截原有的操作
     });
 }
);
```

## AJAX

+ get

```
$.get(URL,callback);

$.get("a.jsp", function(data, status) {
    console.log("Data: " + data + "\nStatus: " + status);
});
```
+ post

```
$.post(URL,data,callback);

$.post("a.jsp", {
valA: "val1",
valB: "val2"
},
function(data, status) {
    console.log("Data: " + data + "\nStatus: " + status);
});
```

+ comm

```
$.ajax({  key:val,key2:val2,...   });

$.ajax({
        type: "POST",
        url: "......",
        data:{valA: "val1",valB: "val2"},
        dataType: "json",
        success: function(data){}
        error:function(errorData){}
//返回数据不为dataType: "json"时调用error方法
      });
```

## 名称冲突

```
var jq=jQuery.noConflict();
使用自己的名称（比如 jq）来代替 $ 符号
```

## 隐藏显示

```
$("p").hide();    隐藏所有p标签
$("p").show();   显示所有p标签
```

## 淡入淡出

```
参数可以是"slow"、"fast" 或毫秒
$("p").fadeOut(1000);   所有p标签1s内逐渐消失
$("p").fadeIn(1000);    所有p标签1s内逐渐出现

$("p").toggle(1000);   如果处于隐藏则淡入，如果显示状态则淡出
$("p").fadeTo(1000,0.5);   变成0.5的透明度
```

## 滑动

```
参数可以是"slow"、"fast" 或毫秒
$("p").slideUp();        向上滑动（消失）
$("p").slideDown();    向下滑动（出现）

$("p").slideToggle();   自动根据状态切换
```

## 动画

```
第一个参数是css效果，第二个参数是变化时间

var v=$("p");
v.animate({height:'30px',opacity:'0.4'},"slow");

停止动画效果
$(selector).stop(stopAll,goToEnd);
可选的 stopAll 参数规定是否应该清除动画队列。默认是 false，即仅停止活动的动画，允许任何排入队列的动画向后执行。
可选的 goToEnd 参数规定是否立即完成当前动画。默认是 false。
因此，默认地，stop() 会清除在被选元素上指定的当前动画。
```

## 连续操作

$("p").css("color","red").slideUp(2000).slideDown(2000);

## HTML元素操作

+ 设置获取

```
text() - 设置或返回所选元素的文本内容
html() - 设置或返回所选元素的内容（包括 HTML 标记）
val() - 设置或返回表单字段的值
```

+ 添加

```
$("p").append("   XXX");         结尾加入文字
$("p").prepend("   XXX");        前方加入文字

after() 和 before() 会加入换行


var txt1="<p>Text.</p>";               // 以 HTML 创建新元素
var txt2=$("<p></p>").text("Text.");   // 以 jQuery 创建新元素
var txt3=document.createElement("p");  // 以 DOM 创建新元素
txt3.innerHTML="Text.";
$("p").append(txt1,txt2,txt3);         // 追加新元素
```

+ 删除

```
$("div").remove();    删除元素及子元素
$("p").remove(".aa");   选择p的class为aa的元素进行删除

$("div").empty();   删除子元素
```

## radiobutton

+ 选择值

```
var val = $('#radioGroupDiv   input[name="radioItemClass"]:checked').val();
选择id为radioGroupDiv 范围内的class为radioItemClass的value属性
```

## 设置值

`$("input[name=name属性对应的值]").val(["value属性对应值"]);`

## CSS

```
<style>
.ss{color:blue;}
</style>

$("p").addClass("ss");  //添加样式

$("p").css("color","red");//设置颜色

removeClass() - 从被选元素删除一个或多个类
toggleClass() - 对被选元素进行添加/删除类的切换操作
css() - 设置或返回样式属性
```
