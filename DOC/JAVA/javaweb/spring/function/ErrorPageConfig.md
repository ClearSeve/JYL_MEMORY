# ErrorPageConfig

```
import org.springframework.boot.web.server.ErrorPage;
import org.springframework.boot.web.server.ErrorPageRegistrar;
import org.springframework.boot.web.server.ErrorPageRegistry;
import org.springframework.http.HttpStatus;
import org.springframework.stereotype.Component;

@Component
public class ErrorPageConfig implements ErrorPageRegistrar {

	@Override
	public void registerErrorPages(ErrorPageRegistry registry) {
		ErrorPage error404Page = new ErrorPage(HttpStatus.NOT_FOUND, "/error404");
		ErrorPage error500Page = new ErrorPage(HttpStatus.INTERNAL_SERVER_ERROR,"/error500");
		ErrorPage error403Page = new ErrorPage(HttpStatus.FORBIDDEN,"/error403");
		registry.addErrorPages(error403Page,error404Page,error500Page);
		
	}

}



对应Controller中：
@GetMapping("/error404")
public String error404Page() {
	return "error-page/404";
}
@GetMapping("/error403")
public String error403Page() {
	return "error-page/403";
}

@GetMapping("/error500")
public String error500Page() {
	return "error-page/500";
}
```