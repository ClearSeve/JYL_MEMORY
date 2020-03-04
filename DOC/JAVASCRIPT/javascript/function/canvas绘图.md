# canvas绘图

```
<canvas id="canv"></canvas>

var canvas = document.getElementById('canv');
var ctx = canvas.getContext("2d");
```

## 铅笔绘图

```
ctx.strokeStyle = "#f00";
ctx.lineWidth = 3;  
function draw(sx, sy, ex, ey) {
    //ctx.clearRect(0, 0, 600, 600);
    //ctx.beginPath();//执行clearRect和beginPath 则重绘
    ctx.moveTo(sx, sy);
    ctx.lineTo(ex, ey);
    ctx.stroke();
}
var oX, oY;
var flag = false;
canvas.onmousemove = function(e) {
    if(!flag) return;
    draw(oX, oY, e.pageX, e.pageY);
    oX = e.pageX;
    oY = e.pageY;
}
canvas.onmousedown = function(e) {
    flag = true;
    oX = e.pageX;
    oY = e.pageY;
}
canvas.onmouseup = function(e) {
    flag = false;
}
onmouseup = function(e)
{
    flag = false;
}
```

## 绘制矩形

```
ctx.fillStyle = "red";
ctx.strokeStyle = "red";
ctx.lineWidth = 5;
ctx.fillRect(0,0, 300, 200);
ctx.strokeRect(0,0,300,200);
```

## 绘制图片

```
var img = new Image();
img.src = "1.jpeg";
img.onload = function()
{
    ctx.drawImage(img,0,0);   //从左上角开始绘制图像  
    ctx.drawImage(img,10,20,150,200);  //从指定坐标（10,20）开始绘制图像，并设置图像宽和高 指定这些参数可以使得图像可以缩放  

    //裁剪一部分图像放在左上角，并调整尺寸
    //ctx.drawImage(img,90,80,100,100,0,0,120,120);
}
```
