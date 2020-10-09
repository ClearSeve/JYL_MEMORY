# FileStream

使用try判断文件是否打开或创建成功

## 读取

```
FileStream file = new FileStream("e:\\1.txt", FileMode.OpenOrCreate);

byte[] data = new byte[10];
file.Read(data, 0, 10);
string s = Encoding.UTF8.GetString(data);
```

## 写入

```
FileStream file = new FileStream("e:\\1.txt", FileMode.OpenOrCreate);

string s = "abcdefg";
byte[] data =Encoding.UTF8.GetBytes(s);

file.Write(data, 0, data.Length);
```

## 文件指针移动
```
file.Seek(2, SeekOrigin.Begin);  //文件指针从头向后移动两个
file.Seek(-2, SeekOrigin.End);从末尾向前移动两个位置
```
