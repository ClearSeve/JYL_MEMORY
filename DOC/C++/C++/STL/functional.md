# functional

```
int fun1(int a,double b){return a + b;}
auto fun2 = [](int a, double b) {return a - b; };
class Functor
{
public:
	int operator() (int a,double b){return a*b;}
};
class ABC
{
	int x;
public:
	ABC() { x = 1000; }
	int fun3(int a, double b) { return a/b + x; }
};
void __fun(int a,  std::function<int(int, double)> fun)
{
	double  dd = 100.2;
	int ret = fun(a, dd);
	cout << ret << endl;
}


__fun(1, fun1);
__fun(1, fun2);
int x = 5;
__fun(1, [&](int a, double b) {return x + a + b; });
__fun(1, Functor());
ABC aa;
std::function<int(int,double)>  obj = std::bind(&ABC::fun3, &aa, std::placeholders::_1,std::placeholders::_2);
__fun(1, obj);
```
