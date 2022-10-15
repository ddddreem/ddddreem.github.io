---
title: SpringMvc学习笔记
data: 2022-03-02 21:11
tags: 
- tech
- springmvc
---
# SpringMVC

## 一.SpringMVC简介

#### 1. 什么是SpringMVC：

​	MVC是一种软件架构的思想，将软件按照模型、视图、控制器来划分

​	M：Model，模型层，指工程中的JavaBean，作用是处理数据

​		**JavaBean分为两类**：

​		· 一类是实体类Bean：专门存储业务数据的，数据库表的映射Bean

​		· 一类是业务处理Bean：指 Service 或 Dao 对象，专门用于处理业务逻辑和数据访问

​	V：View，视图层，指工程中的 html 或 jsp 等页面，作用是与用户进行交互，展示数据

​	C：Controller，控制层，指工程中的 Servlet，作用是接收页面请求和响应浏览器

​	**MVC的工作流程**：用户通过视图层发送请求到服务器，在服务器中，请求被Controller接收，Controller调用相应的Model层处理请求，处理完毕将结果返回到Controller，Controller再根据请求处理的结果找到相应的View视图，渲染数据后最终响应给浏览器

> ​	JavaEE项目一般分为三层架构：表述层（或是表示层），业务逻辑层、数据访问层，表述层表示前台页面和后台Servlet。

#### 2. SpringMVC的主要特点

+ 基于Spring，与IOC容器等基础设施无缝对接
+ 基于原生的Servlet，通过功能强大的前端控制器DispatcherServlet，对请求和响应进行统一处理

## 二.使用SpringMVC框架需要注意的点

