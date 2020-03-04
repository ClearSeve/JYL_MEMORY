# json

## 依赖

```xml
<dependency>
    <groupId>org.json</groupId>
    <artifactId>json</artifactId>
    <version>20180130</version>
</dependency>
```

## 生成

```
//{"name":"AA","do":["eat","sleep"],"age":10}
JSONObject obj = new JSONObject() ;

obj.put("name", "AA") ;
obj.put("age",10) ;

JSONArray arr = new JSONArray() ;
arr.put("eat") ;
arr.put("sleep") ;
obj.put("do", arr) ;

String str = obj.toString() ;
System.out.println(str);
```

## 解析

```
String str = "{\"dat\":[{\"inf\":\"abc\",\"lon\":\"116\",\"lat\":\"116\"},{\"inf\":\"def\",\"lon\":\"116.1\",\"lat\":\"116.1\"},{\"inf\":\"gg\",\"lon\":\"116.2\",\"lat\":\"116.2\"}]}";

JSONObject jsonObj = new JSONObject(str);
JSONArray arr = jsonObj.getJSONArray("dat");

for (Iterator<Object> tor = arr.iterator(); tor.hasNext();) {
    JSONObject obj = (JSONObject) tor.next();
    System.out.println(obj.get("inf"));
    System.out.println(obj.get("lon"));
    System.out.println(obj.get("lat"));
}


String json = '[["abc","116","39"],["def","116.1","39.2"],["gg","116.2","39.1"]]'
JSONArray arr = new JSONArray(json);
int len = arr.length();
for (int pos = 0 ; pos< len;++pos)
{
    JSONArray arrObj = (JSONArray)arr.get(pos);
    if(arrObj.length() != 3) continue;

    String infStr = (String) arrObj.get(0);
    String lonStr = (String) arrObj.get(1);
    String latStr = (String) arrObj.get(2);
}
```
