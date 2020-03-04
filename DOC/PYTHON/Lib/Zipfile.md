# Zipfile

import zipfile

## 压缩

```
f = zipfile.ZipFile('E:/test.zip','w',zipfile.ZIP_DEFLATED)
f.write('D:/1.jpg')
f.write('D:/2.jpg')
f.close()


def zip_dir(srcDir,dstPathName):
    z = zipfile.ZipFile(dstPathName, 'w', zipfile.ZIP_DEFLATED)
    for dir_path, dir_names, file_names in os.walk(srcDir):
        f_path = dir_path.replace(srcDir, '')
        f_path = f_path and f_path + os.sep or ''
        for filename in file_names:
            z.write(os.path.join(dir_path, filename), f_path + filename)
    z.close()
```

## 解压

```
f = zipfile.ZipFile('E:/test.zip')
f.extractall() 不传递参数则解压所有文件到当前目录
f.close()

第一个参数为解压后路径，路径必须存在
第二个参数为需要解压的文件名列表（zip.namelist()可以获取）
第三个参数为密码，函数必须传入ascii字符串  
zip.extractall(**r'D:\\1'**,**None**,**'123'**.encode(**'ascii'**))
```
