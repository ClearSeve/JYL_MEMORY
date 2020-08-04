# weui

## 组件引入
下载地址：  
https://github.com/Tencent/weui-wxss/tree/master/dist/style  

在app.wxss中导入（weui.wxss放在同级目录）

```
@import 'weui.wxss';

page{
    background-color: #EDEDED;
    font-size: 16px;
    font-family: -apple-system-font,Helvetica Neue,Helvetica,sans-serif;
}
.page__hd {
    padding: 40px;
}
.page__bd {
    padding-bottom: 40px;
}
.page__bd_spacing {
    padding-left: 15px;
    padding-right: 15px;
}

.page__ft{
    text-align: center;
    padding:0 0 10px;
    padding:0 0 calc(10px + constant(safe-area-inset-bottom));
    padding:0 0 calc(10px + env(safe-area-inset-bottom));
}


.page__title {
    text-align: left;
    font-size: 20px;
    font-weight: 400;
}

.page__desc {
    margin-top: 5px;
    color: #888888;
    text-align: left;
    font-size: 14px;
}
.weui-cell_example:before{
    left:52px;
}
```

## 组件调用

根组件使用class="page"  
页面骨架组件使用class="page__xxx"

```
<view class="page">

<view class="page__hd"><!--页头--></view>

<view class="page__bd"><!--主体--></view>

</view>
```


其他组件都已weui-开头后接组件名称,组件和子组件使用两个下划线衔接

```
<view class="weui-footer">
<view class="weui-footer__links">
    <navigator url="" class="weui-footer__link">***</navigator>
</view>
<view class="weui-footer__text">Copyright © **</view>
</view>
```

## 属性

weui-flex方向：  
style="flex-direction:column;"
style="flex-direction:row;"
