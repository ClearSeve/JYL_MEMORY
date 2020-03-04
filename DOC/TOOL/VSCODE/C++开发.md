# C++开发

linux需要安装开发环境：  
ubuntu:
sudo apt-get install build-essential
sudo apt install gdb
centeros:
yum install -y gcc-c++

.vscode目录下配置文件：

## c_cpp_properties.json

gcc -v -E -x c++ -
获取头文件目录

```
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

## tasks.json

```
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "TaskName",     // task的名字
            "type": "shell",   
            "command": "g++",   
            "args": [ 
                "-g", // 加上-g可以断点调试
                "Server.cpp",
                "-o",
                "a.out"
            ]
        }
    ]
}
```

## launch.json

```
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
            "program": "${workspaceFolder}/a.out",   
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
            "preLaunchTask": "TaskName"  //先执行TaskName这个task
        }
    ]
}
```
