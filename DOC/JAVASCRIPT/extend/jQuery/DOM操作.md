# DOM操作

## 设置获取

```
text() - 设置或返回所选元素的文本内容
html() - 设置或返回所选元素的内容（包括 HTML 标记）
val() - 设置或返回表单字段的值
attr("attrname", 'val')  属性值设置

$('#lb').text("abc");
$("input[name=name属性对应的值]").val(["value属性对应值"]);
 $("#imgid").attr("src", url); //设置img的url属性
```

## 添加

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

## 删除

```
$("div").remove();    删除元素及子元素
$("p").remove(".aa");   选择p的class为aa的元素进行删除

$("div").empty();   删除子元素
```

## radiobutton

```
var val = $('#radioGroupDiv   input[name="radioItemClass"]:checked').val();
选择id为radioGroupDiv 范围内的class为radioItemClass的value属性
```