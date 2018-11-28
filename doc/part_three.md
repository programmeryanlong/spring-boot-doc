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



