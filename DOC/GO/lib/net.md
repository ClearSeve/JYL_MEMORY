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
	defer r.Body.Close()
	
    //fmt.Fprintln(w, "hello world")

	// 检查是否为post请求
	if r.Method != http.MethodPost {
		w.WriteHeader(http.StatusMethodNotAllowed)
		fmt.Fprintf(w, "invalid_http_method")
		return
	}

	//application/x-www-form-urlencoded 格式
	//r.ParseForm()
	//name:=r.PostFormValue("name")
	//fmt.Fprintf(w, "name="+name+"\n")


	//application/json格式
	
	bys, _ := ioutil.ReadAll(r.Body)
	jsonstr := string(bys)
	fmt.Fprintf(w, jsonstr)
}

func main() {
	http.HandleFunc("/", IndexHandler)
    http.ListenAndServe("0.0.0.0:8080", nil)

    //https
	http.ListenAndServeTLS(":443", "cajyl.crt",
		"cajyl.key", nil)
}


const (
	StatusContinue           = 100
	StatusSwitchingProtocols = 101

	StatusOK                   = 200
	StatusCreated              = 201
	StatusAccepted             = 202
	StatusNonAuthoritativeInfo = 203
	StatusNoContent            = 204
	StatusResetContent         = 205
	StatusPartialContent       = 206

	StatusMultipleChoices   = 300
	StatusMovedPermanently  = 301
	StatusFound             = 302
	StatusSeeOther          = 303
	StatusNotModified       = 304
	StatusUseProxy          = 305
	StatusTemporaryRedirect = 307

	StatusBadRequest                   = 400
	StatusUnauthorized                 = 401
	StatusPaymentRequired              = 402
	StatusForbidden                    = 403
	StatusNotFound                     = 404
	StatusMethodNotAllowed             = 405
	StatusNotAcceptable                = 406
	StatusProxyAuthRequired            = 407
	StatusRequestTimeout               = 408
	StatusConflict                     = 409
	StatusGone                         = 410
	StatusLengthRequired               = 411
	StatusPreconditionFailed           = 412
	StatusRequestEntityTooLarge        = 413
	StatusRequestURITooLong            = 414
	StatusUnsupportedMediaType         = 415
	StatusRequestedRangeNotSatisfiable = 416
	StatusExpectationFailed            = 417
	StatusTeapot                       = 418

	StatusInternalServerError     = 500
	StatusNotImplemented          = 501
	StatusBadGateway              = 502
	StatusServiceUnavailable      = 503
	StatusGatewayTimeout          = 504
	StatusHTTPVersionNotSupported = 505

	statusPreconditionRequired          = 428
	statusTooManyRequests               = 429
	statusRequestHeaderFieldsTooLarge   = 431
	statusNetworkAuthenticationRequired = 511
)
```
