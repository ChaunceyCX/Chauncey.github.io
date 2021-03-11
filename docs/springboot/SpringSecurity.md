# Spring Security 学习

### pom

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

## 简单form登录实现

1. 配置默认用户名密码
   1. 配置文件方式
      ```yml
      spring:
        security:
          user:
            name: chauncey
            password: xcx111
      ```
   2. 配置类方式
      ```java
      @Configuration
      public class SecruityConfig extends WebSecurityConfigurerAdapter {
          
          /**
           * @despretion 加密选项
           * @author Chauncey
           * @param
           * @return org.springframework.security.crypto.password.PasswordEncoder
           * @throws
           * @date 2020/10/18 18:23
           */
          @Bean
          PasswordEncoder passwordEncoder() {
              //不对密码加密
              return NoOpPasswordEncoder.getInstance();
          }
          
          /**
           * @despretion 内存定义用户名密码
           * @author Chauncey
           * @param auth
           * @return void
           * @throws
           * @date 2020/10/18 18:23
           */
          @Override
          protected void configure(AuthenticationManagerBuilder auth) throws Exception {
              auth.inMemoryAuthentication() //内存定义用户名密码
                  .withUser("admin")
                      .password("111").roles("admin")
                      .and()  //相当于xml的节点结束标记 “/”
                      .withUser("chauncey")
                      .password("222").roles("user");
          }
      ```
1. 配置登录页
   ```java
   /**
     * @despretion 忽略静态文件
     * @author Chauncey
     * @param web
     * @return void
     * @throws
     * @date 2020/10/18 18:41
     */
    @Override
    public void configure(WebSecurity web) throws Exception {
        web.ignoring().antMatchers("/js/**","/css/**","/images/**");  //忽略静态文件 "/"路径对应resource/static
    }

    /**
     * @despretion 自定义登录页
     * @author Chauncey
     * @param http
     * @return void
     * @throws
     * @date 2020/10/18 18:41
     */
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .anyRequest().authenticated()
                .and()
                .formLogin()
                .loginPage("/login.html")
                .loginProcessingUrl("/doLogin")  //如果不定义登录接口地址默认为登录页地址：/login.html，修改之后登录验证接口就与登录页url不一样了
                .usernameParameter("name")
                .passwordParameter("pwd") // 自定义登录表单属性名
                //.defaultSuccessUrl("/index") //如果从登录地址进登录成功后跳转index，如果从其它地址进，登录成功后返回到对应地址
                //.successForwardUrl("/index") //登录成功后强制到index，与defaultSuccessUrl 只要一个即可
                .defaultSuccessUrl("/index",true) //登录成功后强制到index
                //.failureForwardUrl("/login.html") //登录失败服务端跳转
                .failureUrl("/fail") //登录失败重定向,与failureForwardUrl 只要一个就可
                .permitAll()
                .and()
                .logout()
                .logoutUrl("/doLogout") //修改默认注销url（/logout）
                .logoutRequestMatcher(new AntPathRequestMatcher("/doLogout","post")) //修改注销url及请求方式，与logoutUrl只要一个
                .logoutSuccessUrl("/index")  //注销后跳转页面
                .deleteCookies() //清楚cookies
                .clearAuthentication(true) //清除认证信息
                .invalidateHttpSession(true) // 清楚session ，默认此两项就会清楚，可不配
                .permitAll()
                .and()
                .csrf().disable();
    }
   ```
## 前后端分离的简单的无状态登录实现（非jwt）

> 有状态服务：即服务端需要记录每次会话的客户端信息，从而识别客户端身份，根据用户身份进行请求的处理，典型的设计如 Tomcat 中的 Session。例如登录：用户登录后，我们把用户的信息保存在服务端 session 中，并且给用户一个 cookie 值，记录对应的 session，然后下次请求，用户携带 cookie 值来（这一步有浏览器自动完成），我们就能识别到对应 session，从而找到用户的信息。这种方式目前来看最方便，但是也有一些缺陷，如下：
- 服务端保存大量数据，增加服务端压力
- 服务端保存用户状态，不支持集群化部署，需要支持分布式共享session
> 无状态服务：微服务集群中的每个服务，对外提供的都使用 RESTful 风格的接口。而 RESTful 风格的一个最重要的规范就是：服务的无状态性，即：
- 服务端不保存任何客户端请求者信息
- 客户端的每次请求必须具备自描述信息，通过这些信息识别客户端身份

1. 无状态登录流程
   1. 客户端：发送账号/密码到服务端认证
   2. 服务端：验证通过后，将用户信息加密编码出token，返回给客户端
   3. 客户端请求服务器资源携带token
   4. 服务端对token解密，判断token是否有效，并获取用户登录信息
2. 登录成功处理（successHandler）

## Spring Security 实现 JWT

https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247486735&idx=1&sn=4208470b30cbe5b7bbfb0ab53e6914a3&scene=21#wechat_redirect

https://mp.weixin.qq.com/s?__biz=MzI1NDY0MTkzNQ==&mid=2247488026&idx=2&sn=3bd96d91e822abf753a8e91142e036be&scene=21#wechat_redirect

https://cloud.tencent.com/developer/article/1620683