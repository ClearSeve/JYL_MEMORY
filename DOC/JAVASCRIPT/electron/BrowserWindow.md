# BrowserWindow

##  获取窗口对象
```
const {BrowserWindow } = require('electron')

let win

win = new BrowserWindow({ width: 800, height: 600, minWidth: 1280, minHeight: 800, resizable: false, allowRunningInsecureContent: true, experimentalCanvasFeatures: true, icon: './a.ico'})

//隐藏标题栏 
frame: false

//html中<title>决定了窗口标题
```


## 开发者工具

win.webContents.openDevTools()