1. 由于SpringMVC用于开发JavaEE项目，所以需要配置相应的web.xml文件

   + 创建maven工程，并在maven的配置文件中指定打包方式为war，war表示该项目为一个Web项目，并且在maven的配置文件中引入相关的依赖

     > 由于Maven的传递性，我们不必将所有需要的包全部配置依赖，而是配置最顶端的依赖，其他靠传递性导入

   + **配置web.xml**：注册SpringMVC的前端控制器DispatcherServlet，有两种配置方式

   1. **默认配置**方式：如果用这种配置，SpringMVC的配置文件默认位于WEB-INFx下，默认名字为-servlet.xml，例如，以下配置所对应SpringMVC的配置文件位于WEB-INF下，文件名为springMVC-servlet.xml

      ***eg：***在web.xml配置文件中需要添加如下配置；

      ```xml
      <!-- 配置SpringMVC的前端控制器，对浏览器发送的请求统一进行处理 -->
      <servlet>
          <servlet-name>springMVC</servlet-name>
          <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
      </servlet>
      <servlet-mapping>
          <servlet-name>springMVC</servlet-name>
          <url-pattern>/</url-pattern>
      </servlet-mapping>
      ```

   2. **扩展配置**方式（推荐）：可以通过\<init-param\>标签设置SpringMVC配置文件的位置和名称，通过\<load-on-startu\>标签设置SpringMVC前端控制器DispatcherServlet的初始化时间；使用\<init-param\>标签设置SpringMVC配置文件的位置和名称，这就是说明在扩展配置方式下，SpringMVC的配置文件可以在项目的任何位置；与默认配置方式相比，默认配置方式直接在WEB-INF下找指定文件名的文件，而扩展配置方式不需要

      ***eg：***在web.xml配置文件中需要添加如下配置；

      ```xml
      <!-- 配置SpringMVC的前端控制器，对浏览器发送的请求统一进行处理 -->
      <servlet>
          <servlet-name>springMVC</servlet-name>
          <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
          <!-- 通过初始化参数指定SpringMVC配置文件的位置和名称 -->
          <init-param>
              <!-- contextConfigLocation为固定值 -->
              <param-name>contextConfigLocation</param-name>
              <!-- 使用classpath:表示从类路径查找配置文件，例如maven工程中的src/main/resources -->
              <param-value>classpath:springMVC.xml</param-value>
          </init-param>
          <!-- 
               作为框架的核心组件，在启动过程中有大量的初始化操作要做
              而这些操作放在第一次请求时才执行会严重影响访问速度
              因此需要通过此标签将启动控制DispatcherServlet的初始化时间提前到服务器启动时
          -->
          <load-on-startup>1</load-on-startup>
      </servlet>
      <servlet-mapping>
          <servlet-name>springMVC</servlet-name>
          <url-pattern>/</url-pattern>
      </servlet-mapping>
      ```

      > **注：**标签中使用/ 和 /* 的区别
      >
      > 在\<url-pattern\>中使用 / ，/表示所匹配的请求可以是/login 或 .html 或  .js 或 .css 方式的请求路径，但是 / 不能匹配 .jsp 请求路径的请求
      >
      > ​	 因为如果jsp页面的请求被DispatcherServlet处理，有可能找不到相应的页面
      >
      >  /* 能匹配所有的请求，例如在使用过滤器时，若需要对所有请求进行过滤，就需要使用/* 的写法

2. **创建请求控制器**

由于前端控制器对浏览器发送的请求进行了统一的处理，但是具体的请求有不同的处理过程，因此需要创建处理具体请求的类，即请求控制器

请求控制器中每一个处理请求的方法成为控制器方法

因为SpringMVC的控制器由一个POJO（普通的Java类）担任，因此需要通过@Controller注解将其标识为一个控制层组件，交给Spring的IoC容器管理，此时SpringMVC才能够识别控制器的存在

```java
@Controller
public class HelloController {  
}
```

3. **创建springMVC的配置文件**

```xml
<!-- 开启扫描 -->
<context:component-scan base-package="com.dreem.springmvc.controller"/>
<!-- 配置Thymeleaf视图解析器 -->
<bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
    <property name="order" value="1"/>
    <property name="characterEncoding" value="UTF-8"/>
    <property name="templateEngine">
        <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
            <property name="templateResolver">
                <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
                    <!-- 视图前缀 -->
                    <property name="prefix" value="/WEB-INF/templates/"/> 
                    <!-- 视图后缀 -->
                    <property name="suffix" value=".html"/>
                    <property name="templateMode" value="HTML5"/>
                    <property name="characterEncoding" value="UTF-8" />
                </bean>
            </property>
        </bean>
    </property>
</bean>
```

4. **一个请求及其响应的过程**：当页面发出请求，得到其请求地址，会被前端控制器DispatcherServlet进行处理（也就是说，只要有请求，就肯定会经过DispatcherServlet）。前端控制器DispatcherServlet会加载并读取SpringMVC的核心配置文件，即开启Spring的 IOC 容器，通过扫描组件，获得Controller控制器，将请求地址和相应的属性与所有的控制器中的@RequestMapping注解的属性值（value, method, params, headers）进行匹配，如果匹配成功，Controller会调用匹配的方法并返回一个字符串类型的视图名称，返回的视图名称会经过thymeleaf的视图解析器解析，加上配置的前缀和后缀组成视图的路径，通过Thymeleaf对视图进行渲染，最后请求转发到视图对应页面。如果匹配不成功则报404-错误。

<!-- more -->

## 三.@RequestMapping注解

#### 		1.@RequestMapping注解的功能

​	从注解名称上我们可以看到，@RequestMapping注解的作用就是将请求和处理请求的控制器方法关联起来，建立映射关系。

​	SpringMVC 接收到指定的请求，就会来找到在映射关系中对应的控制器方法来处理这个请求。

#### 		2.@RequestMapping注解的位置

@RequestMapping标识一个类：设置映射请求的请求路径的初始信息

@RequestMapping标识一个方法：设置映射请求请求路径的具体信息

```java
@Controller
@RequestMapping("/test")
public class RequestMappingController {
    //此时请求映射所映射的请求的请求路径为：/test/testRequestMapping
    @RequestMapping("/testRequestMapping")
    public String testRequestMapping(){
        return "success";
    }
}
```

#### 3. @RequestMapping注解的value属性

@RequestMapping注解的value属性通过请求的请求地址匹配请求映射

@RequestMapping注解的value属性是一个字符串类型的数组，表示该请求映射能够匹配多个请求地址所对应的请求

@RequestMapping注解的value属性必须设置，至少通过请求地址匹配请求映射

#### 4. RequestMapping注解的method属性

@RequestMapping注解的method属性通过请求的请求方式（get或post）匹配请求映射

@RequestMapping注解的method属性是一个RequestMethod类型的数组，表示该请求映射能够匹配多种请求方式的请求

若当前请求的请求地址满足请求映射的value属性，但是请求方式不满足method属性，则浏览器报错405：Request method ‘POST’ not supported

> 注：
>
> 1、对于处理指定请求方式的控制器方法，SpringMVC中提供了@RequestMapping的派生注解
>
> 处理get请求的映射–>@GetMapping
>
> 处理post请求的映射–>@PostMapping
>
> 处理put请求的映射–>@PutMapping
>
> 处理delete请求的映射–>@DeleteMapping
>
> 2、常用的请求方式有get，post，put，delete
>
> 但是目前浏览器只支持get和post，若在form表单提交时，为method设置了其他请求方式的字符串（put或delete），则按照默认的请求方式get处理
>
> 若要发送put和delete请求，则需要通过spring提供的过滤器HiddenHttpMethodFilter，在RESTful部分会讲到

#### 5. @RequestMapping注解的params属性（了解）

@RequestMapping注解的params属性通过请求的请求参数匹配请求映射

@RequestMapping注解的params属性是一个字符串类型的数组，可以通过四种表达式设置请求参数和请求映射的匹配关系

“param”：要求请求映射所匹配的请求必须携带param请求参数

“!param”：要求请求映射所匹配的请求必须不能携带param请求参数

“param=value”：要求请求映射所匹配的请求必须携带param请求参数且param=value

“param!=value”：要求请求映射所匹配的请求必须携带param请求参数但是param!=value

#### @RequestMapping注解的headers属性（了解）

@RequestMapping注解的headers属性通过请求的请求头信息匹配请求映射

@RequestMapping注解的headers属性是一个字符串类型的数组，可以通过四种表达式设置请求头信息和请求映射的匹配关系

“header”：要求请求映射所匹配的请求必须携带header请求头信息

“!header”：要求请求映射所匹配的请求必须不能携带header请求头信息

“header=value”：要求请求映射所匹配的请求必须携带header请求头信息且header=value

“header!=value”：要求请求映射所匹配的请求必须携带header请求头信息且header!=value

若当前请求满足@RequestMapping注解的value和method属性，但是不满足headers属性，此时页面显示404错误，即资源未找到

#### ***eg:***

```java
// 多标签测试
// headers属性的值可以在页面的network中查看请求参数和响应参数
@RequestMapping(value = {"/testMultipleTag", "/testMultiple"}, // 设置请求的匹配地址
	method = {RequestMethod.POST, RequestMethod.GET}, // 设置请求的方式
	params = {"username!=admin", "password=123"}, // 设置匹配参数
	headers = {"host=localhost:8080"}) // 设置匹配的主机号
public String multipleTag(String[] hobby){
    System.out.println("hobby = " + Arrays.toString(hobby));
    return "/pages/success";
}
```

***报错类型***：

1. 400：请求参数（即@RequestMapping的params属性）不匹配报该类错误
2. 404：如果页面请求没有跟任何控制器方法（即@RequestMapping的value属性）匹配，报该类错误
3. 405：如果页面的请求方式与控制器方法中的属性（即@RequestMapping的method属性）匹配不了，报该类错误
4. 500：如果请求路径在控制器中匹配到了方法，但是方法返回的字符串通过Thymeleaf的视图解析器解析之后，找不到该视图，最后Thymeleaf向浏览器发送 500错误。

## 四、RESTFul风格案例

![1645149025164](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1645149025164.png)



​		访问首页可以直接使用配置文件，在springMVC.xml文件中添加控制器(view-controller)`<mvc:view-controller path="/" view-name="index"></mvc:view-controller>` 并且需要开启注解驱动，否则其他控制器方法中的RequestMapping注解将失效，**开启注解驱动**：`<mvc:annotation-driven/>`

#### @RequestBody注解

​		用来标识形参，表示浏览器的请求体（也就是浏览器传入的一些参数）

#### RequestEntity类型

​		RequestEntity封装请求报文的一种类型，需要在控制器方法的形参中设置该类型的形参，当前请求的请求报文就会赋值给该形参，可以通过getHeaders()获取请求头信息，通过getBody()获取请求体信息。

#### @ResponseBody处理json

​		服务器向客户端发送数据的时候，也就是说服务器通过response向页面发送数据时（不能接受服务器发来的对象类型），只能通过文本的形式发送，如果说想向服务器发送一个对象的话，这个时候就要转换为json，json是一种数据交互格式，与之前所用的xml是一个道理。json有两种格式，{}表示json对象，[]表示json数组。

​		在SpringMVC的核心配置文件中开启mvc的注解驱动，此时在HandlerAdaptor中会自动装配一个消息转换器：Mappingjackson2HttpMessageConverter，可以将响应转换为json格式的字符串`<mvc:annotation-driven/>`

​		在处理器方法上使用@ResponseBody注解进行标注

​		将java对象直接作为控制器方法的返回值返回，就会自动转换为json格式的字符串

处理静态资源

像css、js和图片类的静态资源，如果页面显示404找不到资源，一是可能需要重新打包（添加资源之前，程序以及运行过了，后面添加的资源没有存在我们之前打包之后的结果里面），如果重新打包之后还报404错误，有可能是springmvc的配置文件里面的没有开启对静态资源的默认访问，需在springmvc的配置文件中添加`<mvc:default-servlet-handler />`,(如果光是添加了这个标签，没有开启注解驱动的话，网页就只能访问静态资源，所以在配置完默认servlet处理器之后还需要添加`<mvc:annotation-driven />`标签)



#### @RestController注解

@RestController注解是springMVC提供的一个复合注解，标识在控制器的类上，就相当于为类添加了@Controller注解，并且为其中的每一个控制器方法都添加了@ResponseBody注解

#### @ResponseEntity注解

ResponseEntity用于控制器方法的返回值类型，该控制器方法的返回值就是响应到浏览器的响应报文，换句话来说ResponseEntity就是自定义的一个响应报文。



文件上传和下载用的都是文件复制的方法，上传功能一定用的是post



## 五、拦截器

想要使用拦截器，必须先在springMVC的配置文件中注册拦截器，拦截器会在控制器执行阶段自动进行拦截。

preHandler在控制器执行之前执行

postHandler在控制器执行之后执行

afterComplication在渲染视图之后执行

**插件**：应用中的插件就相当于拦截器的作用

##### 1、拦截器的配置

SpringMVC中的拦截器用于拦截控制器方法的执行

SpringMVC中的拦截器需要实现HandlerInterceptor接口

SpringMVC的拦截器必须在SpringMVC的配置文件中进行配置：

```xml
<bean class="com.pj.interceptor.FirstInterceptor"></bean>
<ref bean="firstInterceptor"></ref>
<!-- 以上两种配置方法都是针对DispatcherServlet所处理的所有请求进行拦截 -->
    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <mvc:exclude-mapping path="/"/>
            <ref bean="firstInerceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
<!-- 以上配置方法可以通过ref或bean标签设置拦截器，通过mvc:mapping设置需要拦截的请求，通过mvc:exclude-mapping设置需要排除的请求，即不需要拦截的请求 -->
```

>  其中`mvc:interceptors`表示可以配置多个拦截器，每个拦截器可以配置自己的拦截策略

>  /\** 表示拦截全部请求，与web.xml中配置的过滤器 Filter 的过滤所有请求的 /\*区分开

配置拦截器组件：

```java
// 用Component来表示拦截器为普通组件
@Component
// 拦截器类需要实现HandlerInterceptor接口，并且可以重写他提供的三个方法
public class FirstInerceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle()运行");
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle()运行");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion()运行");
    }
}
```



##### 2、拦截器的三个抽象方法

SpringMVC中的拦截器有三个抽象方法：

***preHandle***：控制器方法执行之前执行preHandle()，其中boolean类型的返回值表示是否拦截或放行，返回true为放行，即调用控制器方法；返回false表示拦截，即不调用控制器方法

***postHandle***：控制器方法执行之后执行postHandle()

***afterCompletion***：处理完视图和模型数据，渲染视图完毕之后执行afterCompletion()

##### 3、多个拦截器的执行顺序

> 若每个拦截器的preHandle()都返回true

此时多个拦截器的执行顺序和拦截器在SpringMVC的配置文件中的配置顺序有挂：

preHandle()会按照配置的顺序执行，而postHandle()和afterCompletion()会按照配置的反序执行

> 若某个拦截器的preHandle()返回了false

preHandle()返回false和它之前的拦截器的preHandle()都会执行，postHandle()都不执行，返回false的拦截器之前的拦截器的afterCompletion()会执行

## 六、异常处理器

#### 1、基于配置的异常处理

SpringMVC提供了一个处理控制器方法执行过程中所出现的异常的接口：***HandlerExceptionResolver***

HandlerExceptionResolver接口的实现类有：**DefaultHandlerExceptionResolver**和**SimpleMappingExceptionResolver**

SpringMVC提供了自定义的异常处理器SimpleMappingExceptinResolver，使用方式：

```xml
<bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
	<peoperty name="exceptionMapping">
		<props>
    	<!-- 
			peoperties的键表示处理器方法执行过程中出现的异常
			peoperties的值表示若出现指定异常时，设置一个新的视图名称，跳转到指定页面
		-->
			<prop key="java.lang.ArithmeticException">error</prop>
		</props>
	<!-- exceptionAttribute属性设置一个属性名，将出现的异常信息在请求域中进行共享 -->
	<property name="exceptionAttribute" value="ex"></property>
</bean>
```

#### 2、基于注解的异常处理

**与xml配置文件的工作原理一样**：都是识别到异常，处理异常，跳转相应页面

```java
@ControllerAdvice      //@ControllerAdvice将当前类表示为处理异常的组件
public class ExceptionController {
	//@ExceptionHandler设置所处理的异常
    @ExceptionHandler(value = {ArithmeticException.class,NullPointerException.class})
    public String testException1(Exception ex, Model model){   //通过形参Exception类型对象接收异常信息
        model.addAttribute("ex",ex);
        return "error";
    }
}
```

## 七、SpringMVC执行流程

#### 1、SpringMVC常用组件

+ **DispatcherServlet：前端控制器**，不需要工程师开发，由框架提供

  **作用**：统一处理请求和响应，整个流程控制的中心，由它调用其他组件处理用户的请求

+ **HandlerMapping：处理器映射器**，不需要工程师开发，由框架提供

  **作用**：根据请求的url 、method 等信息查找Handler，即控制器方法（@Controller）

+ **Handler：处理器**，需要工程师开发

  **作用**：在DispatcherServlet的控制下Handler对具体的用户请求进行处理

+ **HandlerAdapter：处理器适配器**，不需要工程师开发，由框架提供

  **作用**：通过HandlerAdapter 对处理器（控制器方法）进行执行

+ **ViewResolver：视图解析器**，不需要工程师开发，由框架提供

  **作用**：进行视图解析，得到相应的视图。例如：ThymeleafView、InternalResourceView、RedirectView

+ **View：视图**，不需要工程师开发，由框架或视图技术提供

  **作用**：将模型数据通过页面展示给用户

***注：***HandlerMapping是使页面请求和控制器方法配对，将请求和控制器方法进行映射，其实也就是RequestMapping。

HandlerAdapter是调用相应的控制器方法，当我们找到了控制器方法，我们需要使用控制器方法的时候，靠的是HandlerAdapter来调用

#### 2、DispatcherServlet

#### 3、SpringMVC的执行流程

+ 用户向服务器发送请求，请求被SpringMVC前端控制器 DispatcherServlet捕获
+ DispatcherServlet 对请求url 进行解析，得到请求资源标识符（URI），判断请求URI对应的映射：
  + 不存在
    + 再判断是否配置了mvc:default-servlet-handler
    + 如果没配置，则控制台包映射查找不到，客户端展示404错误
    + 如果有配置，则访问目标资源（一般为静态资源，如：JS、CSS、HTML），找不到客户端也会展示404错误
  + 存在则执行下面的流程
+ 根据该URI，调用HandlerMapping 获取该Handler配置的所有相关的对象（包括Handler对象以及Handler对象对应的拦截器），最后以HandlerExecutionChain 执行链对象的形式返回
+ DispatcherServlet 根据获得的Handler，选择一个合适的HandlerAdapter
+ 如果成功获取HandlerAdapter，此时将开始执行拦截器的preHandler(...)方法
+ 提取Request中的模型数据，填充Handler入参，开始执行Handler(Controller)方法，处理请求。在填充Handler的入参的过程中，根据你的配置，Spring将帮你做一些额外的工作：
  + HTTPMessageConverter：将请求信息（如 Json、xml等数据）转换成一个对象，将对象转换为指定的响应信息
  + 数据转换：对请求信息进行数据转换。如String转换成Integer、Double等
  + 数据格式化：对请求信息进行数据格式化。如将字符串转换成格式化数字或格式化日期
  + 数据验证：验证数据的有效性（长度。格式等），验证结果存储到BindingResult或Error中
+ Handler执行完成后，向DispatcherServlet 返回一个ModelAndView对象
+ 此时将开始执行拦截器的postHandle(...)方法
+ 根据返回的ModelAndView（此时会判断是否存在异常：如果存在异常，则执行HandlerExceptionResolver进行异常处理）选择一个适合的ViewResolver进行视图解析，根据Model和View，来渲染视图
+ 渲染视图完毕执行拦截器的afterCompletion(…)方法
+ 将渲染结果返回给客户端

## 知识点补充

#### session和cookie：

session和cookie的关系（会话技术：生命周期是服务器启动到服务器停止）

session依赖于cookie，cookie是客户端的会话技术，session是服务器端的会话技术

​		如果是第一次执行getSession方法，会先检测请求报文中是否会携带JSESSIONID的cookie，如果没有的话，表示在这次会话中是第一次创建HttpSession对象，然后会创建HttpSession对象，将这个HttpSesion对象放在服务器维护的map集合中，并且会去创建一个cookie，这个cookie的键是固定的是JSESSIONID，他的值是一个随机序列，还会将创建的HttpSession对象存储到服务器维护的map集合中，以JSESSIONID这个cookie的值（也就是这个随机序列）作为map集合的键，把创建的这个HttpSession对象作为map集合的值来进行存储，存储在服务器的内部。再把cookie响应到浏览器。第一次执行getSession方法的时候，这个JSESSIONID的cookie会存在响应报文中，之后会存在请求报文中。

#### 钝化和活化：

​		钝化是把session中的数据序列化然后保存到本地磁盘的过程，活化就是从磁盘中读取数据，然后反序列化的过程

#### 转发和重定向的区别:

​		***转发***：转发是一次请求，第一次是浏览器发送，第二次是发生在服务器内部，这里指的一次是浏览器发送的一次请求。转发跳转之后的地址栏还是第一次发送请求的地址。转发能访问WEB-INF下的资源。转发可以访问域对象中的数据，转发归根到底还是一次请求。转发不能跨域，只能访问服务器内部的资源，

​		***重定向***：重定向是浏览器发送了两次请求，第一次访问Servlet，第二次是访问重定向的地址。地址栏中的地址是重定向的地址。重定向不能获取域对象中的数据。重定向不能访问WEB-INF下的资源，只能通过转发到一个servlet，通过转发来访问资源，即重定向最后要转变成转发请求。重定向是浏览器发送的两次请求，浏览器可以访问任何资源，例如：我可以重定向到百度。重定向也是重定向到一个请求

​		在配置编码过滤器时，需要放在配置文件最前面，为了防止其他过滤器在设置编码前获取请求参数，出现乱码现象。

#### json

​		什么是json：json是javascript里面的一种数据格式，是一种数据交互格式(xml也是一种数据交互格式)

​		json类型：json对象和json数组

​		dom4j解析xml对象，就类似于网页解析html页面(将整个html文档当做一个document对象，通过document对象来操作当前文档中的各个标签)一样，根据标签层层解析。

​		关于json的知识点：一个是json对象，一个是json数组，对象是放在{}大括号中，数组是放在方括号[]里面。***eg：***java对象转换成json对象，map集合转换成json对象，list集合转换成json数组。

#### Ajax

​		ajax本身是页面不刷新域服务器进行交互，



### 相关链接：

​		**尚硅谷SpringMVC教程：**https://www.bilibili.com/video/BV1Ry4y1574R

