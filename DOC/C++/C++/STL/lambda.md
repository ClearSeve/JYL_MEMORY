# lambda表达式

lambda表达式的参数列表基本和函数的一致，有如下限制：  
参数列表不能有默认参数  
不能是可变参数列表  
所有的参数必须有个变量名


```
[this, &y](int x) {} //[]中写入要使用的变量列表，&为访问所有外部元素，=为按值访问所有外部元素


//作为参数传递
template<typename _Fun>
void fun(_Fun callback)
{
	int x = 10;
	callback(x);
}

调用：
int factor = 10;
fun([&] (int &x){
	cout << factor* x << endl;
});


//map访问方式
map<int, Data> m;
m[1] = Data();
m[2] = Data();
for_each(m.begin(), m.end(), [](pair<int, Data>  dat) { cout << dat.first << " " << dat.second.a << endl;});

//返回值
int z = []()->int { return 10; }();
()->int可以省略

//设置数组的值
const int SIZE = 20;
int array[SIZE];
generate_n(array, SIZE,[]() { return rand() % 10; });
```
