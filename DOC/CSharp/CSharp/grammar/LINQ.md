# LINQ

实现IEnumerable的类都可以用linq

## 查询

```
string[] arry = { "a1","a2","b1","b2","c"};
var query = from n in arry where n.StartsWith("a") select n;//LINQ语句
string str="";
foreach(var s in query){str += s;}

from n in arry类似foreach
where后是限定选择的条件
select是必须的，指定结果集中包含哪些元素。

int[] numbers = {3,5,2,5,12,5,36,75,42};
var qur = from num in numbers where num % 2 == 0 orderby num descending select num;
//只有调用语句才会运行

//var qur = numbers.Where(n => n % 2 == 0).OrderBy(n => n);
foreach (var i in qur) Console.WriteLine(i + "");


int[] ret = qur.ToArray<int>();  
List<int> ret = qur.ToList();
```

## 方法语句

```
var query = arry.Where(n=>n.StartsWith("a"));
var query = arry.Where(n=>n.Length==1);
```

## 排序

```
var query = from n in arry where n.StartsWith("a") orderby n select n;
var query = arry.OrderBy(n=>n).Where(n=>n.StartsWith("a"));

var query = arry.OrderBy(n => n.Substring(n.Length - 1)).Where(n=>n.StartsWith("a"));//排序时，按照最后一个字符排
```

## 聚合运算符

可以对查询结果进行分析。  
int count=query.Count();

### let
```
string[] strs = { "aA-11", "bb-22","cc" };
var qur = from s in strs
          let words = s.Split('-')
          from word in words
          let w = word.ToUpper()
          select w;
foreach (var s in qur)
    Console.WriteLine("{0}",s);
```

### 分组与表连接

```
lass Customer
{
    public string Name { get; set; }
    public string City { get; set; }
}
class Employee
{
    public string Name { get; set; }
    public int ID { get; set; }
}

List<Customer> cust = new List<Customer>();
cust.Add(new Customer() { Name="aa", City = "beijing" });
cust.Add(new Customer() { Name = "bb", City = "beijing" });
cust.Add(new Customer() { Name = "cc", City = "xian" });

List<Employee> em = new List<Employee>();
em.Add(new Employee() { Name = "aa", ID = 1 });
em.Add(new Employee() { Name = "dd", ID = 2 });
```

## 分组查询

```
var quer = from c in cust
group c by c.City;

foreach (var cg in quer)
{
    Console.WriteLine(cg.Key);
    foreach (var c in cg)    
       Console.WriteLine(" " + c.Name);
}



var quer = from c in cust
           group c by c.City into custGroup
           where custGroup.Count() >= 2
           select new { City = custGroup.Key, number = custGroup.Count() };
foreach (var c in quer)
    Console.WriteLine("{0} {1}", c.City,c.number);
```


## 表连接

```
var queryJoin = from c in cust
                         join e in em on c.Name equals e.Name
          select new { PersonName = c.Name, PersonID = e.ID, PersonCity = c.City };

foreach (var p in queryJoin)
      Console.WriteLine("{0} {1} {2}", p.PersonName, p.PersonID, p.PersonCity);
```