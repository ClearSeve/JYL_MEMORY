# js调用

## 写入HTML

```
<script>
document.write("<h1>xxxxxx</h1>");
</script>
包含函数的脚本位于文档的head部分，这样可以确保调用函数前，脚本已经载入。
包含执行内容的脚本一般放在body末尾，有利于载入，防止阻塞造成的页面载入慢。

在xhtml上，<和&被解释为xml，为了避免干扰，在xhtml中使用
<script type="text/javascript">
```

## JS文件载入

```
将脚本保存为.js，可以被多个文件调用
JS文件中：
document.write("<h1>xxxxxx</h1>");
function fun(ss){alert(ss);}

Html文件中：
<script language = "javascript" type ="text/jscript" src = "a.js">
</script>

<button onclick = "fun('xx');">btn</button>
外部有双引号时，内部用单引号


代码导入js文件
newElement = document.createElement("script");
newElement.setAttribute("src","a.js");
newElement.setAttribute("type","text/javascript");
document.body.appendChild(newElement);
```

## 写入浏览器

```
在浏览器的地址栏直接写入代码
javascript:alert("hello");
```


## 事件

### 标签中调用

```
 <script language = "javascript" type ="text/jscript">
function check()
{
    if (f1.user.value == "")
    {
         alert("请输入");
         f1.user.focus();
    }
    else
    {
       f1.submit();
    }
}
</script>


 Name或id属性都可以用.语法访问到
<form name="f1" action="b.html" method="post" target="_blank">
 用户名:<input type="text" name="user" />
 <input type="button"   onclick = "check()" value="按钮名"/>
 </form>
```

### 放入html代码中

```
Js代码加载必须在相应标签加载完后执行

document.getElementById('bt').onclick = function() { alert(1); };
document.getElementById('bt').addEventListener('click', function() { alert(1); }, false);
```
