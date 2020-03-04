# DirectoryInfo

string dir = @"D:\";  
@可以将\转换为\\

DirectoryInfo TheFolder = new DirectoryInfo(dir);  

属性  
TheFolder.Name  
TheFolder.FullName

```
//遍历dir中所有目录     不包含点目录
foreach (DirectoryInfo NextFolder in TheFolder.GetDirectories())

///遍历dir中所有文件
foreach (FileInfo NextFile in TheFolder.GetFiles())
///遍历文件夹下所有jpg文件
foreach (FileInfo NextFile in TheFolder.GetFiles("*.jpg"))

//遍历目录及子目录下所有txt
void AddPath(string path, List<string> PathNameList)
{
      DirectoryInfo TheFolder = new DirectoryInfo(path);
      foreach (FileInfo NextFile in TheFolder.GetFiles("*.txt"))
      {
            PathNameList.Add(NextFile.FullName);
      }
      foreach (DirectoryInfo NextFolder in TheFolder.GetDirectories())
      {
           AddPath(path + "\\" + NextFolder.Name, PathNameList);
      }
}
```
