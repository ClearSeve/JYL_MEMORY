# token验证

## POM
```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
      <groupId>com.auth0</groupId>
      <artifactId>java-jwt</artifactId>
      <version>3.4.0</version>
</dependency>
```

## AuthenticationInterceptor.java

```
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.auth0.jwt.JWT;
import com.auth0.jwt.JWTVerifier;
import com.auth0.jwt.algorithms.Algorithm;
import com.auth0.jwt.exceptions.JWTDecodeException;
import com.auth0.jwt.exceptions.JWTVerificationException;
import com.rmt.xcc.domain.User;
import com.rmt.xcc.service.MyService;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.method.HandlerMethod;
import org.springframework.web.servlet.HandlerInterceptor;
import java.lang.reflect.Method;
import java.util.Date;

public class AuthenticationInterceptor implements HandlerInterceptor {
    @Autowired
    private MyService ser;

    @Override
    public boolean preHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse,
            Object object) throws Exception {
        // 如果不是映射到方法直接通过
        if (!(object instanceof HandlerMethod)) {
            return true;
        }

        // 检查有没有需要用户权限的注解
        HandlerMethod handlerMethod = (HandlerMethod) object;
        Method method = handlerMethod.getMethod();
        if (!method.isAnnotationPresent(UseToken.class))
            return true;
        
        //从header或者Cookie中获取token
        String token = httpServletRequest.getHeader("token");
        if (null == token) {
            Cookie[] cookies = httpServletRequest.getCookies();
            if (null != cookies) {
                for (int i = 0; i < cookies.length; ++i) {
                    if (cookies[i].getName().equals("token")) {
                        token = cookies[i].getValue();
                    }
                }
            }
        }


        if (token == null) {
            throw new RuntimeException("无token，请重新登录");
        }

        // 获取 token 中的 userName
        String userName = null;
        try {
            userName = JWT.decode(token).getAudience().get(0);
        } catch (JWTDecodeException j) {
            throw new RuntimeException("401");
        }
        User user = ser.findUser(userName);
        if (user == null) {
            throw new RuntimeException("用户不存在，请重新登录");
        }

        // 验证 token
        JWTVerifier jwtVerifier = JWT.require(Algorithm.HMAC256(user.getPassword())).build();
        try {
            jwtVerifier.verify(token);
        } catch (JWTVerificationException e) {
            throw new RuntimeException("401");
        }
        return true;

    }


    public static String GenToken(String userName,String password,HttpServletResponse response)
	{
		Date start = new Date();
		long currentTime = System.currentTimeMillis() + 60* 60 * 1000;//一小时有效时间
		Date end = new Date(currentTime);
		String token = "";
		
		token = JWT.create().withAudience(userName).withIssuedAt(start).withExpiresAt(end)
				.sign(Algorithm.HMAC256(password));

		if(null != response)
		{
			Cookie cookie = new Cookie("token", token);
			cookie.setPath("/");
			response.addCookie(cookie);
		}

		return token;
	}
}
```

## InterceptorConfig.java

```
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class InterceptorConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(authenticationInterceptor())
                .addPathPatterns("/**");    // 拦截所有请求
    }
    @Bean
    public AuthenticationInterceptor authenticationInterceptor() {
        return new AuthenticationInterceptor();
    }
	
}
```

## UseToken.java

```
import java.lang.annotation.Retention;
import java.lang.annotation.Target;

import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.ElementType;

@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface UseToken {
}
```

## 使用

```
Controller中登录函数：
User user = ser.getUser(userName,password);
String token = AuthenticationInterceptor.GenToken(user.getUserName(),user.getPassword(),response);
```

```
Controller中使用验证的函数：

@UseToken
@RequestMapping("/index")
public String index(ModelMap map) {
	return "index";
}
```