# string

using namespace std;  

wstring 需要使用wcout和wcin  
使用前需要进行初始化：locale::global(locale(""));  

或者wcout.imbue(locale(""));wcin.imbue(locale(""));

## wstring转换

string str = "abc";  
wstring wstr(str.length(),L' ');  
std::copy(str.begin(), str.end(), wstr.begin());

## 函数实现

```
class string
{
public:
    char *m_data;
    string(const char *p=NULL)
    {
        if(p!=NULL)
        {
            int len=strlen(p);
            m_data=new char[len+1];
            strcpy(m_data,p);
        }
        else
        {
            m_data=new char;
            *m_data='\0';
        }
    }
    string(const string& other)
    {
        int len=strlen(other.m_data);
        m_data=new char[len+1];
        strcpy(m_data,other.m_data);
    }
    ~string(void)
    {
        delete[] m_data;
    }
    string& operator=(const string& other)
    {
        if(this==&other)
            return *this;
        else
        {
            delete[] m_data;
            int len=strlen(other.m_data);
            m_data=new char[len+1];
            strcpy(m_data,other.m_data);
        }
    }
};
```

## 输入输出

```
string类重载运算符operator>>用于输入，同样重载运算符operator<<用于输出操作。

getline(istream &in,string &s);用于从输入流in中读取一行字符串到s中，以换行符'\n'分开（可以读入空格，cin>>s遇到空格会截断字符串）

输入ab cd

getline(cin,str); //得到str为ab cd

cin>>str1>>str2;//得到str1为ab，str2为cd
```

## 构造

当构造的string太长而无法表达时会抛出length_error异常  
string(const char *s); //用c字符串s初始化  
string(int n,char c); //用n个字符c初始化  
string s2="hello"；

## 属性获取

int size()const; int length()const; //返回当前字符串的长度

bool empty()const; //当前字符串是否为空

int capacity()const; //返回当前容量（即string中不必增加内存即可存放的元素个数）

max_size()const;//返回string对象中可存放的最大字符串的长度，结果是一个很大的数值

## 字符操作

const char &operator[](int n)const;  
const char &at(int n)const;  

operator[]和at()均返回当前字符串中第n个字符的位置，但at函数提供范围检查，当越界时会抛出out_of_range异常，下标运算符[]不提供检查访问。

## 获取char*

const char* c_str()const;//返回一个以null终止的c字符串  
const char* data()const;//返回一个非null终止的c字符数组

## 操作函数

+ 查找

```
string::npos可以指代-1，当未查找到时，可以用来判断返回值。

pos = s.find(str,int pos = 0);//在s中查找str第一次出现的位置。默认从下标0位置开始查找
pos = s.rfind(str);//在s中查找str最后一次出现

s.find_first_of(str);//在s中查找str中任意字符第一次出现
s.find_last_of(str);//在s中查找str中任意字符最后一次出现
s.find_first_not_of(str);//在s中查找第一个不属于str的字符
s.find_last_not_of(str);//在s中查找最后一个不属于str的字符
```

+ 子串  
string substr(int pos = 0,int n = npos)const;//返回pos开始的n个字符组成的字符串，n默认值时代表子串是pos到末尾。

+ 替换  

```
s.replace(pos,len,str);//从pos位置开始的len个字符替换为str  
s.replace(itA,itB,str);//将迭代器AB之间的字符删除替换为str

inline int stringReplace(string &str,const char* findstr,const char* repl)
{
    int len = strlen(findstr);
    string::size_type pos = str.find(findstr);
    int count = 0;
    while(pos != string::npos)
    {
        str.replace(pos,len,repl);
        ++count;
        pos = str.find(findstr,pos);
    }
    return count;
}
```

+ 调整大小  
void resize(int len,char c);//把字符串当前大小置为len，并用字符c填充不足的部分

+ 拷贝  
int copy(char *s, int n, int pos = 0)const;//把当前串中以pos开始的n个字符拷贝到以s为起始位置的字符数组中，返回实际拷贝的数目

+ 插入

```
//p0位置插入字符串s中pos开始的前n个字符
string &insert(int p0, const char *s);
string &insert(int p0, const char *s, int n);
string &insert(int p0,const string &s);
string &insert(int p0,const string &s, int pos, int n);


string &insert(int p0, int n, char c);//在p0处插入n个字符c

iterator insert(iterator it, char c);//在it处插入字符c，返回插入后迭代器的位置

void insert(iterator it, const_iterator first, const_iterator
last);//在it处插入[first，last）之间的字符

void insert(iterator it, int n, char c);//在it处插入n个字符c
```

+ 删除

```
string &erase(int pos = 0, int n =npos);//删除pos开始的n个字符，返回修改后的字符串

iterator erase(iterator first, iterator
last);//删除[first，last）之间的所有字符，返回删除后迭代器的位置

iterator erase(iterator it);//删除it指向的字符，返回删除后迭代器的位置
```
