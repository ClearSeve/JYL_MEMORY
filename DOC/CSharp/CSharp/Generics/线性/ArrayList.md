# ArrayList

存储空间连续  
可以存储不同类型元素  
当每个元素存储类型相同时，与list用法相同  

```
const int capacity = 100;
ArrayList ll = new ArrayList(capacity);

ll.Add(1);
ll.Add(1.5);
ll.Add("abc");
ll.Add(new Dat(2));

Console.WriteLine(ll[0]);
Console.WriteLine(ll[1]);
Console.WriteLine(ll[2]);
Dat dd = (Dat)ll[3];
```
