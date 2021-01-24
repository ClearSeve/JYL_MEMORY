# https

```
开发-开发管理-开发设置-服务器域名
设置后台服务器地址。

function ShowTips(inf) {
  wx.showToast({
    title: inf,
    icon: 'none',
    duration: 2000
  });
}



//jsonparamdata:
//{
//  username: "abc",
//  password: "bbb",
//}
RequestData: function (addr, jsonparamdata, funSuc){
  if (this.requesting){
    ShowTips('正在获取数据，请稍后...')
    return
  }
  
  this.requesting = true
  var _this = this
  wx.request({
    url: addr,
    method: 'post',
    data: jsonparamdata,
    header: {
      'content-type': 'application/json'
    },
    success(res) {
      _this.requesting = false
      if (res.statusCode == 200) {
        funSuc(res.data)
      }
      else {
        ShowTips(res.data)
      }    
    },
    fail() {
      _this.requesting = false
      ShowTips('发送请求失败')
    },
  })
},  
```