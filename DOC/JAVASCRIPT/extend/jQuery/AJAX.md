# AJAX

## get

```
$.get(URL,callback);

$.get("url", function(data, status) {
    
});
```


## post

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

## comm

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