# echarts配置

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="echarts.min.js">
    </script>
</head>

<body>
    <div id="area" style="width: 600px;height: 500px;"></div>
    <script>
        //绑定元素
        var dom = document.getElementById('area');
        var chart = echarts.init(dom,'dark');//默认主题light

        //chart.showLoading(); 
        //chart.hideLoading();

        //图形设置
        var option = {}
      
        //绘图
        chart.setOption(option);


    </script>
</body>

</html>
```

```
//自动宽度
window.onresize = function(){
    chart.resize();
}
```