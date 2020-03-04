# DOM

## document方法

```
write                   document.write("<h1>xxxxxx</h1>");
                        直接写html文件，如果是在开始载入，则会写在文件头。如果是中间调用则会清空文件

getElementById          通过id获取一个标记对
                        innerHTML 表示标记对在页面显示的内容
                        通过标记对中id属性获取，如<p id="id1"></p>
                        var x = document.getElementById("id1");
                        x.innerHTML = "xfasdfsdf";  //修改标签内的内容//也可以使用id1.innerHTML
                        alert(x.tagName);
                        直接用id也可以代表一个标记对

getElementsByTagName    通过标记对名获取一组标记对
                        获取所有p标签，遍历获得每个的内容
                        var content = document.getElementsByTagName("p");
                        for(i = 0 ; i < content.length; ++i)
                        {
                            alert(content[i].innerHTML);
                        }

createElement           创建标记对
                        根据标签名td创建td标记对元素
                        td_con = document.createElement("td");
```

## 节点方法

```
setAttribute            设置标记对属性值  **.setAttribute("bgcolor","red");

removeAttribute         删除属性  **.removeAttribute("bgcolor");

appendChild             将con加入到**节点下  **.appendChild(con);

insertBefore            在子节点refChil前面插入新节点newElement  **.insertBefore(newElement,refChil);

replaceChild            将子节点odl替换成newElement  **.replaceChild(newElement,old)

removeChild             删除子节点ch  **.removeChild(ch);

cloneNode               复制**节点，包括属性。参数为true会通过递归方法克隆子节点  newNode=  **.cloneNode(true);

hasChildNodes           判断一个元素是否有子节点（标记对中没有任何内容，包括空白）
                        文本节点和属性节点不可能再包含子节点，返回false
                        nodeType
                        1   元素节点类型
                        2   属性节点类型
                        3   文本节点类型
```

## 遍历所有节点

```
var elemList = "";
function getElement(node)
{
    var total = 0;
    if (node.nodeType == 1)  //检查是否是elment对象
    {
        total++;
        elemList =  elemList + node.nodeName + "  ";
    }

    var chil = node.childNodes;
    for(var m = node.firstChild; m != null; m = m.nextSibling)
    {
         total += getElement(m);
    }

    return total;
}

var num = getElement(document);
alert("包含" + num + "个标记" + " " + elemList);
```

## HTML集合

document.anchors  包含所有a元素的组(具有name属性的参数)  
document.forms    包含所有form元素的组  
document.images   包含所有图像元素的组  
document.links    包含所有具有href属性的a元素的值
