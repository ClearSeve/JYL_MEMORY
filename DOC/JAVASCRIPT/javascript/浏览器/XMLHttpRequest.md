# XMLHttpRequest


## 同步读取本地文件

```
var xmlhttp = new window.XMLHttpRequest();
xmlhttp.open("GET","aaa.txt",false); 
xmlhttp.send(null); 
console.log(xmlhttp.responseText);
```


## 异步读取本地文件
```
var xmlhttp = new window.XMLHttpRequest();
xmlhttp.onreadystatechange=()=>{
  if (xmlhttp.readyState==4)
  {
    if (xmlhttp.status==200)
    {
      console.log(xmlhttp.responseText);
    }
  } 
};
xmlhttp.open("GET","aaa.txt",true); 
xmlhttp.send(null);   
```