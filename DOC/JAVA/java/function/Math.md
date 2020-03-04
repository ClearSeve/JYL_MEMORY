# Math

```
Math.PI
Math.ceil(1.1)//向上取整 值为2
Math.floor(1.9)//向下取整  值为1
Math.abs(-1)//绝对值
Math.round(1.5)//四舍五入 值为2
Math.random()//生成大于等于0.1小于1的随机数

产生10个[0-100]的随机数
import java.util.Random;
Random r = new Random();
for(int x = 0; x < 10 ; ++x)
{
      System.out.println(r.nextInt(100));
}
```
