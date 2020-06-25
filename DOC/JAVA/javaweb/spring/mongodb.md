# mongodb

## application.properties

mogodb无用户名密码，数据库名为jyl  
无mongodb数据库需要删除相关代码

spring.data.mongodb.uri=mongodb://localhost:27017/jyl


## POM

```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
```

## domain

```
MongoInfo.java


import org.springframework.data.annotation.Id;

public class MongoInfo {//对应collection的名称为MongoInfo
	@Id
	private Long id;
	private String username;
	private Integer age;

	public MongoInfo(Long id, String username, Integer age) {
		this.id = id;
		this.username = username;
		this.age = age;
	}
	
	
	public Long getId() {
		return id;
	}
	public void setId(Long id) {
		this.id = id;
	}
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public Integer getAge() {
		return age;
	}
	public void setAge(Integer age) {
		this.age = age;
	}
}
```

## Controller

```
@Autowired
private MongoInfoRepository repository;
@RequestMapping(value = "/mo", method = RequestMethod.GET)
MongoInfo mo() {
	repository.save(new MongoInfo(1L, "abc", 10));
	// MongoInfo u = repository.findByUsername("abc");
	MongoInfo u = repository.findByAge(10);
	// u.setAge(110);
	// repository.save(u);
	// int count = repository.findAll().size();
	// repository.deleteByAge(10);
	return u;
}
```

## MongoInfoRepository.java

```
import org.springframework.data.mongodb.repository.MongoRepository;

public interface MongoInfoRepository extends MongoRepository<MongoInfo, Long> { 
	MongoInfo findByUsername(String username);
	MongoInfo findByAge(Integer a);
	
	void deleteByAge(Integer a);
}
```