# 用spring boot 进行java开发
starts是spring项目提供的依赖关系描述的集合，使用starts管理所需依赖，可以获得spring与相关技术的一站式集合，无处额外添加依赖。
例如：你如果想在你的项目里使用spring boot 与 JPA，你只需把**spring-boot-starter-data-jpa**添加在你的项目里面，无需任何多余的依赖坐标。（注：这是因为，SpringBoot已经为我们写好了父依赖，类似Maven种的父pom.xml或者Gradle中的build.gradle）
# spring boot的自动配置
### 在项目的类路径下存在的jar包，Spring boot会尝试自动配置它们，例如，在项目的类路径上存在mvc相关jar包，springboot便会自动配置mvc。自动配置是springboot的亮点，省去了很多繁琐的配置工作。spring boot为我们提供了自动配置，所以可能需要一点点配置直接就可以运行以往需要几百行xml，java config的配置，但是自动配置毕竟不是黑魔法，究其缘由是springboot为我们自动导入了**spring-boot-autoconfigure**这个依赖，这个jar包提供了几乎目前java开发主流技术的自动配置。（如果在你的项目里自己配置了相关的bean，比如数据源datasource，springboot便会自动发觉这个你自己配置的bean,不再自动配置这个bean，把这个bean的配置权限自动交还给你）

1. 如果你想了解SpringBoot为当前项目进行了那些自动配置，请开启debug级别的日志。
2. Springboot提供了@EnableAutoConfiguration(exclude={ClassNameAutoConfiguration.class}),exclude属性可以禁用掉你不需要的自动配置。也可以使用excludeName=全限定类名，还可以在主配置文件中使用spring.autoconfigure.exclude指定不需要的自动配置类。 
## 使用@SpringBootApplication 注解
如果你使用IntelliJ、STS等支持SpringBoot开发的集成开发环境，当你开启一个SpringBoot工程时，会自动生成一个带有 **@SpringBootApplication**注解的类，这一个注解即可开启三个注解（Annotation）的功能，如下：
    
    1. @EnableAutoConfiguration (启用SpringBoot自动配置机制)
    2. @ComponentScan (在应用程序所在的包上启用bean扫描)
    3. @Configuration (允许当前类在应用程序上下文注册bean或导入其他的配置)
    4. @SpringBootApplication等同于使用以上三个注解及其默认属性。
   
# SpringBoot的开发者工具
SpringBoot提供了一组额外的开发者工具包，可以使开发者在开发过程中更愉悦。只需要把**spring-boot-devtools**坐标导入所使用的依赖管理工具如Maven，Gradle中，就可以使用这些额外的功能。

运行完全打包的应用程序时SpringBoot会自动禁用开发人员工具。如果当前应用是使用java -jar启动的，或者它是从特殊的类加载器启动的，那么当前应用就会被视为“生产应用程序”。

1. SpringBoot默认使用缓存来提供当前应用程序的性能，例如模板引擎会缓存已经编译的模板以避免重复解析模板文件.而且Spring MVC在提供静态资源时为响应添加HTTP缓存头.在生产环境中这些缓存可以为我们带来性能上的提高，但在开发环境中当你对你的应用程序进行了修改，你可能不能立刻看到修改过后的运行结果。
2. 关于以上讨论的缓存功能，通常都是由应用程序的主配置文件进行配置（application.properties或application.yml）例如，Thymeleaf提供了**spring.thymeleaf.cache**属性.当使用**spring-boot-devtools**模块时，你不需要配置这些繁琐的属性，专注于你的业务，代码就好，这些小事，请交给SpringBoot，**spring-boot-devtools**会自动配置好你在开发环境中的各种设置。

  
  **完整spring-boot-devtools配置的属性列表，可以查看**<a style="font-size:23px;" href="https://github.com/spring-projects/spring-boot/blob/v2.1.0.RELEASE/spring-boot-project/spring-boot-devtools/src/main/java/org/springframework/boot/devtools/env/DevToolsPropertyDefaultsPostProcessor.java">DevToolsPropertyDefaultsPostProcessor</a>

 3. 当使用spring-boot-devtools时，你的类路径下文件发生更改的时候,dev-tools会自动重启当前项目。（1.静态文件除外。2.自动重启可能需要对IDE进行设置，如IntelliJ）
 4. Dev-tools依赖于应用程序上下文的shutdown钩子 ，如果你关闭了这个钩子 **(SpringApplication.setRegisterShutdownHook(false)).** 则dev-tools会无法正常工作

