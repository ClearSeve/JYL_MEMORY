# main

```
package main //包声明

/* 引入包*/
import (
	"fmt"
)

func main() { // {不能单独放一行
	defer fmt.Println("4") //程序结束时运行
	fmt.Println("1")
	defer fmt.Println("3")
	fmt.Println("2")
}
```
