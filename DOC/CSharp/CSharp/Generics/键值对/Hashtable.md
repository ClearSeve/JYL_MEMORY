# Hashtable

可以存储不同元素，加入数据时，key不能相同，否则异常

```
const int capacity = 100;
Hashtable ht = new Hashtable(capacity);

ht.Add(0, 1);
 ht.Add(1, new Dat(111));
ht.Add(2, 1.5);

Console.WriteLine(ht[0]);
((Dat)ht[1]).Show();
 Console.WriteLine(ht[2]);
```
