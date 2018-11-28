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



 

