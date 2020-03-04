# HashSet

可以对数据进行去重，不进行排序
非基本类型需要重载函数

```
class Dat
{
    int a = 0;

    public override bool Equals(object obj)
    {
        return a == ((Dat)obj).a;
    }
    public override int GetHashCode()
    {
        return a;
    }
}
HashSet<Dat> set = new HashSet<Dat>();

set.Add(new Dat(0));
set.Add(new Dat(2));
set.Add(new Dat(3));
set.Add(new Dat(0));
```
