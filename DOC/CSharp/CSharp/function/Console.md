
# Console

```
Console.Title = "窗口标题";

//输出文字的前景色和背景色
Console.ForegroundColor = ConsoleColor.Green;
Console.BackgroundColor = ConsoleColor.White;

//在同一位置输出数据
int leftOrg = Console.CursorLeft;
int topOrg = Console.CursorTop;
Console.Write("xxx");
Console.SetCursorPosition(leftOrg, topOrg);
Console.Write("yyy");

```
