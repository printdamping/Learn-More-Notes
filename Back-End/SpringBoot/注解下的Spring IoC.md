# 注解下的Spring IoC

## 1. Spring IoC

IoC 是一种**通过描述来生成或者获取对象的技术**，而这个技术不是 Spring，甚至不是 Java 独有的。与传统的 new 关键字创建对象不同， Spring 是通过描述（ XML 文件或注解）来创建对象。

Spring 中把每 需要管理的对象称为 Spring Bean（简称 Bean ），而 Spring 管理这些 Bean 的容器，被我们称为 Spring IoC 容器（或者简称 IoC 容器）。 IoC 容器需要具备两个基本的功能：

- 通过描述管理 Bean ，包括发布和获取 Bean；
- 通过描述完成 Bean 之间的依赖关系，也就是依赖注入。

### 1.1 Spring IoC 容器的接口设计

Spring IoC 容器是一个管理 Bean 的容器，Spring 定义要求所有的 IoC 容器都需要实现顶级容器接口 `BeanFactory` 。

在接口`BeanFactory` 中，有多个 `getBean()` 方法，提供从 IoC 容器获取 Bean 的功能。`getBean()` 多个重载方法分别支持按类型、按名称获取 Bean ，这意味着 Spring IoC 容器允许我们按照类型或名称获取 Bean 。

![image-20210827151243544](C:/Users/z50010162/Documents/LearningNotes/Back-End/SpringBoot/%E6%B3%A8%E8%A7%A3%E4%B8%8B%E7%9A%84Spring%20IoC.assets/image-20210827151243544.png)

`BeanFactory` 接口提供的功能比较基础，不能满足 Spring IoC 容器的需要，因此 Spring 在 `BeanFactory` 接口的基础上，设计了一个更为高级的接口 `ApplicationContext` 。

![img](C:/Users/z50010162/Documents/LearningNotes/Back-End/SpringBoot/%E6%B3%A8%E8%A7%A3%E4%B8%8B%E7%9A%84Spring%20IoC.assets/887326-20171105095449638-1613744939-16300485412574.png)

如上图，`ApplicationContext` 除了间接继承 `BeanFactory` 接口外，还扩展了消息国际接口 `MessageSource` 、环境可配置接口  `EnvironmentCapable` 、 应用事件发布接口 `ApplicationEventPublish` 和资源模式解析接口 `ResourcePatternResolver` 。

Spring Boot 中推荐使用注解装配 Bean 到 Spring IoC 容器中，因此不再涉及 XML 相关的 IoC 容器，我们主要关注基础注解的 IoC 容器—— `AnnotationConfigApplicationContext` 。

### 1.2 Spring IoC 容器装配简单对象

定义 Java 对象：

```java
@Component("user")
public class User {
    private Long id;
    private String username;
    private String password;
    
    /* setter and getter */
}
```

定义 Java 配置文件：

```java
/* 通过@Bean注解，将对应方法返回的POJO装配到IoC容器中 */
@Configuration
public class AppConfig {
    @Bean("user")
    public User user() {
        User user = new User();
        user.setId(1L);
        user.setUsername("user_name");
        user.setPassword("password");
        return user;
    }
}
```

通过 `@Bean` 注解将 Bean 注入到 Spring IoC 容器中，需要在配置文件 AppConfig 中建立对应的方法，非常不方便。因此，Spring 提供了批量扫描装配的方法，使用 `@Component` 和 `@ComponentScan` 注解。 `＠Component` 是标明哪个类被扫描进入 Spring IoC 容器，而`＠ComponentScan` 是标明采用何种策略去扫描装配 Bean 。

```java
@Component("user")
public class User {
    @Value("1L")
    private Long id;
    @Value("user_name")
    private String username;
    @Value("password")
    private String password;
    
    /* setter and getter */
}

@Configuration
@ComponentScan
public class AppConfig {
}
```



