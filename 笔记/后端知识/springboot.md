# springboot2.0

### 添加拦截器后静态资源不生效 

[原因](https://blog.csdn.net/qq_20757489/article/details/89462052)

在2.x版本后springboot默认不拦截静态资源，需要在注册拦截器时手动添加静态资源的目录

```java
registry.addInterceptor(new LoginHandlerInterceptor()).addPathPatterns("/**")
.excludePathPatterns("/","/index","/check","/images/**");
```

## 发送put，delete请求失败

[原因](https://blog.csdn.net/c2861024198/article/details/102807710)

springBoot默认关闭了MVC的HiddenHttpMethodFilter，因此需要打开。

```java
spring.mvc.hiddenmethod.filter.enabled=true
```

## 不执行数据库脚本

[原因](https://blog.csdn.net/qq_34054957/article/details/83016411)

springboot2.0之后需要设置

```java
spring.datasource.initialization-mode=always
```

# Springboot对事物的支持

[教程](https://www.cnblogs.com/sharpest/p/7995203.html)

失效的场景：

1.抛出异常，默认只处理运行时异常，对于需要手动处理的异常不会进行事物回滚：

解决方法：@Transactional(rollbackFor = Exception.class)

2.只能对public方法使用

3.方法出现内部调用：在类中的a方法调用了类中另一个被注解的b方法

# springboot中实现aop

[教程](https://blog.csdn.net/qq_33257527/article/details/82561635)



# 自定义属性的配置和环境

```java
@ConfigurationProperties(prefix = "swagger")
```

读取swagger下的同名配置，注意需要set方法注入

```java
@Profile
```

配置在哪个环境下才读取配置类



# 任务

## 异步任务

1.在启动类上加上@EnableAsync

2.在方法上加上@Async注解

 

## 定时任务

1.启动类上加上@EnableScheduling

2.在方法上加上@Scheduled(cron="0 * * * * *")

second, minute, hour, day of month, month, and day of week（0-7 SUN-SAT）

 

<https://blog.csdn.net/smiling_Z/article/details/82828195>

## 邮件任务

1.引入pom文件

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

2.配置属性：需要去自己的邮箱开启支持服务

```yml
spring:
  mail:
    username: 2928464591@qq.com
    password: vylxryeadhfddffe
    host: smtp.qq.com
    properties:
      mail:
        smtp:
          ssl:
            enable: true

```

 

3.注入操作类完成相关操作

```java
@Autowired
JavaMail  SendermailSender;
```

# 常见类的编写

## 1.拦截器，用于拦截特定请求

```java
public class LoginHandlerInterceptor implements HandlerInterceptor {
    /*
    * 目标方法执行前*/
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        LogUtils.getLogger().info("进入了登录拦截器");
        LogUtils.getLogger().info(request.getContextPath());
        Object username = request.getSession().getAttribute("username");
        if (username == null) {
            request.getRequestDispatcher("/").forward(request, response);
            return false;
        } else {
            return true;
        }
    }
    /*
	 * 请求处理完成之后*/
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
    }
    /*
    * 请求完全完成之后*/
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
    }
}
```

## 2.本地化配置器

```java
public class MyLocaleResolver implements LocaleResolver {
    /*
    * 解析请求中携带的区域信息参数*/
    @Override
    public Locale resolveLocale(HttpServletRequest httpServletRequest) {
        //获取自定义请求头信息，l的参数值
        String l=httpServletRequest.getParameter("l");
        //获取系统的默认区域信息
        Locale locale=Locale.getDefault();
        if (!StringUtils.isEmpty(l)){
            String[] split=l.split("_");
            //接收的第一个参数为：语言代码，国家代码
            locale=new Locale(split[0],split[1]);
        }
        return locale;
    }
    @Override
    public void setLocale(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Locale locale) {
    }
}

```

# 自定义的配置文件

## 1.自定义扩展springmvc配置，类似于以前的spring-mvc.xml文件

```java
@Configuration
public class MySpringMVCConfig implements WebMvcConfigurer {
    /*
    * 添加路径*/
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/main").setViewName("login/success");
        registry.addViewController("/index").setViewName("index");
    }
    /*
    * 添加自定义的拦截器*/
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginHandlerInterceptor()).addPathPatterns("/**")
           .excludePathPatterns("/","/index","/check","/asserts/**");
    }
    /*
    * 添加自定义的区域信息解析器*/
    @Bean
    public LocaleResolver localeResolver() { //注意名字相同，否则不生效 
        return new MyLocaleResolver();
    }
}
```



## 2.自定义服务器配置，类似于以前的web.xml文件，常见配置可以去yml文件配置

```java
//自定义的一些类
import com.wangyi.servlet.MyFliter;
import com.wangyi.servlet.MyListener;
import com.wangyi.servlet.MyServlet;
@Configuration
public class MyServerConfig {
//配置servlet
    @Bean
    public ServletRegistrationBean<MyServlet> myServlet() {
        return new ServletRegistrationBean<>(new MyServlet(), "/test");
    }
//配置Filter
    @Bean
    public FilterRegistrationBean<MyFliter> myFilter(){
        FilterRegistrationBean<MyFliter> registrationBean = new FilterRegistrationBean<>();
        registrationBean.setFilter(new MyFliter());
        registrationBean.setUrlPatterns(Arrays.asList("/test"));
        return registrationBean;
    }
//配置listener
    @Bean
    public ServletListenerRegistrationBean<MyListener> myListener() {
        return new ServletListenerRegistrationBean<>(new MyListener());
    }
}

```

# swagger配置

<https://blog.csdn.net/moshowgame/article/details/80265480>

1.导入依赖

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
```

2.创建配置java类

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    /*
     * @Author Wrysunny
     * @Description //TODO 配置swagger的bean实例
     * @Date 16:34 2020/1/5
     * @Param []
     * @return springfox.documentation.spring.web.plugins.Docket
     **/
    @Bean
    public Docket docker(Environment environment) {
        boolean flag = environment.acceptsProfiles(Profiles.of("dev", "test"));
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .groupName("TestController")
                .enable(flag)
                .select()                		.apis(RequestHandlerSelectors.basePackage("com.wangyi.spring_boot_swapper.demo.controller"))//配置扫描的包
                .paths(PathSelectors.any())                              //配置需要被过滤的路径
                .build();
    }
    @Bean
    public Docket docker1(Environment environment) {
        boolean flag = environment.acceptsProfiles(Profiles.of("dev", "test"));
        return new Docket(DocumentationType.SWAGGER_2)
//                .apiInfo(apiInfo())
                .groupName("UserController")
                .enable(flag)
                .select()            .apis(RequestHandlerSelectors.basePackage("com.wangyi.spring_boot_swapper.demo.controller"))
                .paths(PathSelectors.any())
                .build();
    }
    private ApiInfo apiInfo(){
//        作者信息
        Contact contact = new Contact("wangyi","https://github.com/","2928464591@qq.com");
        return new ApiInfo("API文档",
                "个人测试的API",
                "v0.1",
                "https://github.com/",
                contact,
                "apache 2.0",
                "http://www.apache.org/licenses/LICENSE-2.0",
                new ArrayList<>());
    }

}

 

```

3.访问[http://localhost:8080/swagger-ui.html



# springboot配置druid

## 1.编写java配置文件

```
public class DruidConfig {
    /*
     * @Author Wrysunny
     * @Description //TODO 配置druid读取yml文件的属性
     * @Date 13:27 2020/1/4
     * @Param []
     * @return javax.sql.DataSource
     **/
    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource druid(){
        return new DruidDataSource();
    }
    /*
     * @Author Wrysunny
     * @Description //TODO 配置druid的监控页面
     * @Date 13:27 2020/1/4
     * @Param []
     * @return org.springframework.boot.web.servlet.ServletRegistrationBean<com.alibaba.druid.support.http.StatViewServlet>
     **/
    @Bean
    public ServletRegistrationBean<StatViewServlet> statViewServlet(){
        ServletRegistrationBean<StatViewServlet> bean = new ServletRegistrationBean<>(new StatViewServlet(),"/druid/*");
        Map<String, String> initParams = new HashMap<>();
        initParams.put("loginUsername","admin");
        initParams.put("loginPassword","123");
        initParams.put("allow","");//默认就是允许所有访问
        initParams.put("deny","192.168.15.21");
        bean.setInitParameters(initParams);
        return bean;
    }
    /*
     * @Author Wrysunny
     * @Description //TODO 配置druid的过滤器
     * @Date 13:28 2020/1/4
     * @Param []
     * @return org.springframework.boot.web.servlet.FilterRegistrationBean<com.alibaba.druid.support.http.WebStatFilter>
     **/
    @Bean
    public FilterRegistrationBean<WebStatFilter> webStatFilterFilter(){
        FilterRegistrationBean<WebStatFilter> bean = new FilterRegistrationBean<>(new WebStatFilter());
        Map<String, String> initParams = new HashMap<>();
        initParams.put("exclusions",".js,.css,/druid/*");
        bean.setInitParameters(initParams);
        bean.setUrlPatterns(Collections.singletonList("/*"));
        return bean;
    }
}
```



## 2.去配置文件配置自己需要的属性

```java
spring:
  datasource:
    #   数据源基本配置
    username: root
    password: WANGYI
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/spring_boot_ssm?serverTimezone=UTC
    type: com.alibaba.druid.pool.DruidDataSource
    #   数据源其他配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true
#   配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙
#    ,wall,log4j
    filters: stat
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
```

# springboot配置Redis

[ssm](https://blog.csdn.net/Evan_QB/article/details/82622755https://blog.csdn.net/qq_42605968/article/details/92759510https://blog.csdn.net/lizhengyu891231/article/details/88598781)

[jedis和lettuce的区别](https://blog.csdn.net/tianyaleixiaowu/article/details/89847286)

 

1.导入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.61</version>
</dependency>
```

如果使用letture作为redis客户端工具还需要导入

```xml
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-pool2</artifactId>
    <version>2.5.0</version>
</dependency>
```

 

2.配置文件属性

```java
spring:
  redis:
    host: localhost
    port: 6379
    password: WANGYI
    lettuce:
      pool:
        max-active: 8
        max-wait: -1
        max-idle: 500
        min-idle: 0
      shutdown-timeout: 0
```

 

 

3.使用fastjson作为序列化器（新版的fastjson提供了FastJsonRedisSerializer来作为redis的序列化器，可以直接使用）

```java
public class FastJsonRedisSerializer<T> implements RedisSerializer<T> {
    public static final Charset DEFAULT_CHARSET = StandardCharsets.UTF_8;
    private Class<T> clazz;
    public FastJsonRedisSerializer(Class<T> clazz) {
        super();
        this.clazz = clazz;
    }
    @Override
    public byte[] serialize(T t) throws SerializationException {
        if (null == t) {
            return new byte[0];
        }
        return JSON.toJSONString(t, SerializerFeature.WriteClassName).getBytes(DEFAULT_CHARSET);
    }
    @Override
    public T deserialize(byte[] bytes) throws SerializationException {
        if (null == bytes || bytes.length <= 0) {
            return null;
        }
        String str = new String(bytes, DEFAULT_CHARSET);
        return (T) JSON.parseObject(str, clazz);
    }
}
```

4.编写自定义的序列化器

```java
@Configuration
public class RedisConfig {
    @Bean
    public RedisTemplate<String, User> userRedisTemplate(RedisConnectionFactory factory){
        RedisTemplate<String, User> template = new RedisTemplate<>();
        FastJsonRedisSerializer fastJsonRedisSerializer = new FastJsonRedisSerializer(User.class);
        //关联
        template.setConnectionFactory(factory);
        template.setValueSerializer(fastJsonRedisSerializer);
        template.setHashValueSerializer(fastJsonRedisSerializer);
        // key的序列化采用StringRedisSerializer
        template.setKeySerializer(new StringRedisSerializer());
        template.setHashKeySerializer(new StringRedisSerializer());
        return template;
    }
}
```

5.使用自定义的类注入

```java
@Autowired
private RedisTemplate<String, User> redisTemplate;
```



# 文件上传

前端使用FormData:formdata是以name-value模拟键值对返回数据

ajax提交：

```javascript
$.ajax({
    url: '[[@{/image/upload}]]', // 图片上传url
    type: 'POST',
    data: imageData,
    cache: false,
    contentType: false,
    processData: false,
    dataType: 'json',  
}
```

注意不要对请求头和数据进行处理

在配置类中实现项目外部地址的映射

```java
@Configuration
public class SpringMVCConfig implements WebMvcConfigurer {
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {      registry.addResourceHandler("/upload/**").addResourceLocations("file:H://upload/");
    }
}
```

# springboot关于rabbitmq的配置

[学习链接](https://www.cnblogs.com/linyufeng/p/9885645.html)

1.导入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

 

2.注入rabbitTemplate，所有的操作都是基于此对象完成

```java
@Autowired
private RabbitTemplate rabbitTemplate;
```

 

3.配置json解析器

@Bean

```java
public MessageConverter messageConverter(){
    return new Jackson2JsonMessageConverter();
}
```
4.配置监听

启动类加上注解@EnableRabbit

在需要监听的地方编写方法



```java
@RabbitListener(queues = "a.yi")
public void receive1(Message message,Map<String,Object> map){
    System.out.println("收到消息"+ message.getBody());
    System.out.println(message.getMessageProperties());
    System.out.println(map.toString());
}
```
5.管理组件

注入



# springboot整合elasticsearch

[项目启动报错](https://www.jianshu.com/p/4d6bedded895https://blog.csdn.net/weixin_44408925/article/details/103530652)

[自定义方法要遵守的](https://docs.spring.io/spring-data/elasticsearch/docs/3.2.3.RELEASE/reference/html/#elasticsearch.repositories)

 

1.添加pom依赖，yml文件配置基本属性

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
 </dependency>
```

```java
spring:
  data:
    elasticsearch:
      cluster-nodes: 127.0.0.1:9300
```

2.启动类上加入（未解之谜）

```java
System.setProperty("es.set.netty.runtime.available.processors", "false");
```

3.创建实体类

```java
@Document(indexName = "xinhuashudian",type = "book")
public class Book {
    private int id;
    private String name;
    private String author;
}
```

4.创建操作接口

```java
public interface BookRepository extends ElasticsearchRepository<Book, Integer> {
//    扩展的方法需要遵守一定的命名规则
    List<Book> findByNameLike(String name);
}
```

5.注入实例后使用

6.获取数据

<http://localhost:9200/xinhuashudian/book/_search>