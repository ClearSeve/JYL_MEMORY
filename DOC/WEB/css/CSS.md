# CSS

## 选择器

+ class选择器

```
class在同一页面可以重复使用
可以同时引用多个如class="c1 c2"

.a{color:Red;}
.b{color:Blue;}
两个ｐ使用不同样式
<p class="a">xxxx</p>
<p class="b">yyyy</p>
```

+ 多级选择

```
对于div1的区域中，选择p标签进行设置
.div1 p{}

只选择div1区域中一级元素p，不选择div1区域中多级元素下的p元素
.div1 >p{}



对于div标签，选择content_1进行设置
div .content_1{}
```

+ 多个选择
```
html,body{height:100%;}
```

+ id选择器

```
id在同一页面不能重复使用
#first{color:Red;}
#second{color:Blue}

id不能使用数字
<p id="first">xxxx</p>
```

## 链接式

```
<link rel="stylesheet" type="text/css" href="a.css" />
```

```css
 h1,h2
{
   color:Blue;
   font-size:small;
}
p{
   color:Red;
}
```

## 行内样式

```
直接在标记对加属性

<p style="font-family:宋体; color:Red;font-size:30px;text-align:center">红色宋体30px居中显示</p>
```

## 内嵌式

```
style标记对中定义
定义ｐ标记对内容颜色为红色
<style>
   p{color:Red;}
</style>
```

## 注释

```
/* xxx */
```