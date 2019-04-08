# SpringBoot 随笔

## CommandLineRunner

>实现在项目启动后执行的一些功能

- 直接调用run方法

```
@EnableScheduling
@SpringBootApplication
public class StartApplication {

    private static final Logger logger = LoggerFactory.getLogger(StartApplication.class);

    public static void main(String[] args) {
        SpringApplication.run(StartApplication.class, args);
    }

    @Bean
    public CommandLineRunner run(Start start) throws Exception {
        Thread.setDefaultUncaughtExceptionHandler((t, e) -> {
            logger.error(e.getMessage(), e);
            System.exit(1);
        });
        return args -> start.start();
    }
}
```

- 实现一个model,调用run
```
package org.springboot.sample.runner;
import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;

@Component
public class MyStartupRunner implements CommandLineRunner {

@Override
public void run(String... args) throws Exception {
System.out.println(">>>>>>>>>>>>>>>服务启动执行，执行加载数据等操作<<<<<<<<<<<<<");
}
}
```

- 如果多个类实现CommandLineRunner接口,如何保证顺序
  >SpringBoot会在项目启动时遍历所有实现CommandLineRunner的实体类并执行run方法,如果需要按照顺序执行需要在实体类上使用@Order注解(或实现Order接口来表明顺序)
  
```
package org.springboot.sample.runner;

import org.springframework.boot.CommandLineRunner;
import org.springframework.core.annotation.Order;
import org.springframework.stereotype.Component;


@Component
@Order(value=2)
public class MyStartupRunner1 implements CommandLineRunner {

@Override
public void run(String... args) throws Exception {
System.out.println(">>>>>>>>>>>>>>>服务启动执行 2222 <<<<<<<<<<<<<");
}

}
```

```
package org.springboot.sample.runner;

import org.springframework.boot.CommandLineRunner;
import org.springframework.core.annotation.Order;
import org.springframework.stereotype.Component;

@Component
@Order(value=1)
public class MyStartupRunner2 implements CommandLineRunner {

@Override
public void run(String... args) throws Exception {
System.out.println(">>>>>>>>>>>>>>>服务启动执行 111111 <<<<<<<<<<<<<");
}

}
```

> 控制台显示
```
>>>>>>>>>>>>>>>服务启动执行 11111111 <<<<<<<<<<<<<
>>>>>>>>>>>>>>>服务启动执行 22222222## 标题 ## <<<<<<<<<<<<<
```
> 根据控制台结果可判断，@Order 注解的执行优先级是按value值从小到大顺序。
