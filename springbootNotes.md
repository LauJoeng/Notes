# 一、Spring Boot入门

## 1、Spring Boot简介

Spring Boot是由Pivotal团队提供的全新框架，其设计目的是用来简化新Spring应用的初始搭建以及开发过程。该框架使用了特定的方式来进行配置，从而使开发人员不再需要定义样板化的配置。

## 2、微服务

微服务：架构风格（服务微化）

一个应用应该是一组小型服务，可以通过HTTP的方式进行互通

单体应用：ALL IN ONE

微服务：每个功能元素最终都是一个可以独立替换和升级的软件单元

## 3、环境准备

环境约束

- jdk1.8
- maven 3.x :maven3.3以上
- IDEA2017
- SpringBoot 1.5.9RELEASE

### 1、MAVEN设置

```xml
<!-- 配置JDK版本 -->
<profile>    
    <id>jdk18</id>    
    <activation>    
        <activeByDefault>true</activeByDefault>    
        <jdk>1.8</jdk>    
    </activation>    
    <properties>    
        <maven.compiler.source>1.8</maven.compiler.source>    
        <maven.compiler.target>1.8</maven.compiler.target>    
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>    
    </properties>     
</profile>
   <!-- 当 nexus-aliyun 下不了的包，或许这个镜像能下，
        才开放它，这个实在太慢，而且要把它放在首位，即 nexus-aliyun 之前，做过测试。
        所以它的用途只有那么一瞬间，就是或许它能下载，可以通过 url 去查找确定一下
    -->
    <!-- <mirror>
        <id>spring-libs-milestone</id>
        <mirrorOf>central</mirrorOf>
        <name>Spring Milestones</name>
        <url>http://repo.spring.io/libs-milestone</url>
    </mirror> -->

    <!-- nexus-aliyun 首选，放第一位,有不能下载的包，再去做其他镜像的选择  -->
    <mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>central</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>

    <!-- 备选镜像，也是可以通过 url 去查找确定一下，
        该镜像是否含有你想要的包，它比 spring-libs-milestone 快  -->
    <mirror>
        <id>central-repository</id>
        <mirrorOf>*</mirrorOf>
        typor<name>Central Repository</name>
        <url>http://central.maven.org/maven2/</url>
    </mirror>  
```

### 2、IDEA设置

​    配置IDEA的Maven，指定Setting的Maven目录和MAVEN的setting.xml文件

​	快捷键：

​	Ctrl+D 复制一行

​	Ctrl+Y 删除一行

​	Ctrl+P 参数提示

​	Ctrl+Alt+V 自动补齐方法

​	Ctrl+N 查找类方法

​	Alt+Ins 构造器、getter/setter toString

​	Ctrl+O 重载方法提示

​	Alt+Enter 提示导入类etc

​	Shift+F6 :文件重命名

## 4、Spring Boot的Hello World

### 1、创建一个Maven工程



### 2、导入Spring Boot的相关依赖

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.1.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>

<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    <java.version>1.8</java.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>

```

### 3、编写个主程序

```java
@SpringBootApplication
public class SpringBoot01HelloQuickApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBoot01HelloQuickApplication.class, args);
    }
}
```

### 4、编写相应的Controller和Service

```java
@Controller
public class HelloController {

    @ResponseBody
    @RequestMapping("/hello")
    public  String  hello(){
        return "hello world";
    }
}
```

### 5、运行主程序测试

访问 localhost:8080/hello

### 6、简化部署

在pom.xml文件中，导入build插件

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

## 5、HelloWorld深度理解

### 1.POM.xml文件

#### 1、父项目

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.1.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

这个父项目**spring-boot-starter-parent**又依赖一个父项目

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.0.1.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
</parent>
```

下面有个属性，定义了对应的版本号

```xml
<properties>
    <activemq.version>5.15.3</activemq.version>
    <antlr2.version>2.7.7</antlr2.version>
    <appengine-sdk.version>1.9.63</appengine-sdk.version>
    <artemis.version>2.4.0</artemis.version>
    <aspectj.version>1.8.13</aspectj.version>
    <assertj.version>3.9.1</assertj.version>
    <atomikos.version>4.0.6</atomikos.version>
    <bitronix.version>2.1.4</bitronix.version>
    <build-helper-maven-plugin.version>3.0.0</build-helper-maven-plugin.version>
    。。。。。。。
```



