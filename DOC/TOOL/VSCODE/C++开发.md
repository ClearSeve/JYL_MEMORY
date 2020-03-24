# C++开发

## 开发环境安装

linux需要安装开发环境：

ubuntu:
sudo apt-get install build-essential
sudo apt install gdb

centeros:
yum install -y gcc-c++


## 工程配置

在工程目录的下需要包含.vscode目录
.vscode目录下包含配置文件tasks.json和launch.json
（VS工作空间目录和.vscode目录同级，代码也需要在这一级放置）

```
tasks.json:

{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "TaskName",     //任务名TaskName
            "type": "shell",
            "command": "g++",
            "args": [
                "-g",  //断点调试
                "Server.cpp",
                "-o",
                "a.out" //生成文件a.out
            ]
        }
    ]
}
```


```

launch.json

{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "debugLaunch",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/a.out", //运行生成文件的a.out
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",  
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "TaskName"  //先执行TaskName
        }
    ]
}
```


代码提示有问题时，需要配置c_cpp_properties.json
```

gcc -v -E -x c++ -
获取头文件目录

c_cpp_properties.json:

{
    "configurations": [
        {
            "name": "linux",
            "includePath": [
                "${workspaceFolder}/**",
                "/usr/include/c++/7",
                "/usr/include/x86_64-linux-gnu/c++/7",
                "/usr/include/c++/7/backward",
                "/usr/lib/gcc/x86_64-linux-gnu/7/include",
                "/usr/local/include",
                "/usr/lib/gcc/x86_64-linux-gnu/7/include-fixed",
                "/usr/include/x86_64-linux-gnu",
                "/usr/include"
            ],
            "cStandard": "c11",
            "cppStandard": "c++17",
            "intelliSenseMode": "gcc-x64"
        }
    ],
    "version": 4
}
```