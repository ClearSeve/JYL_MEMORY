# BrowserWindow

##  获取窗口对象
```
const {BrowserWindow } = require('electron')

let win

win = new BrowserWindow({ width: 800, height: 600, minWidth: 1280, minHeight: 800, resizable: false, allowRunningInsecureContent: true, experimentalCanvasFeatures: true, icon: './a.ico'})

//隐藏标题栏 
frame: false
//禁止用户缩放窗口  
resizable: false

//启用跨域和nodejs
win = new BrowserWindow({ width: 800, height: 600 ,webPreferences: {
    nodeIntegration:true,
    webSecurity: false
  }})

//html中<title>决定了窗口标题

//关闭窗口
win.close();
```

## 打开页面

```
//打开本地的页面
win.loadFile('index.html')

//打开网页
win.loadURL('http://jiyanglin.xyz/')
```

## 开发者工具

win.webContents.openDevTools()