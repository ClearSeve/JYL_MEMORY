# resultType

## 对象/List类型

单条/多条

resultType="Integer"
resultType="com.**"

基本类型直接将结果赋值  
实体类型时，将记录取出，并根据字段名匹配对象中的变量名进行赋值  
多条数据时，返回值函数声明需要写成`List<ClassName>`
返回值可以用`List<任意包装类型>`接收

## Map类型

单条/多条

resultType="map"

返回值是json: [{key1=val1, key2=value2}, {key1=val3, key2=val4}]


## Map类型

多条

Map中value的类型，如：resultType="com.**"

需要在Mapper的java文件中定义map中key的类型及对应的字段名

```
如以下代码定义使用age这个字段（数字类型）作为key：
@MapKey("age")
public Map<Integer,User> fun() throws Exception;
```