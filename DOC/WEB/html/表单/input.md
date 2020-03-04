# input

+ 按钮  
`<input   type="button"   onclick = "alert('xx');" value="按钮名"/>`

+ 文本输入  
`<input type="text" />`

+ 密码  
`<input type="password" />`

+ 重置form中输入内容  
`<input type="reset" value="重置"/>`

+ 文件选择  
`<input type="file"/>`

+ 单选按钮

```
<input type="radio" name="sex" value="male"/>Male
<input type="radio" name="sex" value="female"/>Female


获取选择内容
var sexval = document.getElementsByName("sex");
 for(var i = 0; i < sexval.length;++i )
 {
         if(sexval[i].checked)
               alert("check  "+ sexval[i].value);
}  
```

+ 组选按钮

```
<input type="checkbox" name="x"/>A
<input type="checkbox" name="x"/>B
```
