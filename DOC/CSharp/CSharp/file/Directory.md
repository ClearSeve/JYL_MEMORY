# Directory

静态类

## 目录是否存在

Directory.Exists(Path);

## 删除目录

如果使用第二个参数，则只删除非空目录
Directory.Delete(Path,true);

## 创建目录

可以创建多层次目录
Directory.CreateDirectory(newName);
