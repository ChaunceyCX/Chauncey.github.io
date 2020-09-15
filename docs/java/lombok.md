# lombok使用

## IDEA 配置

## Maven依赖

```
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.8</version>
    <scope>provided</scope>
</dependency>
```

## 常用注解

1. @EqualsAndHashCode：实现equals()方法和hashCode()方法
2. @ToString：实现toString()方法 
3. @Data ：注解在类上；提供类所有属性的 get 和 set 方法，此外还提供了equals、canEqual、hashCode、toString 方法 
4. @Setter：注解在属性上；为属性提供 set 方法 
5. @Getter：注解在属性上；为属性提供 get 方法 
6. @Log4j ：注解在类上；为类提供一个 属性名为 log 的 log4j 日志对象 
7. @NoArgsConstructor：注解在类上；为类提供一个无参的构造方法 
8. @AllArgsConstructor：注解在类上；为类提供一个全参的构造方法 
9. @Cleanup：关闭流
10. @Synchronized：对象同步
11. @SneakyThrows：抛出异常