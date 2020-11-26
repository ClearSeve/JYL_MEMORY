# linux子系统

## 安装

win+X-》设置-》更新和安全-》开发者选项，启用开发者模式  
控制面板-》（类别）程序-》程序和功能-》启用或关闭windows功能-》适用于Linux的Windows子系统   

```
https://docs.microsoft.com/en-us/windows/wsl/install-manual下载系统
https://wsldownload.azureedge.net/CanonicalGroupLimited.Ubuntu18.04onWindows_1804.2018.817.0_x64__79rhkp1fndgsc.Appx

双击文件进行安装

文件存放位置：
C:\Users\用户名\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu18.04onWindows_79rhkp1fndgsc

安装编译环境：
sudo apt-get install build-essential
sudo apt install gdb

查看所有安装的子系统
wsl --list --all

删除某个子系统
wsl --unregister name
```

## 命令

|cmd               | 说明        |
| :-:              |:-:         |
|bash              |  指令进入系统|
|cd /mnt/d         |  进入d盘     |
