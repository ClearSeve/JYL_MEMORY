# parameterType

## 单个参数

可以不写parameterType(嵌套语句中引用变量)，也可以限定参数类型
如： parameterType="int"  ,   parameterType="com.xxx.xxx"


如果是基本类型，直接引用变量：#{随便写}
如果是实体类，写成如#{id}，会自动在实体类中根据id字段拿出对应值
在嵌套语句中引用变量情况下，需要在函数声明时加@Param

## 多个参数

不用写parameterType或者写parameterType="map"


需要在对应函数中为参数加@Param注解，以标识xml引用的变量名
如：
public User fun(@Param("name") String aa,@Param("age") int bb) throws Exception;

xml中：select name from user where name=#{name} AND age=#{age}