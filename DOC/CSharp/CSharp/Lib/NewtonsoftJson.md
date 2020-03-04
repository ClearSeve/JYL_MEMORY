# NewtonsoftJson

```
{
    "key1": "val1",
    "key2": {
        "obj1": ["a", "b", "c"],
        "obj2": 12.3
    }
}
```

## 解析

```
JObject jsonObj = (JObject)JsonConvert.DeserializeObject("{'key1':'val1','key2':{'obj1':['a','b','c'],'obj2':12.3}}");
string val = (string)jsonObj["key1"];
JArray arry = (JArray)jsonObj["key2"]["obj1"];
string valb = (string)arry[1];
double val2 = (double)jsonObj["key2"]["obj2"];
```

## 创建json串

```
JObject obj= new JObject();
obj["key1"] = "val1";
JObject objKey2 = new JObject();
JArray  arry1 = new JArray() { "a","b","c"};
objKey2["obj1"] = arry1;
objKey2["obj2"] = 12.3;
obj["key2"] = objKey2;
string json = obj.ToString();
```
