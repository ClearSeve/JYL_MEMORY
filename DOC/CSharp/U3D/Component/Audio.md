# Audio

## Audio listener

```
音频监听器（Audio listener）通常附加在使用的相机上（在加入Audio Source会自动加入到相机）。声音在3D空间内，所以如果包含声音的模型远离相机，声音就会减小。

勾选play on Awake可以在物体出现时自动播放，其他属性都可以在这个Audio Source中设置。
void Awake() {
	audio.volume = 0.2F;
	audio.Play();
}

播放assets中的声音
模型必须有Audio Source
public AudioClip sound=new AudioClip();  //可以在inspector视图中选择声音

audio.PlayOneShot (sound, 1);//1代表音量最大
```
