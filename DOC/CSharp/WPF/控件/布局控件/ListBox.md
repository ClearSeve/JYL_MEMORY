# ListBox

Item可以是字符串，也可以是任意类型视图控件

```
<ListBox Name="listbox1" ></ListBox>
```

## 添加方法1

listbox1.Items.Add(1);  
listbox1.Items.Add(2);

## 添加方法2

```
List<int> ll = new List<int>();

ll.Add(2);
ll.Add(3);
listbox1.ItemsSource = null;
listbox1.ItemsSource = ll;
```

## 添加方法3

```
<ListBox Name="listbox1" DisplayMemberPath="ss"></ListBox>

struct Inf
{
    public string ss { get; set; }
}

List<Inf> ll = new List<Inf>();

Inf inf1 = new Inf();
inf1.ss = "aa";
ll.Add(inf1);

listbox1.ItemsSource = null;
listbox1.ItemsSource = ll;
```
