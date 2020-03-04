# FileInfo

FileInfo fin = new FileInfo(Path);

## 文件是否存在

fi.Exists

## 创建文件

FileStream fs = fi.Create();

## 时间获取

string modiytime = fin.LastWriteTime.ToLongDateString() + " " + fin.LastWriteTime.ToLongTimeString();

## 删除文件

fin.Delete();

## 获取文件流

FileStream fs =  fi.OpenRead()