**spring-boot-starter-web:**帮我们导入web模块正常运行所依赖的组件

**spring boot**将所有的功能场景都抽取出来，做成一个个的starter(启动器)，只需要在项目里引入这些starter相关场景的所有依赖都会被导入进来，要用什么功能就导入什么场景的启动器。


@SpringBootApplication:SpringBoot应用标注在某类上说明这个类是SpringBoot的主配置类，SpringBoot应该运行这个类的main方法来启动SpringBoot应用

##使用Spring Initializer快速创建SpringBoot项目

IDE都支持使用Spring的项目向导快速创建一个SpringBoot项目，选择我们需要的模块，向导会联网创建SpringBoot项目默认生成的SpringBoot项目：
- 主程序已经生成好了，我们只要写自己的逻辑
- resource文件夹中的目录结构
 - static：保存所有的静态资源：js，css，images
 - templates:保存所有的模板页面：(SpringBoot默认使用嵌入式的Tomcat，默认不支持JSP页面，可以)可以使用模板引擎(freemarker,thymeleaf)
 - application.properties:SpringBoot应用的配置文件，可以修改一些默认配置

##Spring Boot配置文件

SpringBoot使用一个全局的配置文件，配置文件名字是固定的

application.properties
application.yml

配置文件的作用：修改SpringBoot的自动配置默认值

###配置文件的注入

配置文件

```
person:
    lastName: zhangsan
    age: 19
    boss: false
    borth: 2018/07/25
    maps: {k1: v1,k2: v2}
    list:
      - lisi
      - zhaoliu
    dog:
      name: 小狗
      age: 12
```

javaBean

```
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date borth;
    private Dog dog;
    private Map<String,Object>maps;
    private List<Object> list;

```

@Value和@ConfigurationProperties获取值比较

Name | @ConfigurationProperties | @Value
- | :-: | -: 
功能 | 批量注入配置文件中的属性| 一个个指定 
松散语法 | 支持 | 不支持 
SpEL | 不支持 | 支持
JSR303数据校验 | 支持 | 不支持
复杂类型封装 | 支持 |不支持

如果只是在某个业务逻辑中要获取一下配置文件中的某项值，使用@Value,如果专门编写了一个JavaBean来个配置文件进行映射，就使用@ConfigurationProperties

###@PropertySource和@ImportSource

@PropertySource:加载指定的配置文件
@ImportResource：导入Spring的配置文件，让配置文件里面的内容生效

SpringBoot里面没有Spring配置文件，我们自己编写的配置文件也不能自动识别，想让Spring的配置文件生效，加载进来；@ImportResource标注在一个配置类上

SpringBoot推荐给容器添加组件的方式
1. 配置类===Spring配置文件
2. 是用@Bean给容器添加组件

```
/**
 * @Configuration: 指明当前类是一个配置类，就是来替代之前的Spring配置文件
 *
 * 在配置文件中用<bean></bean>标签添加组件
 */
@Configuration
public class MyAppConfig {

    /**
     * 将方法的返回值添加到容器中。容器中这个组件默认的id就是方法名
     * @return
     */
    @Bean
    public HelloService helloService(){
        System.out.println("容器添加组件");
        return new HelloService();
    }
}
```

###配置文件占位符

随机数

```
random.value  ${random.int}、${random.long},${random.int(10)},

```

占位符获取之前的值，如果没有可以使用 : 指定默认值

```
person.lastName=张三${random.uuid}
person.age=${random.int}
person.borth=2018/07/25
person.boss=false
person.maps.k1=v1
person.maps.k2=v2
person.list=a,b,c
person.dog.name=${person.lastName}_dog
person.dog.age=21
```

##Profile

### 多profile文件

我们在主配置文件编写的时候，文件名可以是application-{profile}.properties/yml,默认使用application.properties

###yml支持多文档块

```
---
server:
  port: 8081
spring:
  profiles:
    active: dev
---
---
server:
  port: 8081
spring:
  profiles: prod
---
server:
  port: 8082
spring:
  profiles: dev
---
```

###激活指定profile

1. 在配置文件中指定spring.profiles.active=dev
2. 命令行方式，--spring.profiles.active=dev
3. 虚拟机参数配置

