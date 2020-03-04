# Dictionary

```
Dictionary<string, int> dir=new Dictionary<string,int>();
dir.Add("a", 1);
dir.Add("b", 2);

//通过键值查找
if (dir.ContainsKey("a")){int x = dir["a"];}

//遍历
foreach (string key in dir.Keys)
foreach(int value in dir.Values)
foreach (KeyValuePair<string, int> d in dir)

//排序遍历
foreach (var item in dic.OrderBy(dicItem=> dicItem.Key))
{//用key进行排序
}
```
