# net

## socket

```
import (
	"fmt"
	"net"
	"time"
)

func handleRead(c net.Conn) {
	defer c.Close()
	var buf = make([]byte, 1024)
	for {
		revlen, err := c.Read(buf)
		if err != nil {
			fmt.Println("read error:", err)
			return
		}

		fmt.Printf("revlen:%d\n", revlen)
		var str string = string(buf[:revlen])
		fmt.Println(str)
	}
}
func server() {
	l, err := net.Listen("tcp", ":1234")
	if err != nil {
		fmt.Println("listen error:", err)
		return
	}

	for {
		c, err := l.Accept()
		if err != nil {
			fmt.Println("accept error:", err)
			break
		}
		go handleRead(c)
	}
}



func clent() {
	//conn, err := net.Dial("tcp", "127.0.0.1:1234") 	//阻塞连接
	//带超时机制的连接
	conn, err := net.DialTimeout("tcp", "127.0.0.1:1234", 2*time.Second)
	if err != nil {
		fmt.Println("connect error:", err)
	}

	data := "abcd"
	n,erro := conn.Write([]byte(data))
}
```

## http

```
import (
    "fmt"
    "net/http"
)

func IndexHandler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(w, "hello world")
}

func main() {
	http.HandleFunc("/", IndexHandler)
    http.ListenAndServe("0.0.0.0:8080", nil)
}
```