# VS

## 环境变量使用

存在环境变量CODE D:\code  

在工程属性配置时使用  
$(CODE)  
即可指代D:\code  

## cmd编译

call  "******\vcvarsall.bat"

devenv  /out build_result.txt  /rebuild  "release|win32"   ******.sln  

## 程序管理员权限

工程属性： 链接器-〉清单文件->UAC执行级别-〉requireAdministrator

## 生成事件

文件名如果包含空格，需要用引号  

### 预先生成事件

C++:  
工程属性->生成事件->预生成事件  
xcopy  /s  /q  /y  /d  $(CODE)\发布    $(OutDir)  

C#(不需要写生成的目标文件夹):  
工程属性->生成事件->预先生成事件命令  
xcopy  /s  /q  /y  /d  $(CODE)\发布  

 $(SolutionDir)
SolutionDir表示sln所在目录  

xcopy  /s  /q  /y  /d  $(ProjectDir)\res  
拷贝项目目录（sln下级目录）下的res文件夹内容


### 后期生成事件

copy  "$(TargetPath)"   $(CODE)\发布\xx.exe  

或者[C#无效]  copy  $(OutDir)$(TargetName)$(TargetExt)  $(CODE)\发布\xx.exe  
路径中有空格时需要用""  

## C#代码提示

重载函数:  
类内写入 override 后空格，会自动提示可以重载的函数  

事件处理:  
this.button1.Click +=然后使用tab键，直接生成对应事件的处理函数  

## 调试

### 变量显示

|类型        |       值                        |
|:-          |      -:                         |
|int         |      10                         |
|struct      |      {成员1=**  成员2=**}       |
|map         |      [成员数量]（元素1，元素2） |


### 操作

|快捷键             |      功能                                 |
|:-                 |      -:                                   |
|F7                 |        编译工程                           |
|Ctrl+F5            |        运行程序                           |
|Ctrl+F7            |        编译当前文件                       |
|F9                 |        设置断点                           |
|F5                 |       以调试状态运行/执行到下一断点       |
|F11                |       跟踪时进入函数内                    |
|Shift+F11          |       跳到上一层调用栈                    |
|F10                |       执行一句                            |
|Ctrl+F10           |        执行到光标所在行                   |
|Shift+F9           |      QuickWatch,并显示关标所在处的变量值  |
|Alt+3              |      Watch 查看窗口                       |
|Alt+4              |      Variables 监视变量(常用)             |
|Alt+5              |      显示寄存器                           |
|Alt+6              |      显示内存(常用)                       |
|Alt+7              |      显示堆栈情况                         |
|Alt+8              |      显示汇编码                           |



### 窗口

|快捷键             |      功能                |
|:-                 |      -:                  |
|ctrl+alt+L         |      解决方案管理器      |
|ctrl+alt+X         |      工具箱              |
|ctrl+shift+C       |      类视图              |
|ctrl+shift+E       |      资源视图            |
|ctrl + shift + F   |      查找所有结果        |
|Alt+F7             |  工程设置对话框          |


## 快捷键

|快捷键             |      功能                             |
|:-                 |      -:                               |
|Ctrl+k f           |    格式化代代码                       |
|Ctrl+c             |      复制一行                         |
|Ctrl+L             |        剪切一行                       |
|Shift+方向         |        选择的粘滞键                   |
|Ctrl+Z/Ctrl+Y      |     Undo/Redo                         |
|Ctrl+K,C           |       注释选中的行                    |
|Ctrl+K,U           |     取消选中行的注释                  |
|Ctrl+U             |   字母转化为小写(有的VC没有设置)      |
|Ctrl+Shift+U       |   字母转化为大写(有的VC没有设置)      |
|Ctrl+F             |   查找                                |
|Ctrl+H             |   替换                                |
|Ctrl+G             |   跳到文件中第n行                     |
|Ctrl+}             |   匹配括号(),{}                       |
|Ctrl+Shift+G       |   光标在一个文件名上,直接跳到指定文件 |


## 安装项目

解决方案属性，系统必备（从安装包路径下载）  
解决方案属性页，TargetPlantform   

文件系统编辑器，文件加入应用程序文件夹，程序文件夹，启动，桌面  

自定义操作编辑器，添加程序，属性中加入参数  

## nuget

工具-》nuget包管理器-》管理解决方案的nuget程序包  
界面中程序源包选择设置，添加本地包位置  
选择程序源包位置为添加的本地位置  

## 结构体对齐

c/c++ -> 代码生成 -> 结构体成员对齐

## python  

工具-》选项-》python-》调试-》使用旧版本调试程序
（或者安装升级ptvsd包）  

工程中python环境，右键可以打开交互环境、安装python包  

## linux

sudo apt-get install openssh-server g++ gdb gdbserver  

yum install gdb-gdbserver  
yum -y update gcc  
yum -y install gcc+ gcc-c++  

## 问题

无法找到资源编译器 rcdll.dll:         将rcdll.dll放到外层x86、x64文件夹  
XXX模块对于SAFESEH 映像是不安全的: 	 链接器->命令行->将 /SAFESEH:NO 键入附加选项框中
