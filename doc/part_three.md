# Spring Boot外部配置

**24.0 .**   Spring Boot允许您外部化配置，以便您可以在不同的环境中使用相同的应用程序代码。您可以使用属性文件（`properties`文件），`YAML`文件，环境变量和命令行参数来外部化配置。可以使用`@Value`注释将属性值直接注入到bean中，通过Spring的`Environment`抽象访问，或者通过`@ConfigurationProperties`绑定到结构化对象。

**SpringBoot按以下顺序使用外部配置**

1. :large_orange_diamond:  在您的主目录上开发全局设置属性（当devtools处于活动状态时，〜/ .spring-boot-devtools.properties）。
2. :large_orange_diamond: 测试中的`@TestPropertySource`。
3. :large_orange_diamond:测试中的属性。可在`@SpringBootTest`上使用，以及用于测试应用程序特定片段的测试注释。
4. :large_orange_diamond:命令行参数。
5. :large_orange_diamond:`SPRING_APPLICATION_JSON`中的属性（嵌入在环境变量或系统属性中的内联JSON）。
6. :large_orange_diamond:`ServletConfig`初始化参数。
7. :large_orange_diamond:ServletContext` init参数。
8. :large_orange_diamond: JDNI属性。
9. :large_orange_diamond: Java系统属性（`System.getProperties()`）
10. :large_orange_diamond: 操作系统环境变量
11. :large_orange_diamond:RandomValuePropertySource，只具有随机属性。
12. :large_orange_diamond:在打包的jar之外应用程序属性（application- {profile} .properties和YAML）。
13. :large_orange_diamond:打包在jar中的特定于配置文件的应用程序属性（application- {profile} .properties和YAML）。
14. :large_orange_diamond:打包jar之外的应用程序属性（application.properties和YAML）。
15. :large_orange_diamond:打包在jar中的应用程序属性（application.properties和YAML）。
16. :large_orange_diamond:`@Configuration`类上的`@PropertySource`注释。
17. :large_orange_diamond:默认属性（通过设置`SpringApplication.setDefaultProperties`指定）。       

:star:**优先级从大至小**

**24.1   配置随机值**   

RandomValuePropertySource对于注入随机值非常有用（例如，注入秘密或测试用例）。它可以生成整数，长整数，uuids或字符串，如以下示例所示：

````properties
my.secret=${random.value}
my.number=${random.int}
my.bignumber=${random.long}
my.uuid=${random.uuid}
my.number.less.than.ten=${random.int(10)}
my.number.in.range=${random.int[1024,65536]}
````

**24.2 命令行属性**

​	默认情况下，SpringApplication将任何命令行选项参数（即以 --开头的参数，例如--server.port = 9000）转换为属性，并将它们添加到Spring环境中。如前所述，命令行属性始终优先于其他属性源。

如果您不希望将命令行属性添加到环境，则可以使用禁用它们`SpringApplication.setAddCommandLineProperties(false)`

**24.3 Application 属性文件**

SpringApplication从以下位置加载`application.properties`文件并载入文件中配置的属性配置到Spring环境中：

1. 当前目录的/config目录

2. 当前目录

3. 类路径下的/config目录

4. 根类路径

 如果不喜欢`application.properties`作为配置文件名，则可以通过指定`spring.config.name`环境属性来切换到另一个文件名。还可以使用`spring.config.location`属性（以逗号分隔的目录位置或文件路径列表）来引用显式位置。以下示例显示如何指定其他文件名：

````bash
$ java -jar myproject.jar --spring.config.name=myproject
````

````bash
$ java -jar myproject.jar --spring.config.location=classpath:/default.properties,classpath:/override.properties
````

**24.4  属性文件规范**

除application.properties文件外，还可以使用以下命名约定定义特定于配置文件：

`application-{profile}.properties`

果未显式激活任何配置文件，则会加载application-default.properties中的属性。

特定配置文件和默认配置文件从同一个位置被应用程序加载，特定配置文件属性始终覆盖非特定文件中的属性，无论在jar中还是之外。如果配置了多个配置文件，SpringBoot会采用last-wins 策略来加载属性。例如，spring.profiles.active属性指定的配置文件是在通过SpringApplication API配置的配置文件之后添加的，因此优先。

````yaml
spring:
  profiles:
    active: helloWorld
#例如你配置了一个application-helloWorld.yml配置文件，
#你想让这个配置生效，那么你需要在主配置文件里激活这个配置文件
````

**24.5 配置文件中的占位符**

application.properties中的值在使用时通过现有环境进行过滤，因此您可以返回先前定义的值。

````properties
app.name=MyApp
app.description=${app.name} is a Spring Boot application
````

使用@ConfigurationProperties进行注入，例如

````yaml
my :
 age : 18
 name : java
````

````java
@ConfigurationProperties(prefix="my")
@Component
public class Person {
	private Integer age;
    private String name;
	//省略getter和setter
}
````

SpringBoot会自动根据配置文件中我们设置的值，为当前应用上下文中添加一个Person对象(age:18,name:java)

````yaml
#在yaml中定义数组
my:
 array:
	- dev.example.com
	- another.example.com
#在yaml中定义List
my:
 list:
 	-a
 	-b
 	-c
#在yaml中定义Map
my:
 map :
   key1: value1
   "[/key2]":value2
   "/key3":value3
#如果设置的key中只包含数字，字母和"-"，则无需加上引号
#key中保护以上之外的符号，须加上"[]"
#如果key中包含之外的符号且没有加"[]"，则会被自动忽略
#以上设置map key为{key1,/key2,key3}
````

在通过yml配置JavaBean时，要注意在当前项目的配置类上加上`@EnableConfigurationProperties`注解，开启配置功能，同时在代码里，注入的变量名要与配置文件中声明的配置保持一直，否则会注入失败，如下。

````java
@Component
@Data
@AllArgsConstructor
@NoArgsConstructor
@ConfigurationProperties(prefix = "my")
public class Person {
    private String[] array;
    private Map<String,String> map ;
    private List<String> list;

}
//代码2-4行使用了lombok提供的注解。（lombok可以帮助我们自动生成getter，setter等一些样板代码）
````

当Spring绑定到@ConfigurationProperties bean时，Spring Boot会尝试将外部应用程序属性强制转换为正确的类型。

**24.6 校验**

​	只要使用Spring的@Validated注释注释，Spring Boot就会尝试验证@ConfigurationProperties类。可以直接在配置类上使用JSR-303 javax.validation约束注释。为此，请确保符合条件的JSR-303实现位于类路径上，然后将约束注释添加到字段中，如以下示例所示：

````java
@ConfigurationProperties(prefix="acme")
@Validated
public class AcmeProperties {

	@NotNull
	private InetAddress remoteAddress;

	// ... getters and setters

}
````