##配置文件加载位置

springboot启动会扫描以下位置的application.properties或者application.yml文件作为springboot的默认配置文件

- file:./config
- file:./
- classpath:/config/
- classpath:/

以上是按照优先级从高到低的顺序，所有的位置文件都会被加载，高优先级覆盖低优先级，也可以通过命令行参数spring.config.location来改变默认配置

示例:java -jar spring-boot-02.0.0.1-SNAPSHOT.jar --server.port=8087

##自动配置原理

配置文件能写什么？怎么写

[配置文件属性参照](https://docs.spring.io/spring-boot/docs/1.5.15.BUILD-SNAPSHOT/reference/htmlsingle/#common-application-properties)

###自动配置原理

- SpringBoot启动的时候加载主配置类，开启了自动配置功能@EnableAutoConfiguration
- @EnableAutoConfiguration作用:
 - 利用EnableAutConfigurationImportSelector给容器中导入了一些组件
 - 可以查看selectImport方法内容
 - List<String>configurations = getCandidateConfgurations(annotationMetadata,attributes);获取候选的配置
 - 将类路径下META-INF/spring.factories里面所有的EnableAutoConfiguration的值加入到容器中

每一个这样的的xxxAutoConfiguration类都是容器的一个组件，都加入到容器中，用它来做自动配置
- 每一个自动配置类进行自动配置功能
- 以**HttpEncodingAutoConfiguration（Http编码自动配置）**为例解释自动配置原理

```
@Configuration //表示这是一个配置类。类似以前编写配置文件一样，也可以给容器中添加组件
@EnableConfigurationProperties({HttpEncodingProperties.class}) //启用指定ConfigurationProperties功能，将配置文件中对应的值和HttpEncodingProperties绑定起来
@ConditionalOnWebApplication(
    type = Type.SERVLET
) //Spring底层@Conditional注解，根据不同条件，整个配置类里面的配置就会生效：判断当前应用是否是WEB应用
@ConditionalOnClass({CharacterEncodingFilter.class}) //判断当前项目有没有这个类
@ConditionalOnProperty(
    prefix = "spring.http.encoding",
    value = {"enabled"},
    matchIfMissing = true
) //判断配置文件是否存在某个配置 spring.http.encoding.enabled,如果存在，判断也成立
public class HttpEncodingAutoConfiguration {
```

- 所有在配置文件中能配置的属性都是在xxxProperties类中封装的，配置文件可以配置什么就可以参照某个功能对应的属性类

```
ConfigurationProperties(
    prefix = "spring.http.encoding"
)
public class HttpEncodingProperties {
```

自动配置类必须在一定的条件下才能生效，可以启用debug=true	来让控制台打印自动配置报告，可以很方便的知道哪些自动配置类生效

##SLF4j使用

- 如何在系统中使用SLF4j，开发中，日志记录方法的调用，不应该调用日志的实现类，而是调用日志抽象层里面的方法,系统应先导入slf4j和logback的jar

```
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);
    logger.info("Hello World");
  }
}
```

每一个日志的实现框架都有自己的配置文件，使用slf4j以后，配置文件还是做成日志实现框架的配置文件

**如让系统中所有的日志都统一到slf4j？**

1. 将系统中其他日志框架先排除出去
2. 用中间包替换原有的日志框架
3. 我们导入slf4j其他的实现
4. 如果我们引入其他框架，一定要把这个框架的日志依赖去除

**SpringBoot能自动适应匹配所有的日志，而且底层使用slf4j+logback的方式记录日志，引入其他框架的时候，只需要把这个框架依赖的日志框架排除掉**



###指定配置

给类路径下放上每个日志框架自己的配置文件即可，SpringBoot就不使用它默认配置了

LoggingSyetem | Customization 
- | :-: | -: 
logback | logback-spring.xml,logback-spring.groovy,logback.xml,logback.groovy 
log4j2 | log4j2-spring.xml,log4j2.xml 
JDK(Java Default Logging) | logging.properties

logback.xml:直接就被日志框架加载了
logback-spring.xml:日志框架不直接加载而由SpringBoot加载

```
<springProgile name="staging">
	可以指定某段配置只在某个环境下生效
</springProfile>
```




##WEB开发

使用SpringBoot

1. 创建SpringBoot应用，选中我们需要的模块
2. SpringBoot已经默认将这些场景配置好了，只需要在配置文件中指定少量配置就可以运行起来
3. 自己编写业务代码

以上都依靠自动配置原理

```
xxxAutoConfiguration:帮我们给容器自动配置组件
xxxProperties：配置类来封装配置文件内容
```


###springboot对静态资源的映射规则

1. 所有/webjars/**,都去classpath:/META-INF/resources/webjars/找资源
 - webjars：以jar包的方式引入静态资源
2. "/**访问当前任何项目的资源"
```
"classpath:/META-INF/resources/",
"classpath:/resources/"
"classpath:;/static/",
"classpath:/public/"
"/" 当前项目的根路径
```
3. 欢迎页：静态资源文件夹下所有index.html页面，被"/**"映射，如访问localhost:8080/就直接访问首页
4. 所有的**/favicon.ico 都是在静态资源文件夹下找

###模板引擎

JSP，Velocity，FreeMarker，Thymeleaf，SpringBoot推荐使用Thymeleaf


###引入thymeleaf

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>

<properties>
	 <thymeleaf.version>3.0.2.RELEASE</thymeleaf.version>
	<!--布局功能支持程序，thymeleaf3主程序 layout2以上版本-->
	<thymeleaf-layout-dialect.version>2.1.1</thymeleaf-layout-dialect.version>
</properties>
```

###Thymeleaf使用

只要把html页面放在classpath:templates/下，thymeleaf就会自动渲染

1. 导入themeleaf的名称空间
```
<html lang="en" xmlns:th="http://www.thymeleaf.org">
```
2. thymeleaf语法
```
	<!DOCTYPE html>
	<!--<html lang="en">-->
	<html lang="en" xmlns:th="http://www.thymeleaf.org">
	<head>
	    <meta charset="UTF-8">
	    <title>Title</title>
	</head>
	<body>
	    <h1>success</h1>
	    <div th:text="${hello}">这是显示信息</div>
	
	</body>
	</html>
```

表达式

```
Simple expressions: 
	Variable Expressions: ${...}:获取变量值。OGNL
		1. 获取属性，调用方法
		2. 使用内置的基本对象
				#ctx: the context object.
				#vars: the context variables.
				#locale: the context locale.
				#request: (only in Web Contexts) the HttpServletRequest object.
				#response: (only in Web Contexts) the HttpServletResponse object.
				#session: (only in Web Contexts) the HttpSession object.
				#servletContext: (only in Web Contexts) the ServletContext object.
		3. 工具对象
	Selection Variable Expressions: *{...}和${}功能相似，补充使用，配合th:object使用
	Message Expressions: #{...} ： 获取国际化内容
	Link URL Expressions: @{...}：定义url
	Fragment Expressions: ~{...}：片段引用表达式
Literals 
	Text literals: 'one text', 'Another one!',…
	Number literals: 0, 34, 3.0, 12.3,…
	Boolean literals: true, false
	Null literal: null
	Literal tokens: one, sometext, main,…
Text operations: 
	String concatenation: +
	Literal substitutions: |The name is ${name}|
Arithmetic operations: 
	Binary operators: +, -, *, /, %
	Minus sign (unary operator): -
Boolean operations: 
	Binary operators: and, or
	Boolean negation (unary operator): !, not
Comparisons and equality: 
	Comparators: >, <, >=, <= (gt, lt, ge, le)
	Equality operators: ==, != (eq, ne)
Conditional operators: 
	If-then: (if) ? (then)
	If-then-else: (if) ? (then) : (else)
	Default: (value) ?: (defaultvalue)
Special tokens: 
	No-Operation: _	
```


##SpringBoot 对SpringMVC的配置

如何修改SpringBoot的默认配置

模式:
1. SpringBoot在自动配置很对组件的时候，先看容器中有没有用户配置的(@Bean、@Component)，如果有就用户配置的，没有才自动配置，如果有些组件可以有多个(如ViewResolver)将用户配置的和自己默认的组合起来
2. 扩展MVC
```
<mvc:view-controller path="/hello" view-name="success"/>
    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/hello"/>
            <bean></bean>
        </mvc:interceptor>
    </mvc:interceptors>
```
**编写一个配置类(@Configuration),WebMvcConfigurationSupport，不能标注@EnableWebMvc**,既保留了所有的自动配置又用到了我们的扩展配置

```
//使用WebMvcConfigurerAdapter可以来扩展SpringMVC功能
@Configuration
public class MyMvcConfig extends WebMvcConfigurationSupport {
    @Override
    protected void addViewControllers(ViewControllerRegistry registry) {
        //浏览器发送/yang 请求来到success页面
        registry.addViewController("/yang").setViewName("success");
    }
}
```

原理:

1. WebMvcAutoConfiguration是SpringMVC自动配置类
2. 在做其他自动配置时会导入@Import(EnableWebMvcConfiguration.class)
3. 容器中所有的WebMvcConfiguration都会一起起作用
4. 我们的配置类也会被调用

可以在配置类上添加@EnableWebMvc来全面接管SpringMVC的配置


##RestfulCRUD

国际化

1. 编写国际化配置文件
2. 使用ResourceBundleMessageSource管理国际化
3. 在页面使用fmt:message去除国际化功能

步骤：
1. 编写国际化配置文件，抽取页面需要显示的国际化消息
2. SpringBoot自动配置好了管理国际化资源组件
```
@EnableConfigurationProperties
public class MessageSourceAutoConfiguration {
```
3. 去页面获取国际化的值
4. 通过点击链接切换国际化,自己编写一个LocaleResolver并把它加入到容器中即可

```
/**
 * 可以链接上携带上区域信息
 */
public class MyLocaleResolver implements LocaleResolver {
    @Override
    public Locale resolveLocale(HttpServletRequest request) {
        String l=request.getParameter("l");
        Locale locale = null;
        if(!StringUtils.isEmpty(l)){
            String[] split = l.split("_");
            locale=new Locale(split[0],split[1]);
        }
        return locale;
    }

    @Override
    public void setLocale(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Locale locale) {

    }
}

```

开发期间模板引擎页面修改以后要立即生效需要禁用缓存，设置spring.thymeleaf.cache=false

页面修改完成后按Ctrl + f9重新编译

登录错误提示

```
<p style="color: red" th:text="${msg}" th:if="${not #strings.isEmpty(msg)}"></p>
```

**拦截器进行登录检查**
```

/**
 * 登录检查
 */
public class LoginHandlerInterceptor implements HandlerInterceptor {

    /**
     * 目标方法执行之前
     * @param request
     * @param response
     * @param handler
     * @return
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws ServletException, IOException {
        Object user = request.getSession().getAttribute("loginUser");
        if(user == null){
            //未登录，返回登录页面
            request.setAttribute("msg","请先登录");
            request.getRequestDispatcher("/index.html").forward(request,response);
            return false;
        }
        return true;
    }
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) {

    }
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {

    }
}
```
##CRUD员工列表

1. RestfulCRUD：

|  - | 普通CRUD | RestfulCRUD | 
| - | :-: | -: | 
| 查询 | getEmp| emp --GET | 
| 添加  | addEmp?xxx | emp--Post |
| 修改 | updateEmp?xxx | emp/{id}--PUT |
| 删除 |deleteEmp?id=xx |emp/{id}--DELETE|

###thymeleaf公共页面抽取

```
1.抽取公共片段
<div th:fragment="copy">     
 	&copy; 2011 The Good Thymes Virtual Grocery   
</div> 

2.引入公共片段
<div th:insert="~{footer :: copy}"></div> 
~{template::selector} 模板名
~{tempplate::fragmentname}模板名:片段名

3.默认效果
insert的功能片段在div中
如果使用:th:insert等属性引入，可以不写~{},直接使用template:selector,
行内写法可以加上[[~{}]],[(~{})]
```

三种引入功能片段的th属性

- th:insert  将公共片段整个插入到声明引入的元素中
- th:replace  将声明引入的元素替换为公共片段
- th:include  将被引入的片段包含进这个标签中

```
<footer th:fragment="copy"> 
 	&copy; 2011 The Good Thymes Virtual Grocery 
</footer>

<div th:insert="footer :: copy"></div>
<div th:replace="footer :: copy"></div>
<div th:include="footer :: copy"></div>

效果:

<div>    
	<footer>      
		&copy; 2011 The Good Thymes Virtual Grocery    
	</footer>  
</div>

<footer>    
	&copy; 2011 The Good Thymes Virtual Grocery  
</footer>

<div>    
	&copy; 2011 The Good Thymes Virtual Grocery  
</div> 
```

引入片段的时候可以传入参数


##定制错误页面

SpringBoot对不存在页面有一个默认处理，客户端请求会返回一个json数据，浏览器会进入一个默认页面，原因是在BasicErrorController中有两个方法，可以针对客户端和浏览器请求做出不同处理，客户端和浏览器请求中请求头不一样。可以参照ErrorMvcAutoConfiguration类

该类给容器添加了以下组件

1. DefaultErrorAttributes 这个类可以共享错误信息
2. BasicErrorController
3. ErrorPageCustomer
4. DefaultErrorViewResolver

一旦出现4xx或5xx之类的错误，ErrorPageCustomer就会生效，就会去往/error请求，而这个请求会被BasicErrorController处理

1. 如何定制错误页面
 - 有模板引擎的情况下，:error/状态码  [将错误页面命名为 错误状态码.html放在模板引擎文件夹里面的error文件加下]，发生此状态码的错误就会来到对应的页面，也可以用 4xx,5xx来匹配所有这种类型的错误，但有限匹配精确数字页面。
 - 没有模板引擎(模板引擎找不到错误页面)，那就去静态资源文件夹下找。
 - 以上都没有错误页面，就是去往SpringBoot默认错误处理页面

定制错误的json数据

```
@ControllerAdvice
public class MyExceptionHandler {

    @ResponseBody
    @ExceptionHandler(UsernameNotExistException.class)
    public Map<String, Object> handleException(Exception e){
        Map<String,Object>map = new HashMap<>();
        map.put("code","user not exist");
        map.put("message",e.getMessage());
        return map;
    }
}
//无自适应效果


//转发到"/erro"页面
 @ResponseBody
    @ExceptionHandler(UsernameNotExistException.class)
    public String handleException(Exception e){
        Map<String,Object>map = new HashMap<>();
        map.put("code","user not exist");
        map.put("message",e.getMessage());
        //转发到error
        return "forward:/error";
    }
```

出现错误请求会去往/error请求，会在BasicErrorController处理，响应出去可以获取数据是由getErrorAttributes得到的(是AbstractErrorController(ErrorController)规定的方法);
完全编写一个类ErrorController或AbstractErrorController的子类的实现类，放在容器中
页面上能用的数据，或者是json能返回的数据都是通过ErrorAttributes.getErrorAttributes得到，容器中DefaultErrorAttributes.getErrorAttributes（）默认进行数据处理

自定义
```
@Component
public class MyErrorAttributes extends DefaultErrorAttributes {
    @Override
    public Map<String, Object> getErrorAttributes(WebRequest webRequest, boolean includeStackTrace) {
        Map<String, Object> map = super.getErrorAttributes(webRequest, includeStackTrace);
        map.put("company","yang");
        return map;
    }
}
```

最终效果：响应是自适应的，可以通过定制ErrorAttributes改变需要返回的内容





##嵌入式Servlet容器

springBoot默认使用Tomcat作为嵌入式Servlet容器

- 如何定制和修改Servlet容器相关的配置
 - 如何修改额server有关的配置
	```
	server.port=8088
	serve.context-path=/myapplication
	
	//通用的Servlet容器设置
	server.xxx
	//Tomcat设置
	server.tomcat.xxx
	```
 - 编写一个EmbeddedServletContainerCustomizer：嵌入式的Servlet容器的定制器，来修改Servlet容器的配置
	```
	@Bean
	public WebServerFactoryCustomizer webServerFactoryCustomizer(){
	   return  new WebServerFactoryCustomizer<ConfigurableServletWebServerFactory>(){
	       //定制servlet容器相关规则
	        @Override
	        public void customize(ConfigurableServletWebServerFactory factory) {
	            factory.setPort(8089);
	        }
	    };
	}
	```
- 注册三大组件（Servlet，Filter，Listener）
 由于SprigBoot默认是以jar包方式启动嵌入式Servlet容器来启动web应用，没有web.xml文件。注册三大组件用一下方式。

ServletRegistrationBean
```
//注册三大组件
    @Bean
    public ServletRegistrationBean<MyServlet> myServlet(){
        ServletRegistrationBean<MyServlet> servletRegistrationBean = new ServletRegistrationBean<>(new MyServlet(), "/myServlet");
        return servletRegistrationBean;
    }
```
FilterRegistrationBean
```
@Bean
    public FilterRegistrationBean<MyFilter>myFilter(){
        FilterRegistrationBean<MyFilter>myFilterFilterRegistrationBean = new FilterRegistrationBean<>();
        myFilterFilterRegistrationBean.setFilter(new MyFilter());
        myFilterFilterRegistrationBean.setUrlPatterns(Arrays.asList("/hello","/myServlet"));
        return myFilterFilterRegistrationBean;
    }
```

ServletListenerRegistrationBean
```
@Bean
    public ServletListenerRegistrationBean myListener(){
        ServletListenerRegistrationBean<MyListener> myListenerServletRegistrationBean = new ServletRegistrationBean<MyListener>(new MyListener());
        return myListenerServletRegistrationBean;
    }
```
- SpringBoot也能支持其他Servlet容器

首先SpringBoot可以通过ServletRegistrationBean，FilterRegistrationBean，ServletListenerRegistrationBean来注册Servlet，Filter，Listener三大组件。

```
 //注册三大组件
    @Bean
    public ServletRegistrationBean<MyServlet> myServlet(){
        ServletRegistrationBean<MyServlet> servletRegistrationBean = new ServletRegistrationBean<>(new MyServlet(), "/myServlet");
        return servletRegistrationBean;
    }

    @Bean
    public FilterRegistrationBean<MyFilter>myFilter(){
        FilterRegistrationBean<MyFilter>myFilterFilterRegistrationBean = new FilterRegistrationBean<>();
        myFilterFilterRegistrationBean.setFilter(new MyFilter());
        myFilterFilterRegistrationBean.setUrlPatterns(Arrays.asList("/hello","/myServlet"));
        return myFilterFilterRegistrationBean;
    }

    @Bean
    public ServletListenerRegistrationBean myListener(){
        ServletListenerRegistrationBean<MyListener> myListenerServletRegistrationBean = new ServletListenerRegistrationBean<MyListener>(new MyListener());
        return myListenerServletRegistrationBean;
    }
```

- SpringBoot默认使用Tomcat，也支持其他Servlet容器

Jetty:适合长连接应用，如WEB聊天等需要长时间保持连接的应用
Udertow：这个容器不支持JSP，但是一个高性能非阻塞的容器，适合高并发应用

在pom文件里配置切换容器:
```
<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                    <groupId>org.springframework.boot</groupId>
                </exclusion>
            </exclusions>
        </dependency>

        <!--引入其他Servlet容器-->
        <dependency>
            <groupId>spring-boot-starter-jetty</groupId>
            <artifactId>org.springframework.boot</artifactId>
        </dependency>
```
##SpringBoot嵌入式Servlet容器自动配置原理

- SpringBoot根据依赖情况，给容器中添加相应的EmbeddedServletContainerFactory
- 容器中某个组件要创建对象就会使后置处理器EmbeddedServletContainerCustomizerBeanPostProcessor起作用，只要是嵌入式Servlet容器工厂，后置处理器就工作
- 后置处理器，从容器中获取所有的EmbeddedServletContainerCustomizer，调用定制器的定制方法

##SpringBootServlet容器启动原理

获取嵌入式的Servlet容器工厂

1. SpringBoot应用启动运行run方法
2. refreshContext(context)SpringBoot应用属性IOC容器，创建IOC容器对象，并初始化容器，创建容器中每一个组件；如果是web应用创建web版的IOC容器，否则创建默认IOC容器
3. refresh(context)刷新刚才创建的IOC容器
4. onRefresh()：web的IOC容器重写了onRefresh方法
5. 获取嵌入式的Servlet容器EmbeddedServletContainerFactory
6. 从IOC容器中获取该组件，上面的工厂对象就来创建，从而触发上面自动配置原理的后置处理器起作用
7. 最后创建完成并启动

先启动嵌入式Servlet容器，然后再创建其他对象，IOC容器启动创建嵌入式Servlet容器


##Docker

Docker是一个开源的应用容器引擎，基于Go语言并遵从Apache2.0协议开源。Docker可以让开发者打包他们的应用以及依赖包到一个轻量级、可以移植的容器中，然后发布到任何流行的Linux机器上，也可以实现虚拟化。容器完全是沙箱机制，相互之间不会有任何接口，更重要的是容器性能开销极低。

- docker主机(Host)：安装了Docker程序的机器。
- docker客户端(Client)：连接docker主机进行操作，与docker守护进程进行通信
- docker仓库(Registry):用来保存各种打包好的软件镜像
- docker镜像(Images):软件打包好的镜像。
- docker容器(Container)：镜像启动后的实例称为一个容器

使用Docker的步骤；

1. 安装Dockcer
2. 去Docker仓库找到这个软件的镜像
3. 使用Docker运行这个镜像，这个镜像会生成一个Docker容器
4. 对容器的启动停止就是对软件的启动停止

##安装Docker

docker要求CentOS内核版本大于3.10，可以使用uname -r查看内核版本，使用yum update 更新内核版本

安装只需要是用yum install docker即可

systemctl start docker 启动docker

设值docker开机启动，systemctl enable docker

停止docker  systemctl stop docker

查找镜像  docker search 镜像名

可以用 docker pull 镜像名:[tag] 来下载镜像

docker images 查看当前有哪些镜像

docker rmi image-id  用镜像id删除镜像

###docker容器操作

软件镜像	---运行镜像---产生一个容器(正在运行的软件)


![常用命令](https://github.com/LauJoeng/Image/blob/master/2018-8/dockerOrder.PNG?raw=true)

```
docker search tomcat  搜索镜像
docker pull tomcat 拉取镜像
docker run --name mytomcat -d tomcat:latest 启动镜像
docker ps 查看运行中的容器
docker stop 容器id 停止镜像
docker ps -a 查看所有容器
docker rm 容器id  删除容器
docker start 容器id 启动容器

docker run -d -p 8888:8080 tomcat   ：   -d后台运行，-p将主机端口映射到容器内部端口

docker logs 容器id  查看启动日志

可以通过service firewalld status 查看防火墙是否开启，可以关闭，systemctl stop firewalld

每一个镜像在网站上都给出了命令参考文档
```

##SpringBoot数据访问

##JDBC

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
```

```
#schema指定建表sql脚本，会在启动应用之前执行，不指定要使用默认文件名
spring:
  datasource:
    username: root
    password: 123456
    url: jdbc:mysql://192.168.22.129:3306/jdbc
    driver-class-name: com.mysql.jdbc.Driver
    schema:
      - classpath:department.sql
      - classpath:employee.sql
	
```

效果SpringBoot2默认使用com.zaxxer.hikari.HikariDataSource数据源
数据源的相关配置都在DataSourceProperties里面；

自动配置原理:org.springframework.boot.autoconfigure.jdbc1

1. 参考DataSourceConfiguration类，根据配置创建数据源，SpringBoot2默认使用Hikari数据源，可以使用Spring.datasource.type指定自定义数据源类型
2. SpringBoot默认支持org.apache.commons.dbcp2.BasicDataSource，com.zaxxer.hikari.HikariDataSource，org.apache.tomcat.jdbc.pool.DataSource三种类型数据源，也可以使用以上三种之外的数据源
3. DataSourceInitializer，是一个ApplicationListener：作用
 - runSchemaScripts()；运行建表语句
 - runDataScripts()；运行插入数据的sql语句
 默认需要将文件命名为
```
schema-*.sql 和 data-*.sql
```
4. 默认了JDBCTemplate操作数据库

###整合MyBatis

```
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.1</version>
</dependency>
```

自定义mybatis的配置规则：给容器添加一个ConfigurationCostomizer；

```
//开启驼峰命名映射
@org.springframework.context.annotation.Configuration
public class MyBatisConfig {

    @Bean
    public ConfigurationCustomizer configurationCustomizer(){
        return new ConfigurationCustomizer(){

            @Override
            public void customize(Configuration configuration) {
                configuration.setMapUnderscoreToCamelCase(true);
            }
        };
    }
```
```
//使用MapperScan批量扫描mapper接口
@MapperScan(value = "com.yang.springboot2.mapper")
```


