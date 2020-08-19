# string_view

```
C++17
#include <string_view>

const char* str = "ab,cdef,gh";
//大字符串视图
std::string_view sv(str, strlen(str));
std::string_view sv1 = sv.substr(0,sv.rfind(','));//"ab,cdef"
std::string_view sv2 = sv1.substr(sv1.find(',')+1);//"cdef"
const char* memery = sv2.data();//string_view并没有尾0
std::string sv2Str(memery, memery + sv2.length());
std::cout<< sv2 <<":"<< sv2.length()<<std::endl<<"memery:"<< memery;
```
