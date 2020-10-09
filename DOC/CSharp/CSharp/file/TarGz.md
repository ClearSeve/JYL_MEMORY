# TarGz

```
using System.IO;
using System.IO.Compression;


//写入压缩流
FileStream fileStream = new FileStream(filename, FileMode.Create, FileAccess.Write);
GZipStream compresssionStream = new GZipStream(fileStream, CompressionMode.Compress);

//写入字符串
StreamWriter writer = new StreamWriter(compresssionStream);
writer.Write(data);
writer.Close();




//读取压缩流
FileStream fileStream = new FileStream(filename, FileMode.Open, FileAccess.Read);
GZipStream compressionstream = new GZipStream(fileStream, CompressionMode.Decompress);

//读取字符串
StreamReader reader = new StreamReader(compressionstream);
string data = reader.ReadToEnd();

//读取字节流
byte[] dat = new byte[512];
while(true)
{
     int len = br.Read(dat, 0, 512);
     if (0 == len) break;
}

```



```
struct tar_header
{
	char name[100];//* 文件/文件夹路径 （文件夹以/结尾）
	char mode[8];//0000777
	char uid[8];//0000000
	char gid[8];//0000000
	char size[12];//* 文件大小【8进制】
	char mtime[12];//修改时间【8进制】
	char chksum[8];//* 除去checksum字段（8字节）其他所有的504个字节的ascii码相加的值再加上256  【8进制】
				   //【前0-148/ 后156-512】  【magic为"ustar "，其他空时，大小591】
	char typeflag;//'0'
	char linkname[100];//""
	char magic[6];//*【ustar 】
	char version[2];//""
	char uname[32];
	char gname[32];
	char devmajor[8];//""
	char devminor[8];//""
	char prefix[155];//""
	char padding[12];//""
};//文件内容以512字节为一个block进行分割，最后一个block不足部分以0补齐
```
