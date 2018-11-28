# Spring Boot features

 	本节深入介绍Spring Boot的详细信息。在这里，您可以了解您可能想要使用和自定义的主要功能。

**23.0  **SpringAplication类提供了一种方便的方法来引导从main()方法启动的Spring应用程序，你可以使用SpringApplication.run()方法，如下所示

````java
public static void main(String[] args) {
	SpringApplication.run(MySpringConfiguration.class, args);
}
````

注：默认情况下，SpringBoot会在控制台打印info级别的日志信息。

**23.1  ** 如果您的应用程序无法启动，则已注册的FailureAnalyzers有机会提供专用错误消息和具体操作来帮助您解决问题。例如，如果您在端口8080上启动Web应用程序但该端口已在使用中，您应该会看到类似于以下消息的内容：

````txt
***************************
APPLICATION FAILED TO START
***************************

Description:

Embedded servlet container failed to start. Port 8080 was already in use.

Action:

Identify and stop the process that's listening on port 8080 or configure this application to listen on another port.
````

:musical_note: Spring Boot提供了许多FailureAnalyzer实现，您可以添加自己的[实现](https://docs.spring.io/spring-boot/docs/2.1.0.RELEASE/reference/htmlsingle/#howto-failure-analyzer)

**23.2   自定义banner**

:mushroom:banner就是SpringBoot项目启动时的logo样式，默认效果如下：

````  tx
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.0.4.RELEASE)

````

​	可以通过将banner.txt文件添加到类路径或将`spring.banner.location`属性设置为此类文件的位置来更改启动时打印的横幅。如果文件的编码不是UTF-8，则可以设置`spring.banner.charset`。除了文本文件，您还可以将banner.gif，banner.jpg或banner.png图像文件添加到类路径或设置`spring.banner.image.location`属性。图像将转换为ASCII字符表示，并打印在文本横幅上方。

在banner.txt文件中，您可以使用以下任何占位符：

|                            占位符                            | 描述                                                         |
| :----------------------------------------------------------: | :----------------------------------------------------------- |
|                   `${application.version}`                   | 应用程序的版本号，在MANIFEST.MF中声明。例如，Implementation-Version：1.0打印为1.0。 |
|              `${application.formatted-version}`              | 应用程序的版本号，在MANIFEST.MF中声明并格式化以显示（用括号括起并以v为前缀）。例如（v1.0）。 |
|                   `${spring-boot.version}`                   | 正在使用的Spring Boot版本。例如2.1.0.RELEASE。               |
|              `${spring-boot.formatted-version}`              | 您正在使用的Spring Boot版本，格式化显示（用括号括起来并以v为前缀）。例如（v2.1.0.RELEASE）。 |
| ``${Ansi.NAME} (or ${AnsiColor.NAME}, ${AnsiBackground.NAME}, ${AnsiStyle.NAME})` | 其中NAME是ANSI转义码的名称。有关详细信息，请参见[AnsiPropertySource](https://github.com/spring-projects/spring-boot/tree/v2.1.0.RELEASE/spring-boot-project/spring-boot/src/main/java/org/springframework/boot/ansi/AnsiPropertySource.java) |
|                    `${application.title}`                    | 应用程序的标题，如MANIFEST.MF中所声明的。例如，Implementation-Title：MyApp打印为MyApp。 |



 

