# testing

文件名必须是: 源文件名_test
函数以Test开头

运行 go test

```
import (
	"testing"
)


func TestFun(t *testing.T) {
	t.Errorf("err %d", 11)
}
```