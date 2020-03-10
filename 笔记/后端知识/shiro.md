# 基础知识

[权限架构的学习](https://www.cnblogs.com/jpfss/p/11210694.html)

[拦截器如何写：](https://my.oschina.net/tij/blog/1929288)

[如何退出](https://blog.csdn.net/weixin_42794011/article/details/88667527)

1.编写realm认证策略

2.配置bean

3.登录时验证

```java
Subject subject = SecurityUtils.getSubject();
UsernamePasswordToken token = new UsernamePasswordToken(user.getUsername(), user.getPassword());
try {
    subject.login(token);
    Cookie cookie = new Cookie("login",token.toString());
    session.setAttribute("user", token);
    response.addCookie(cookie);
} catch (AuthenticationException | AuthorizationException e) {
    if (e instanceof AuthenticationException) {
        model.addAttribute("returnMessage", "账号或密码错误");
    } else {
        model.addAttribute("returnMessage", "没有权限");
    }
    return "manager/login";
}
return "redirect:/manager/index";

```



# 重写shiro提供的过滤器

[教程](https://blog.csdn.net/qq_34021712/article/details/84722252)

注意重写的过滤器不能加上@bean注解，否则会被spring代理 



# 自定义realm操作对应的登录验证规则

[教程](https://blog.csdn.net/xiangwanpeng/article/details/54802509)

shiro默认的是使用所有配置的的realm进行验证

实现单独的验证需要重写ModularRealmAuthenticator子类并配置到SecurityManager中



# spring boot中开启shiro的注解

<https://www.jianshu.com/p/ae70d2b1a568>

ssm中是配置aop为cglib代理即可

springboot中需要在配置类中加入：

```java
@Bean
public AuthorizationAttributeSourceAdvisor authorizationAttributeSourceAdvisor(@Qualifier("defaultWebSecurityManager")DefaultWebSecurityManager defaultWebSecurityManager) {
    AuthorizationAttributeSourceAdvisor advisor = new AuthorizationAttributeSourceAdvisor();
    advisor.setSecurityManager(defaultWebSecurityManager);
    return advisor;
}
@Bean
@ConditionalOnMissingBean
public DefaultAdvisorAutoProxyCreator defaultAdvisorAutoProxyCreator(){
    DefaultAdvisorAutoProxyCreator app=new DefaultAdvisorAutoProxyCreator();
    app.setProxyTargetClass(true);
    return app;

}

```

