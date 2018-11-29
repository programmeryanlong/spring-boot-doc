# web

### 28.开发Web应用程序

**1.Spring Boot非常适合Web应用程序开发。 **你可以使用嵌入式Tomcat，Jetty，Undertow或Netty创建自包含的HTTP服务器。大多数Web应用程序使用spring-boot-starter-web模块快速启动和运行。

**28.1 SpringMVC 框架**

​	Spring Web MVC框架（通常简称为“Spring MVC”）是一个丰富的“模型视图控制器”Web框架。 Spring MVC允许您创建特殊的@Controller或@RestController bean来处理传入的HTTP请求。控制器中的方法通过使用@RequestMapping注释映射到HTTP请求。

以下代码显示了一个提供JSON数据的典型@RestController：

````java
@RestController
@RequestMapping(value="/users")
public class MyRestController {

	@RequestMapping(value="/{user}", method=RequestMethod.GET)
	public User getUser(@PathVariable Long user) {
		// ...
	}

	@RequestMapping(value="/{user}/customers", method=RequestMethod.GET)
	List<Customer> getUserCustomers(@PathVariable Long user) {
		// ...
	}

	@RequestMapping(value="/{user}", method=RequestMethod.DELETE)
	public User deleteUser(@PathVariable Long user) {
		// ...
	}

}
````

**28.1.1 Spring MVC自动配置**

Spring Boot为Spring MVC提供自动配置，适用于大多数应用程序。

自动配置在Spring的默认值之上添加了以下功能:

* 包含`ContentNegotiatingViewResolver`和`BeanNameViewResolver` 对象。
* 支持提供静态资源，包括对WebJars的支持。
* 自动注册`Converter`，`GenericConverter`和`Formatter` 对象。
* 支持`HttpMessageConverters`
* 自动注册`MessageCodesResolver`
* 静态index.html支持。
* 自定义Favicon支持
* 自动使用`ConfigurableWebBindingInitializer` 

​        如果您想保留Spring Boot提供自动配置功能，并且想要添加其他MVC配置（拦截器，格式化程序，视图控制器和其他功能），您可以添加自己的@Configuration类，类型为WebMvcConfigurer，但不要包含@EnableWebMvc。

​	如果你希望自定义实例RequestMappingHandlerMapping，RequestMappingHandlerAdapter或ExceptionHandlerExceptionResolver，则可以声明WebMvcRegistrationsAdapter实例以提供此类组件。

​	如果你想完全控制Spring MVC关闭SpringBoot提供的自动配置，可以使用@EnableWebMvc添加自己的@Configuration。

**28.1.2 HttpMessageConverters**

​	Spring MVC使用HttpMessageConverter接口来转换HTTP请求和响应。

​	如果需要添加或自定义转换器，可以使用Spring Boot的HttpMessageConverters类，如下面的清单所示：

````java
import org.springframework.boot.autoconfigure.web.HttpMessageConverters;
import org.springframework.context.annotation.*;
import org.springframework.http.converter.*;

@Configuration
public class MyConfiguration {

	@Bean
	public HttpMessageConverters customConverters() {
		HttpMessageConverter<?> additional = ...
		HttpMessageConverter<?> another = ...
		return new HttpMessageConverters(additional, another);
	}

}
````



