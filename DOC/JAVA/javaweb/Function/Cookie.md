# Cookie

## 设置
```
向客户端设置Cookie

Cookie cookie = new Cookie("token", token);
cookie.setPath("/");
response.addCookie(cookie);
```

## 获取
```
Cookie[] cookies = request.getCookies();
if(null != cookies)
{
	for(int i = 0 ; i < cookies.length;++i)
	{
		if(cookies[i].getName().equals("token"))
		{
			token = cookies[i].getValue();
		}	 
	}
}
```