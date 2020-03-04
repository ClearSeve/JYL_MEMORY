# List

## 定义

`List<string> list=new List<string>();`  
可以直接用下标访问 list[3]

## 遍历

foreach(string  x  in list)

## 排序

```
class Dat
{
   public int a = 0;
}
```

### 使用lambda

```
ll.Sort((Dat x, Dat y) =>
{
    return x.a - y.a;
});
```

### 实现接口IComparable

（右键菜单，选择实现接口，可以自动生成代码）  
基本类型已实现该接口  

```
class Dat : IComparable 
{
    int a = 0;

    int IComparable.CompareTo(Object obj)
    {
        Dat temp = (Dat)obj;
        return a - temp.a;
    }
}

ll.Sort();
```

### 只排序部分元素

实现接口IComparer  

```
class Cmp : IComparer<Stu>
{

    public int Compare(Stu x, Stu y)
    {
        return x.x - y.x;
    }
}
```

从下标4的元素开始，一共排序3个  
Cmp cmp = new Cmp();  
l.Sort(4, 3, cmp);  

## 求最大最小值

实现接口IComparable  

`Stu max = l.Max<Stu>();`  
`Stu min = l.Min<Stu>();`

## 查找

//查找满足某种条件的元素，在函数中写查询条件  

```
 bool find(Stu st)
{
    return st.x == 10;
}

Stu st = l.Find(find);
if (null != st){}

//也可以使用委托
Stu st = l.Find((Stu temp)=> temp.x == 10);
```
