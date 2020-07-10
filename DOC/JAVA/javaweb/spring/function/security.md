# security

## POM

```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

## SecurityConfig

```
import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.web.authentication.AuthenticationFailureHandler;
import org.springframework.security.web.authentication.AuthenticationSuccessHandler;
import org.springframework.stereotype.Component;
import org.springframework.security.authentication.AuthenticationProvider;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;


@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    
	@Autowired
	private LoginValidateAuthProvider lap;


	@Override
	protected void configure(AuthenticationManagerBuilder auth) throws Exception {
		auth.authenticationProvider(lap);
	}

	
	@Override
	protected void configure(HttpSecurity http) throws Exception {
		// 设置静态资源访问权限
		http.authorizeRequests().antMatchers("/login-res/**", "/favicon.ico").permitAll();

		// 设置所有请求需要验证
		http.authorizeRequests().anyRequest().fullyAuthenticated();

        //取消iframe嵌套限制
		http.headers().frameOptions().disable();

		//自定义登录页
		//登录成功后，客户端每次请求都携带ID
        //post请求logout即可退出登录

		//form表单post提交(username，password)
		//未登录时跳转/login,登录成功跳转/index
		http.formLogin().loginPage("/login").permitAll();

		//form表单post或者ajax提交(username，password) 
		//未登录时跳转GetMapping的/login，登录成功返回AuthenticationSuccess中定义的json串,可以在页面中跳转location.href = "index";
		//http.formLogin().loginPage("/login")
		//.successHandler(new AuthenticationSuccess()).failureHandler(new AuthenticationFailure())
		//.usernameParameter("username").passwordParameter("password").permitAll().and()
		//.logout().permitAll().and().csrf().disable();

	}
}

@Component
class LoginValidateAuthProvider implements AuthenticationProvider {
	@Override
	public Authentication authenticate(Authentication authentication) throws AuthenticationException {

		String username=authentication.getName();
        String password=(String)authentication.getCredentials();
        
        //throw new BadCredentialsException("密码输入错误，请重新输入");
		
		return new UsernamePasswordAuthenticationToken(username,password);
	}

	@Override
	public boolean supports(Class<?> authentication) {
		return authentication.equals(UsernamePasswordAuthenticationToken.class);
	}
}

class AuthenticationSuccess implements AuthenticationSuccessHandler {
	@Override
	public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response,
			org.springframework.security.core.Authentication authentication) throws IOException, ServletException {
				response.setContentType("application/json;charset=utf-8");
				PrintWriter out = response.getWriter();
				out.write("{\"code\":\"200\",\"msg\":\"登录成功\"}");
				out.flush();
				out.close();
	}
}

class AuthenticationFailure implements AuthenticationFailureHandler {

	@Override
	public void onAuthenticationFailure(HttpServletRequest request, HttpServletResponse response,
			org.springframework.security.core.AuthenticationException exception) throws IOException, ServletException {
			response.setContentType("application/json;charset=utf-8");
			PrintWriter out = response.getWriter();
			out.write("{\"code\":\"500\",\"msg\":\"登录失败\"}");
			out.flush();
			out.close();

	}
	
}
```


## Controller
```
@GetMapping("/login")
public String login() {
	return "login";
}

@GetMapping("/index")
public String index() {
	return "index";
}
```

## login.html

```
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="https://www.thymeleaf.org"
      xmlns:sec="https://www.thymeleaf.org/thymeleaf-extras-springsecurity3">
    <body>
        <div th:if="${param.error}">
            Invalid username and password.
        </div>
        <div th:if="${param.logout}">
            You have been logged out.
        </div>
        <form th:action="@{/login}" method="post">
            <div><label> User Name : <input type="text" name="username"/> </label></div>
            <div><label> Password: <input type="password" name="password"/> </label></div>
            <div><input type="submit" value="Sign In"/></div>
        </form>
    </body>
</html>
```

## index.html

```
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="https://www.thymeleaf.org"
      xmlns:sec="https://www.thymeleaf.org/thymeleaf-extras-springsecurity3">
    <body>
        <h1 th:inline="text">Hello [[${#httpServletRequest.remoteUser}]]!</h1>
        <form th:action="@{/logout}" method="post">
            <input type="submit" value="Sign Out"/>
        </form>
    </body>
</html>
```