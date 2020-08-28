# main

```
package main //包声明

/* 引入包*/
import (
	"fmt"
)

func main() { // {不能单独放一行
    
	//获取命令行2个参数
	args := os.Args
	if len(args) != 3 {
		return
	}
	ar1 := args[1]    //第一个参数
	ar2 := args[2] //第二个参数


	defer fmt.Println("4") //程序结束时运行
	fmt.Println("1")
	defer fmt.Println("3")
	fmt.Println("2")
}
```
