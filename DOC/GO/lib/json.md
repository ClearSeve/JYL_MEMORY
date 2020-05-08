# json

```
//map[string]interface{}  key:val
//[]interface{}           list

import (
	"fmt"
	"encoding/json"
)
```

## 解析

map方式
```
jstr := "{\"username\":\"jyl\",\"data\":\"123\"}"
byjs :=  []byte(jstr)

var jsonpara map[string]interface{}
err := json.Unmarshal(byjs, &jsonpara)
if err != nil {
	return
}

data, ok := jsonpara["userame"]
if ok{
	fmt.Println(data)
}
```

结构体方式
```
type StuRead struct {
    Name  interface{} `json:"username"`
    Data  interface{} //变量名必须大写,可解析字符串中小写名
}


jstr := "{\"username\":\"jyl\",\"Data\":\"123\"}"
byjs :=  []byte(jstr)
sd := StuRead{}
err := json.Unmarshal(byjs, &sd)
if err != nil {
	return
}

switch v := sd.Name.(type) {
case string:
	fmt.Println(v)
}

```

## 创建

map方式
```
var jsondat map[string]string = make(map[string]string)
jsondat["username"] = "jyl"
jsondat["data"] = "1234"

bys, err := json.Marshal(jsondat)
if err != nil {
	return
}

fmt.Println(bys)
```

结构体方式
```
type fileInfoJSON struct {
	Dir   string  `json:"dir"`
	File  []string //变量名必须大写,生成字符串名称也为大写开头
}

inf := fileInfoJSON{"aa",[]string{"x","y","z"}}

bys, _ := json.Marshal(&inf)	
fmt.Println(string(bys))
```