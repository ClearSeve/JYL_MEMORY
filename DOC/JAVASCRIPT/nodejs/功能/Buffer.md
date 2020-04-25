# Buffer

## 创建

```
const buf = Buffer.alloc(10,1)//创建10个字节长度的buf，初始化全部为1，不写第二个参数初始化为0
Buffer.allocUnsafe(10)//创建但不进行初始化

const buf = Buffer.from(buffer)//复制

let array=[1,10,12]
const buf = Buffer.from(array)

const buf = Buffer.from('abcdefg')

```

## 长度
buffer.length


## 读取

```
let str = buf.toString()//默认转换为utf8字符串
buf.toString('utf8',1,4)//从1下标开始到下标3的数据进行转换
buf.toString('hex')
buf.toString('base64')

json = JSON.stringify(buf)//{"type":"Buffer","data":[...]}


buf.readUInt8(offset)
buf.readUInt16LE(offset)//小端uint16
buf.readUInt16BE(offset)//大端uint16
buf.readUInt32LE
buf.readFloatLE//32位浮点数
buf.readDoubleLE//64位浮点数
```

## 写入

```
//写入缓冲区下标2开始区域，不写第二个参数则默认从0位置开始写  
writelen = buf.write("xxx",2) 

buf.writeUInt8(value, offset)
buf.writeUInt16LE(value, offset)
buf.writeInt32LE
buf.writeDoubleLE
```

## 裁剪
//从下标0开始裁剪到下标1
var buffer2 = buf1.slice(0,2)

## 合并
var buffer3 = Buffer.concat([buf1,buf2])

## 拷贝

```
const buf1 = Buffer.from('012345')
const buf2 = Buffer.from('6789')
let bufStartPos = 2
let sourceStartPos = 1
let sourceEndPos = 3
buf2.copy(buf1, bufStartPos,sourceStartPos,sourceEndPos);
//结果为：017845
```




