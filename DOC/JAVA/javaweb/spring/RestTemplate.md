# RestTemplate

```
RestTemplate restTemplate = new RestTemplate();
String url = "https://aaaaa?id=0";
ResponseEntity<String> responseEntity=restTemplate.getForEntity(url,String.class);//获取返回数据字符串
String body = responseEntity.getBody();

Map map = new LinkedHashMap();
map = JSONObject.parseObject(body, LinkedHashMap.class);
```