# web.xml

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>
  
  <welcome-file-list>
    <welcome-file>/WEB-INF/pages/login.jsp</welcome-file>
  </welcome-file-list>

  <servlet>
    <servlet-name>spring</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:spring-*.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>spring</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
<!--配置druid的监控管理页面-->
  <servlet>
    <servlet-name>DruidStatView</servlet-name>
    <servlet-class>com.alibaba.druid.support.http.StatViewServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>DruidStatView</servlet-name>
    <url-pattern>/druid/*</url-pattern>
  </servlet-mapping>



  <filter>
    <filter-name>encoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>utf-8</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>

  <filter>
    <filter-name>shiroFilter</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    <init-param>
      <param-name>targetFilterLifecycle</param-name>
      <param-value>true</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>shiroFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>

</web-app>
```

# spring-mvc.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd
         http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <context:component-scan base-package="com.wangyi.controller"/>
    <mvc:default-servlet-handler />
    <aop:aspectj-autoproxy proxy-target-class="true"/>

    <mvc:annotation-driven>
        <mvc:message-converters>
            <bean class="com.alibaba.fastjson.support.spring.FastJsonHttpMessageConverter">
                <property name="supportedMediaTypes" value="application/json;charset=utf-8"/>
            </bean>
        </mvc:message-converters>
    </mvc:annotation-driven>
    <mvc:resources mapping="/images/**" location="/WEB-INF/images/"/>
    <mvc:resources mapping="/js/**" location="/WEB-INF/js/"/>
    <mvc:resources mapping="/css/**" location="/WEB-INF/css/"/>
    <mvc:resources mapping="/layer/**" location="/WEB-INF/layer/"/>

    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"/>

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/pages/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>

```

# spring-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd
         http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <context:property-placeholder location="classpath:db.properties"/>
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
        <property name="initialSize" value="${jdbc.initialSize}"/>
    </bean>
    <bean id="factory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <property name="typeAliasesPackage" value="com.wangyi.pojo"/>
        <property name="mapperLocations">
            <list>
                <value>classpath:com/wangyi/mapper/*.xml</value>
            </list>
        </property>
        <property name="plugins">
            <list>
                <bean class="com.github.pagehelper.PageInterceptor">
                    <property name="properties">
                        <value>
                        </value>
                    </property>
                </bean>
            </list>
        </property>
    </bean>
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.wangyi.dao"/>
        <property name="sqlSessionFactoryBeanName" value="factory"/>
    </bean>
</beans>
```

# spring-service.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd
         http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <context:component-scan base-package="com.wangyi.services"/>
    <context:component-scan base-package="com.wangyi.pojo"/>
    <context:component-scan base-package="com.wangyi.test.testWeb"/>
    <context:component-scan base-package="com.wangyi.aspect"/>

    <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <aop:aspectj-autoproxy proxy-target-class="true"/>

    <tx:advice id="txAdvice" transaction-manager="txManager">
        <tx:attributes>
            <tx:method name="sel*" read-only="true"/>
            <tx:method name="upd*"/>
            <tx:method name="ins*"/>
            <tx:method name="del*"/>
        </tx:attributes>
    </tx:advice>
    
    <aop:config>
        <aop:pointcut id="mypoint" expression="execution(* com.wangyi.services.impl.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="mypoint"/>
    </aop:config>
</beans>
```

# spring-shiro.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd
         http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">


    <bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">
        <property name="cacheManager" ref="cacheManager"/>
        <property name="authenticator" ref="authenticator"/>
        <property name="realms">
            <list>
                <ref bean="userRealm"/>
            </list>
        </property>
    </bean>

    <bean id="cacheManager" class="org.apache.shiro.cache.ehcache.EhCacheManager">
        <property name="cacheManagerConfigFile" value="classpath:ehcache.xml"/>
    </bean>

    <bean id="authenticator" class="org.apache.shiro.authc.pam.ModularRealmAuthenticator">
        <property name="authenticationStrategy">
            <bean class="org.apache.shiro.authc.pam.FirstSuccessfulStrategy"/>
        </property>
    </bean>


    <bean id="userRealm" class="com.wangyi.shiro.realm.UserRealm">

        <property name="authenticationCacheName" value="bos"/>

        <property name="credentialsMatcher">
            <bean class="org.apache.shiro.authc.credential.HashedCredentialsMatcher">
                <property name="hashAlgorithmName" value="MD5"/>
                <property name="hashIterations" value="1024"/>
            </bean>
        </property>
    </bean>

    <bean id="lifecycleBeanPostProcessor" class="org.apache.shiro.spring.LifecycleBeanPostProcessor"/>

    <bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator" depends-on="lifecycleBeanPostProcessor"/>
    <bean class="org.apache.shiro.spring.security.interceptor.AuthorizationAttributeSourceAdvisor">
        <property name="securityManager" ref="securityManager"/>
    </bean>

    <bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
        <property name="securityManager" ref="securityManager"/>
        <property name="loginUrl" value="/"/>
        <property name="successUrl" value="/"/>
        <property name="unauthorizedUrl" value="/unauthorized"/>
        <property name="filterChainDefinitions">
            <value>
                /user/logout = logout
                /user/** = anon
                /manager/** = authc
                /teacher/** = authc
                /category/** = authc
            </value>
        </property>
    </bean>
</beans>

```

# mapper.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!--mapper的dtd  -->
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wangyi.dao.TeacherDao">
    <resultMap id="teacher" type="com.wangyi.pojo.Teacher">
        <id property="id" column="id"/>
        <result property="name" column="name"/>
        <result property="username" column="username"/>
        <result property="password" column="password"/>
        <result property="sex" column="sex"/>
        <result property="phone" column="phone"/>
        <result property="email" column="email"/>
        <result property="birthday" column="birthday"/>
        <result property="entryday" column="entrydate"/>
    </resultMap>
    <select id="selAll" resultMap="teacher">
        select * from teacher
        <where>
            <trim prefixOverrides="and" suffixOverrides=",">
                <if test="searchType.object_name!=null and searchType.object_name!=''">
                    <choose>
                        <when test="searchType.object_name=='name'">
                            and name like concat('%',#{searchType.searchMessage},'%'),
                        </when>
                        <when test="searchType.object_name=='username'">
                            and username like concat('%',#{searchType.searchMessage},'%'),
                        </when>
                        <when test="searchType.object_name=='phone'">
                            and phone like concat('%',#{searchType.searchMessage},'%'),
                        </when>
                        <when test="searchType.object_name=='birthday'">
                            and birthday like concat('%',#{searchType.searchMessage},'%'),
                        </when>
                        <when test="searchType.object_name=='email'">
                            and email like concat('%',#{searchType.searchMessage},'%'),
                        </when>
                        <when test="searchType.object_name=='entryday'">
                            and entrydate like concat('%',#{searchType.searchMessage},'%'),
                        </when>
                    </choose>
                </if>
            </trim>
        </where>
    </select>
    <insert id="insTeacher">
            INSERT INTO `teacher` (`name`, `sex`, `entrydate`) VALUES (#{name}, #{sex}, #{entryday})
    </insert>

    <delete id="delTeacher">
        delete from teacher where (id=#{param1})
    </delete>

    <update id="updTeacher">
        update teacher
        <set>
            <trim suffixOverrides=",">
                <if test="name!=null and name!=''">
                    name = #{name},
                </if>
                <if test="entryday!=null">
                    entrydate = #{entryday},
                </if>
                <if test="sex!=null">
                    sex = #{sex},
                </if>
            </trim>
        </set>
        where id = #{id};
    </update>
</mapper>

```

