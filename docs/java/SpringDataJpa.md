# SpringDataJPA

## JPA

>JPA是JAVA开发人员提供的一种对象/关系映射工具,JPA是一套规范

## Spring Data JPA

>Spring Data JPA是Spring基于ORM框架/JPA规范的基础上封装的一套JPA应用框架

### 配置
```
//maven
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
            
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

//application.properties
spring.datasource.url=jdbc:mysql://*:*/test?useUnicode=true&characterEncoding=utf-8&serverTimezone=UTC&useSSL=true
spring.datasource.username=
spring.datasource.password=
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.jpa.properties.hibernate.hbm2ddl.auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
spring.jpa.show-sql=true

```
1. spring.jpa.properties.hibernate.hbm2ddl.auto有以下几种配置:
   
   - create:每次加载都会删除上一次生成的表(包括数据),然后重新生成新表,适用于每次执行测试前清空数据库的场景
   - creat-drop:每次加载都会生成表,但是SessionFactory关闭时所生成的表自动删除
   - update:最常用的属性,第一次加载时创建数据表(前提是已经指定数据库),以后加载时不会删除上一次生成的表,会根据实体更新只新增字段不会删除字段,即使实体中已经删除
   - validate:每次加载都会验证数据表结构,只会和已经存在的表进行比较,根据model修改表结构,但不会创建新表
   - 不配置此项表示禁用自动建表功能

2. Respository

- 建立entity
```
@Entity
@Data  //lombok 自动set/get
public class User {

    @Id
    @GeneratedValue
    private long id;
    @Column(nullable = false, unique = true)
    private String userName;
    @Column(nullable = false)
    private String password;
    @Column(nullable = false)
    private int age;
}
```
- 声明对应Repository接口,继承JpaRepository,默认支持CRUD操作
```
public interface UserRepository extends JpaRepository<User, Long> {

    User findByUserName(String userName);

}
```

- 测试
```
@Autowired
private UserRepository userRepository;

@Test
@Transactional //开启事务功能是为了单元测试的时候不造成垃圾数据
public void userTest() {
    User user = new User();
    user.setUserName("wyk");
    user.setAge(30);
    user.setPassword("aaabbb");
    userRepository.save(user);
    User item = userRepository.findByUserName("wyk");
    log.info(JsonUtils.toJson(item));
}
```
3. 实现原理

对测试进行debug,可以发现userRepository被注入了一个动态代理,被代理的类是JpaRepository

