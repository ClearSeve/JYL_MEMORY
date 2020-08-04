# components

## 创建页面

```
components/myview目录下

=====index.wxml
<view>
  <text>aaa</text>
</view>


=====index.js
Component({
  properties: {},
  data: {},
  methods: {}
})


=====index.json
{
  "component": true,
  "usingComponents": {}
}
```

## 使用

```
=====页面的json文件中添加相对引用路径
{
  "usingComponents": {
    "myview": "../components/myview/index"
  }
}


=====页面的xml中使用组件
<myview></myview>

```