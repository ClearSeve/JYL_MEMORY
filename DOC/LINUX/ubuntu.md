# ubuntu

## 内核版本

查看所有内核  
dpkg --get-selections | grep linux-image  

查看当前使用内核  
uname -a  

删除内核  
sudo apt-get remove linux-image-5.0.0.25-generic  
或者：
sudo dpkg --purge linux-image-5.0.0.25-generic  

固定内核  
sudo apt-mark hold linux-image-4.18.0-15-generic
sudo apt-mark unhold linux-image-4.18.0-15-generic

## sudo权限

sudo gedit /etc/sudoers  

```
# User privilege specification  
jyl  ALL=(ALL:ALL) NOPASSWD:ALL
```

## 用户管理  

添加用户  
sudo adduser ***  

删除用户  
sudo userdel dockers  
查看是否已经没有了被删除的用户  
cat /etc/passwd  

## 双系统安装

+ 将.iso中的initrd.lz、vmlinuz.efi 解压出来与iso 一同放在C盘 根目录。

+ 添加新条目-》NeoGrub -》安装，编写

```
# NeoSmart NeoGrub Bootloader Configuration File  
#  
# This is the NeoGrub configuration file, and should be located at C:\NST\menu.lst  
# Please see the EasyBCD Documentation for information on how to create/modify entries:  
# http://neosmart.net/wiki/display/EBCD/  
  
title Install Ubuntu
root (hd0,0)
kernel (hd0,0)/vmlinuz.efi boot=casper iso-scan/filename=/ios名   ro quiet splash locale=zh_CN.UTF-8
initrd (hd0,0)/initrd.lz  
```

+ 在EasyBCD 编辑引导菜单可看到NeoGrub引导加载器，记得勾选等待用户选择，保存设置  

+ 安装界面系统中用命令卸载盘符，sudo umount -l /isodevice

+ 分配空间
  
|   区域       |  类型               |  
|   :-:       |  :-:                |
| 交换空间      | 逻辑分区 （RAM大小） |
| /           |  逻辑分区            |  
| /home       |  逻辑分区            |
| /boot       |  主分区              |

+ 选择/boot 所在分区进行安装

## 软件包管理

Ubuntu Software Center

## 更新软件

sudo apt-get update

## 浏览器

sudo apt-get update  

sudo apt-get install google-chrome-stable  

/usr/bin/google-chrome-stable  

## 环境变量

vi   .bashrc  
在末尾加入一行    export  PATH=$PATH:.  
source  .bashrc    当前窗口生效，或者重启生效  
加入后，运行文件时不需要使用./文件名，可直接使用文件名运行。

## 输入法

```
sudo apt install libopencc1 fcitx-libs fcitx-libs-qt fonts-droid-fallback  
wget "http://pinyin.sogou.com/linux/download.php?f=linux&bit=64" -O "sougou_64.deb"
sudo dpkg -i sougou_64.deb  
```

重启后  
输入法设置中点击+，在弹出对话框中将only show curent language勾选掉，然后搜索到sougou，然后进行添加  
(系统设置>语言支持>键盘输入方式系统然后选择 fcitx 项)  

安装不完整时移除  
sudo apt remove sogoupinyin  

## 快捷方式/搜索

dash按钮（类似windows按钮）

## 菜单栏隐藏

System Settings->Appearance->Behavior->Auto-hide the Launcher

## 桌面工具

sudo apt-get install cairo-dock  
cairo-dock                               启动  
cairo-dock -m                            设置  
sudo apt-get --purge remove cairo-dock   卸载  

## qtcreator

ctrl+i      自动排版  
ctrl+b      编译  
ctrl+r      运行  
ctrl+空格   代码提示  

=========cannot find lGL  
locate libGL或find /usr -name libGL* 搜索。  
搜索结果中发现/usr/lib/i386-linux-gnu/mesa/libGL.so.1文件（这个文件也可能在另一个目录中）  
使用ln -s /usr/lib/i386-linux-gnu/mesa/libGL.so.1 /usr/lib/libGL.so命令给已存在的库文件创建一个链接到/usr/lib目录。  

## LibreOffice

F5       文档结构  
cpack --config CPackSourceConfig.cmake  
