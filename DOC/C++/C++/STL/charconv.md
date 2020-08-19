# charconv

```
C++17
#include <charconv>

char str[10] = {0};
int val = 999;
auto [pstrend, err] = std::to_chars(str, str + 10, val);
if (err == std::errc())  std::cout << str;
```