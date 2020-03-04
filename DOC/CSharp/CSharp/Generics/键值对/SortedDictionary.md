# SortedDictionary

## 排序

```
对key进行排序，key需要实现IComparable接口
struct Stu:IComparable
{
       public int a;
       public int b;
       int IComparable.CompareTo(object obj)
       {//从小到大排序
            return a - ((Stu)obj).a;
       }
}
```

## 获取首部数据

```
using System.Linq;
string val = SortedDictionaryObj.Values.First<string>();
```
