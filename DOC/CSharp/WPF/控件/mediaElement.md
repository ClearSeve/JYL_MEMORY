# mediaElement

```
<MediaElement Name="mediaElement1"  Stretch="Fill" LoadedBehavior="Manual" />
```

删除标签中的margin，可以实现填满整个程序  
Stretch="Uniform"可以让mediaElement按比例拉伸

## 打开文件

this.mediaElement1.Source = new Uri("d:\\1.avi");
//可以用File.Exists("d:\\1.avi");验证地址合法性

## 播放

this.mediaElement1.Play();

## 暂停

this.mediaElement1.Pause();

## 停止事件

属性：MediaEnded="mediaElement1_MediaEnded"

## 音量

this.mediaElement1.Volume += 10;

## 媒体长度

TimeSpan span = this.mediaElement1.NaturalDuration.TimeSpan;  
string s = string.Format("{0},{1}", span.Minutes, span.Seconds);

## 当前位置

TimeSpan span = this.mediaElement1.Position;

## 快进

mediaElement1.Position = mediaElement1.Position + TimeSpan.FromSeconds(10);

## 设置播放位置

TimeSpan span = new TimeSpan(0, 0, 3);//设置为第3秒  
this.mediaElement1.Position = span;
