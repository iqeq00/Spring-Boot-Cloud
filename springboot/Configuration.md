# @Configuration

所在包：org.springframework.context.annotation

所在 jar：spring-context.jar（5.1.6.RELEASE）

## 源码

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Configuration {

	/**
	 * Explicitly specify the name of the Spring bean definition associated with the
	 * {@code @Configuration} class. If left unspecified (the common case), a bean
	 * name will be automatically generated.
	 * <p>The custom name applies only if the {@code @Configuration} class is picked
	 * up via component scanning or supplied directly to an
	 * {@link AnnotationConfigApplicationContext}. If the {@code @Configuration} class
	 * is registered as a traditional XML bean definition, the name/id of the bean
	 * element will take precedence.
	 * @return the explicit component name, if any (or empty String otherwise)
	 * @see org.springframework.beans.factory.support.DefaultBeanNameGenerator
	 */
	@AliasFor(annotation = Component.class)
	String value() default "";

}
```

标识一个 java 类声明了一个或者多个 @Bean 方法，而且可以被 Spring 的容器进行处理，生成 Bean 的定义，以及针对于运行期的 Bean 服务请求。

## Java Config 实现方式

```java
@Configuration
   public class AppConfig {
  
       @Bean
       public MyBean myBean() {
           // instantiate, configure and return bean ...
       }
   }
```

@Configuration 类通常是要么通过 AnnotationConfigApplicationContext ，要么通过 AnnotationConfigWebApplicationContext 来启动的。

针对之前的代码的一个简单实例：

```java
AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext();
   ctx.register(AppConfig.class);
   ctx.refresh();
   MyBean myBean = ctx.getBean(MyBean.class);
   // use myBean ...
```

首先创造一个 AnnotationConfigApplicationContext 应用上下文的实例，接着把 Java Config 配置类注册到应用上下文当中，刷新应用上下文以后就可以通过 getBean 方法获取（从 Spring 容器内）之前定义好的 MyBean 实例，根据后面的请求，进行 MyBean 操作。

**这也解释了，我们在 Spring Boot 应用当中，所有的 Java 配置类，怎么就能被 Spring Boot 或者 Spring 找到的。**

## xml 实现方式

```xml
<beans>
    <context:annotation-config/>
    <bean class="com.acme.AppConfig"/>
</beans>
```

传统方式，在 Java Config 兴起以前，当时的标准就是使用 xml 来实现，
