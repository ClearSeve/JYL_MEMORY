# algorithm

## 排序

```
如果stl的成员变量是类类型，需要重载类的<号
//假设要比较A内的int  x;成员
bool operator<( const  A& other) const
{
     if(x<other.x)  return  true;   //代表交换两个元素
	 else return false;
}

对数据排序:std::sort(v.begin(),v.end());

插入时排序：v.insert(std::upper_bound(v.begin(),v.end(),a1),a1); 
```

## 距离

```
size_t    i = std::distance(v.begin(),v.end());
可以返回两个迭代器之间的距离（此处返回了v的长度）  
```

## 去重

```
std::sort(v.begin(),v.end());
//将数据排成1,2,2,3,3,4,5
vector<int>::iterator iter = std::unique(v.begin(),v.end());
//将不重复的元素向前赋值，数据变成1,2,3,4,5,4,5，并返回4的位置（返回不重复位置的末尾）
v.erase(iter,v.end());

或者：std::sort(v.begin(),v.end());
v.resize(std::distance(v.begin(),std::unique(v.begin(),v.end())));
```

## 查找

```
vector<int>::iterator iter = std::find(v.begin(),v.end(),10);
查找到则iter执行找到的元素，查找不到则指向v.end();
如果是类类型，则需要重载==符号。
```

## 智能指针

```
std::auto_ptr<A> p(new A());
p->fun();  //使用指针
指针A在不需要使用时，会自动释放

p.reset(); //手动释放指针内存
p.reset(pb);//释放之前的指针，同时让p接管另一个指针

if(0 != p.get() ) p->fun();
对智能指针进行非空判断
```
