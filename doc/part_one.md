# 用spring boot 进行java开发
starts是spring项目提供的依赖关系描述的集合，使用starts管理所需依赖，可以获得spring与相关技术的一站式集合，无处额外添加依赖。
例如：你如果想在你的项目里使用spring boot 与 JPA，你只需把**spring-boot-starter-data-jpa**添加在你的项目里面，无需任何多余的依赖坐标。（注：这是因为，SpringBoot已经为我们写好了父依赖，类似Maven种的父pom.xml或者Gradle中的build.gradle）
# spring boot的自动配置
### 在项目的类路径下存在的jar包，Spring boot会尝试自动配置它们，例如，在项目的类路径上存在mvc相关jar包，springboot便会自动配置mvc。自动配置是springboot的亮点，省去了很多繁琐的配置工作。spring boot为我们提供了自动配置，所以可能需要一点点配置直接就可以运行以往需要几百行xml，java config的配置，但是自动配置毕竟不是黑魔法，究其缘由是springboot为我们自动导入了**spring-boot-autoconfigure**这个依赖，这个jar包提供了几乎目前java开发主流技术的自动配置。（如果在你的项目里自己配置了相关的bean，比如数据源datasource，springboot便会自动发觉这个你自己配置的bean,不再自动配置这个bean，把这个bean的配置权限自动交换给你）

1. 如果你想了解SpringBoot为当前项目进行了那些自动配置，开启debug级别的日志。
2. Springboot提供了@EnableAutoConfiguration(exclude={ClassNameAutoConfiguration.class}),exclude属性可以禁用掉你不需要的自动配置。也可以使用excludeName=全限定类名，还可以在主配置文件中使用spring.autoconfigure.exclude指定不需要的自动配置类
    
