## Component

```
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class Info {
	@Value("${jyl.mo.name}")
	private String name;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
}



对应application.properties中字段：
jyl.mo.a=123
jyl.mo.b=456
jyl.mo.name=${jyl.mo.a}abc${jyl.mo.b}

使用时：
//	@Autowired
//	Info inf;
```