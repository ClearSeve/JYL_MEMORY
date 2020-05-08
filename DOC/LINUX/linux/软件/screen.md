# screen

|命令             |功能                  |
|:-:              |:-:                  |
|screen -S abc    | 创建一个名为abc的窗口 |
|screen -r abc    | 根据窗口名abc打开窗口,处于Attached模式,先screen -d|
|screen -ls       |查看创建的所有窗口     |
|screen           | 创建一个新窗口        |
|screen -r 1234   | 根据id为1234打开窗口  |
|exit             |关闭当前窗口           |
|screen -wipe     |清除掉dead窗口         |

查看所有窗口页面
字符串1234.abc 中1234代表id，abc代表窗口名称
Detached   挂起状态，无终端在连接会话
Attached   有终端在连接会话。
