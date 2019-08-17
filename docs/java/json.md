# object/json 互转(jackson)

## 使用jackson的ObjectMapper实现互转

1. 导入maven依赖

```
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-core</artifactId>
</dependency>
```

2. 创建ObjectMapper对象,并调用相关API

```
@Autowired
private ObjectMapper objectMapper;
@Autowired
User user;

public void transfer(String jsonStr) {
    user = objectMapper.readValue( jsonStr, User.class);
    jsonStr = objectMapper.writeValueAsString(user);
}
```

## 对于Java对象转换时,属性首字母大小写问题

> 使用@JsonProperity(value = ("首字母大写的jsonkey "))解决

```
@Data
public class User() {
    @JsonPropertity( value = " Id " )
    private Long id;
    ....
}
```




