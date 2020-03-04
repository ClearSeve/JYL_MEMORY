# String

```
Hello \
Abc       使用\可以对字符串进行折行处理

"abc"，"A" 都是字符串类型
"***'str'***",   '***"str"***' 字符串用单引号或者双引号包含，单引号内可以包含双引号，双引号内可以包含单引号。
转译字符：\”


length          "***".length可以获取字符串长度

substr          substr(1,2)   返回下标1开始，长度2的字符串。第二个参数可省略

indexOf         indexOf("bc")  从字符串开头查找，返回bc所在的位置下标

lastIndexOf     lastIndexOf("bc")  从字符串末尾查找，返回bc所在的位置下标

split           将字符串根据-分割
                var arr = str.split("-");
                for(i = 0 ; i < arr.length; ++i){}

replace         var abc = "aaxxxaayyyyaa"
                abc = abc.replace("aa" ,'bb')//使用字符串，只替换第一个aa
                abc = abc.replace(new RegExp("aa","g") ,'bb')//使用正则表达式，替换所有的aa

正则表达式       var str = "http://www.abc.org";
                var Regex = /http:\/\/w+\.(.*)/g;  //正则表达式     /g表示匹配全部
                var result = Regex.exec(str); //result为一个字符串数组，第一个元素是源字符串，后面的元素为正则表达式中每个（）内的内容
                String的match,search,replace可以使用正则表达式
```
