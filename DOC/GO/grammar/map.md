# map

```
var info map[int]int = make(map[int]int)
info[0] = 0
info[1] += 10
info[1] += 10
for id := range info {
	fmt.Println(id, info[id])
}
id, has := info[0] //检验key是否存在
if has {
	fmt.Println(id)
}
```