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

## ReatTemplate
> 实现 http请求

```
restTemplate.exchange(
        String url, 
        HttpMethod method,
        HttpEntity requestEntity, 
        Class responseType, 
        Object uriVariables[]
    )
```
说明:
 - url: 请求地址
 - method: 请求类型(POST,PUT,DELETE,GET)
 - requestEntity: 请求实体,封装请求头,请求内容
 - reponseType: 响应类型,根据服务接口的返回类型决定(如:list,map)
 - uriVariable: url中参数变量值

1. POST请求
    
```
String reqJsonStr = "{jsonStr}"
HttpHeaders headers = new HttpHeaders();
headers.setContentType(MediaTypeAPPLICATION_JSON);
HttpEntity<String> entity = new HttpEntity<String>(reqJsonStr,headers);
ResponseEntity<Map> resp restTemplateexchange("http://abc.com",HttpMethod.POST,entity,Map.class);
```

2. PUT请求

```
String reqJsonStr = "{\"id\":227,\"code\":\"updateCC\", \"group\":\"UPDATE\",\"content\":\"updateCT\", \"order\":9}";
HttpHeaders headers = new HttpHeaders();
headers.setContentType(MediaType.APPLICATION_JSON);
HttpEntity<String> entity = new HttpEntity<String>(reqJsonStr,headers);
ResponseEntity<Map> resp = restTemplate.exchange(DIC_DATA_URL, HttpMethod.PUT, entity, Map.class);
```

3. DELETE请求
   
```
ResponseEntity<Map> resp = restTemplate.exchange(DIC_DATA_URL + "?id={id}", HttpMethod.DELETE, null, Map.class, 227);
```

4. GET请求

```
ResponseEntity<String> results = restTemplate.exchange(url,HttpMethod.GET, null, String.class, params);
```
5. 特殊封装
   1. postForObject
   >    postForObject(String url, Object request, Class responseType, Object uriVariables[]):

    返回数据对象Object，例如：  
   ```
    DicData data = new DicData();
    data.setCode("cd123"); data.setGroup("TEST"); data.setContent("测试数据"); data.setOrder(5);    
    DicData obj = restTemplate.postForObject(DIC_DATA_URL, data, DicData.class);
    ```
    >等同于:
    >postForEntity:(String url, Object request, Class responseType, Object uriVariables[])

    返回封装了数据对象的ResponseEntity对象，例如：
    ```
    DicData data = new DicData();
    data.setCode("cd123"); data.setGroup("TEST"); data.setContent("测试数据"); data.setOrder(5);        
    ResponseEntity<Map> respEntity = restTemplate.postForEntity(DIC_DATA_URL, data, Map.class);
    ```

    2. put
    > put(String url, Object request, Object urlVariables[])

    ```
    DicData data = new DicData();
    data.setId(226L); data.setCode("updateCode"); data.setGroup("UPDATE"); 
    data.setContent("测试数据"); data.setOrder(9);      
    restTemplate.put(DIC_DATA_URL, data); 
    ```

    3. DELETE请求
    > delete(String url, Object urlVariables[])

    ```
    restTemplate.delete(DIC_DATA_URL + "?id={id}", 222);
    ```

    4. GET
    > getForObject(String url, Class responseType, Object urlVariables[])：

    ```
    Order o = restTemplate.getForObject(Constants.SERVER_URL+"/order?orderCode={orderCode}",
                        Order.class,order.getOrderCode());
    getForEntity(String url, Class responseType, Object urlVariables[])：
    返回封装了数据对象的ResponseEntity对象，例如：
    ResponseEntity<EBTUser> ebtuserResponse = restTemplate.getForEntity(url,EBTUser.class);
    EBTUser user = ebtuserResponse.getBody();
    ```

    5. GET对一些要返回复合数据的处理

        - 返回List类型数据
            ```
            DicData[] dicResult = restTemplate.getForObject( Constants.SERVER_URL + "/dicDatas/dicData?"
                    + "group={group}", DicData[].class, group);
            List<DicData> list = Arrays.asList(dicResult);
            ```
            或者:
            ```
            / pass generic information to resttemplate; ParameterizedTypeReference为spring3.2版本后引进的类
            ParameterizedTypeReference<List<DicData>> responseType = new ParameterizedTypeReference<List<DicData>>();
            ResponseEntity<List<DicData>> resp = restTemplate.exchange(Constants.SERVER_URL + "/dicDatas/dicData?group={group}", 
                HttpMethod.GET, null, responseType);
            List<DicData> list = resp.getBody();
            ```
        - 返回属性中有泛型数据的复合对象
            >比如分页对象
            ```
            ResponseEntity<String> results = restTemplate.exchange(url,HttpMethod.GET, null, String.class, params);
            // 借助com.fasterxml.jackson.databind.ObjectMapper 对象来解析嵌套的json字符串    
            ObjectMapper mapper = new ObjectMapper(); mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
            PageInfo<Product> page = mapper.readValue(results.getBody(), new TypeReference<PageInfo<Product>>() { });
            ```

