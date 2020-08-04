# Menu

const {Menu} = require('electron')

## 隐藏菜单

Menu.setApplicationMenu(null)

## 自定义菜单

```

function Run(){
  console.log("run...")
}


let menuTemplate = [
  {
    label: '数据',
    submenu: [{
      label: '打开',
      accelerator: 'CmdOrCtrl+Z',
      role: 'open'
    }, {
      label: '关闭',
      accelerator: 'Shift+CmdOrCtrl+Z',
      role: 'close'
    }]
  },

  {
    label: '运行',
    submenu: [{
      label: '运行',
      accelerator: 'CmdOrCtrl+R',
      click: Run
    }
    ]
  }
]



const menu = Menu.buildFromTemplate(menuTemplate)
Menu.setApplicationMenu(menu)
```