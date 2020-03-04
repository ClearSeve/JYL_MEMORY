# main

```
if __name__ == "__main__":
    for i in range(1,len(sys.argv)):
        print("arg" + str(i) + ": " + sys.argv[i])
```

每个文件都由一个模块名__name__，如果是被import__name__为文件名。如果是运行开始的文件则为__main__  
使用if可以让内容只在该文件被启动运行时执行，而该模块被调用时不执行
