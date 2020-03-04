# lambda表达式

```
lambda表达式的参数列表基本和函数的一致，有如下限制：
参数列表不能有默认参数
不能是可变参数列表
所有的参数必须有个变量名


遍历每个元素，让该元素*10；
struct A{ void operator()(int &x){x = x*10;} }; //需要重载()
std::for_each(v.begin(),v.end(),A()); // vector<int> v;
//使用lambda
for_each(v.begin(), v.end(), [](int &a) { a *= 10;});



struct Data
{
	Data():a(1),b(1){}
	int a;
	int b;
};
vector<Data>  v(5);

for_each(v.begin(), v.end(), [](Data &dat) { dat.a *= 10;});//访问每个元素的引用
for_each(v.begin(), v.end(), [](Data dat) {cout << dat.a << endl;});//访问每个元素的值

//////////访问外部变量
int sum = 0;
for_each(v.begin(), v.end(), [&](Data &dat) { sum += dat.a ;});//按引用访问外部元素
 
int  temp = 100;
for_each(v.begin(), v.end(), [=](Data dat) {cout << dat.a * 100 << endl;});//按值访问外部元素

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
