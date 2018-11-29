# Four.日志

**26.0 Logging**

​	Spring Boot使用[Commons Logging](https://commons.apache.org/logging)进行所有内部日志记录，但保留底层日志实现。SpringBoot为Java Util Logging，Log4J2和Logback提供了默认配置。在每种情况下，记录器都预先配置为使用控制台输出，并且还提供可选的文件输出。

​	默认情况下，如果使用“Starters”，则使用Logback进行日志记录。还包括适当的Logback路由，以确保使用Java Util Logging，Commons Logging，Log4J或SLF4J的依赖库都能正常工作。

****

**26.1 日志格式**

Spring Boot的默认日志输出：

````verilog
2014-03-05 10:57:51.112  INFO 45469 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet Engine: Apache Tomcat/7.0.52
2014-03-05 10:57:51.253  INFO 45469 --- [ost-startStop-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2014-03-05 10:57:51.253  INFO 45469 --- [ost-startStop-1] o.s.web.context.ContextLoader            : Root WebApplicationContext: initialization completed in 1358 ms
2014-03-05 10:57:51.698  INFO 45469 --- [ost-startStop-1] o.s.b.c.e.ServletRegistrationBean        : Mapping servlet: 'dispatcherServlet' to [/]
2014-03-05 10:57:51.702  INFO 45469 --- [ost-startStop-1] o.s.b.c.embedded.FilterRegistrationBean  : Mapping filter: 'hiddenHttpMethodFilter' to: [/*]
````

输出以下项目：

* 日期和时间：毫秒精度，易于排序。
* 日志级别：ERROR，WARN，INFO，DEBUG, TRACE。
* 进程ID。
* A ---分隔符，用于区分实际日志消息的开始。
* 线程名称：括在方括号中（可能会截断控制台输出）
* 记录器名称：这通常是源类名称（通常缩写）。
* 日志消息。

:warning:Logback没有FATAL级别。它映射到ERROR。

**26.2 控制台输出**

​	默认日志配置会在写入时将消息显示到控制台。默认情况下，会记录ERROR级别，WARN级别和INFO级别的消息。您还可以通过使用--debug标志启动应用程序来启用“调试”模式来显示更多消息。

````bash
$ java -jar myapp.jar --debug
````

:artificial_satellite:您还可以在application.properties中指定debug = true。

**26.2 .1  彩色编码输出**

​	如果您的终端支持ANSI，则使用颜色输出来提高可读性。可以将`spring.output.ansi.enabled`设置为支持的值以覆盖自动检测。

​	或者，可以通过将其作为转换选项指定应使用的颜色或样式。例如，要使文本变为黄色，请使用以下设置：

````tx
%clr(%d{yyyy-MM-dd HH:mm:ss.SSS}){yellow}
````

支持以下颜色和样式：

| `blue`    | 蓝色 |
| --------- | ---- |
| `cyan`    | 青色 |
| `faint`   | 微粉 |
| `green`   | 绿色 |
| `magenta` | 品红 |
| `red`     | 红色 |
| `yellow`  | 黄色 |

**26.3 文件输出**

​	默认情况下，Spring Boot仅记录到控制台，不会写入日志文件。如果除了控制台输出之外还要编写日志文件，则需要设置`logging.file`或`logging.path`属性（例如，在application.properties中）。

例如：

| logging.file | logging.path | 示例   | 意义                                                         |
| ------------ | ------------ | ------ | ------------------------------------------------------------ |
| 无           | 无           |        | 仅在控制台纪录                                               |
| 指定文件     | 无           | my.log | 日志会记录到当前目录下的my.log文件中                         |
| 无           | 指定目录     | ~/log  | 将spring.log写入指定的目录。名称可以是精确位置或相对于当前目录。 |

​	默认情况下日志文件在达到10 MB时会轮换，并且与控制台输出一样，默认情况下会记录ERROR级别，WARN级别和INFO级别的消息。可以使用logging.file.max-size属性更改大小限制。除非已设置logging.file.max-history属性，否则以前轮换的文件将无限期归档保存。

**26.4 日志级别**

​	所有受支持的日志系统都可以在Spring环境中设置记录器级别，例如在application.properties中，`logging.level.<logger-name>=<level>`

level是，TRACE, DEBUG, INFO, WARN, ERROR, FATAL, or OFF.其中的一个。也可以使用logging.level.root配置根记录器。例如

````properties
logging.level.root=WARN
logging.level.org.springframework.web=DEBUG
logging.level.org.hibernate=ERROR
````

**26.5 日志组技术**

​	可以使用日志组技术轻松把你想更改为TRACE级别的多个包，而不用重复设置。例如

想纪录特定的包的TRACE级别的日志，就可以使用日志组技术

````properties
logging.group.tomcat=org.apache.catalina, org.apache.coyote, org.apache.tomcat
logging.level.tomcat=TRACE
````

Spring Boot包含以下预定义的日志记录组，可以直接使用：

````properties
web=org.springframework.core.codec, org.springframework.http, org.springframework.web
sql=org.springframework.jdbc.core, org.hibernate.SQL
````



