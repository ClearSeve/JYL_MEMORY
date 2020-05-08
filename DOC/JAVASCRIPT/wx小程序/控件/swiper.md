# swiper

```
<swiper class="tab-area"  vertical="true" autoplay="false" circular="false" interval="2" display-multiple-items='3'>
  <view class="tab-item">
    <block wx:key="idx" wx:for='{{lsItem}}'>
      <swiper-item>
        <view class='content-item'>
          <text class='name'>{{item.nickName}}</text>
          <text class='value'>数据{{item.reward}}</text>
        </view>
      </swiper-item>
    </block>       
  </view>
</swiper>
```

```
.tab-area {
  width: 90%;
  margin: 20rpx auto;
  height: 800rpx;
}
.tab-item {
  padding: 0 20rpx;
  background: #fbeeed;
  height: 100%;
  width: 100%;
  box-sizing: border-box;
}
.content-item {
  height: 100rpx;
  border-bottom: 1px solid #ede1d4;
  width: 95%;
}
.name {
  font-size: 26rpx;
  float: left;
  line-height: 100rpx;
  color: #b0aaa9;
  margin-left: 20rpx;
  width: 40%;
  height:100%;
  overflow: hidden;
}
.value {
  font-size: 26rpx;
  float: right;
  line-height: 100rpx;
  color: #999;
}
```


```
Page({
  data: {
    lsItem: [
      {
        nickName: "xx",
        reward: "2"
      },
      {
        nickName: "xx",
        reward: "2"
      },
      {
        nickName: "xx",
        reward: "2"
      },
      {
        nickName: "xx",
        reward: "2"
      }
    ]
  },
```