# Spring 基础知识

## 1. Spring和Spring框架

Spring是一个开源的Java开发框架，由Pivotal团队开发，是目前最流行的Java开发框架之一。Spring框架是一个轻量级的控制反转（IoC）和面向切面（AOP）的容器框架。Spring框架的核心是Spring Core，它提供IoC和DI功能，并支持AOP。Spring框架还提供了众多的模块，如Spring MVC、Spring WebFlux、Spring Data、Spring Security、Spring Batch等，这些模块可以帮助开发者快速构建基于Spring的应用。

## 2. Spring核心：控制反转（IoC）和依赖注入（DI）

1. 控制反转（Inversion of Control，IoC）是一种设计原则，可以用来降低计算机代码之间的耦合度。其中最著名的实现方式就是依赖注入（Dependency Injection，DI）。

2. 依赖注入（DI）是指当一个对象需要其他对象协助才能运行时，通过外部容器（如Spring容器）来提供这些依赖对象。

3. Spring IoC容器负责实例化、配置和管理应用中的对象。Spring IoC容器通过依赖注入（DI）来实现对象之间的解耦。

4. Spring IoC容器通过读取配置元数据（如XML、Java注解等）来实例化、配置和管理对象。

## 3. Spring的AOP（面向切面编程）

- OOP（面向对象编程）：是一种编程范式，通过类和对象来组织代码，以提高代码的重用性、灵活性和可维护性。

- AOP（面向切面编程）：是一种编程范式，通过预编译方式和运行期动态代理，在不修改源代码的情况下给程序增加功能。

Spring AOP是以AspectJ为基础的面向切面编程（AOP）框架。AspectJ是一个开源的Java AOP框架，它提供了强大的切面功能，如事务管理、日志管理、性能监控等。

Spring AOP通过在运行期间动态地将切面织入到目标对象（如方法）中，从而实现对业务逻辑的干预。Spring AOP可以帮助开发者将横切关注点从业务逻辑中分离出来，使得代码更加清晰、易于维护。

## 4. Spring的生命周期、循环依赖和作用域

Spring的生命周期包括：实例化、依赖注入、初始化、调用、销毁。Spring的生命周期可以帮助开发者了解Spring IoC容器的工作原理。

Spring的循环依赖是指两个或两个以上的Bean之间存在循环依赖，导致Bean无法正常工作。

Spring的作用域包括singleton、prototype、request、session、global session、websocket等。

### 4.1. Spring的生命周期

1. 实例化：
    - 描述：这是Spring IoC容器管理Bean（对象）生命周期的第一步。容器根据配置元数据创建Bean的实例。
    - 方法：
        - XML配置元数据：通过配置文件（XML形式）定义Bean的创建信息，IoC容器会读取这些信息并实例化对象。

        - Java注解配置元数据：开发者可以使用注解（如@Component）在Java类上直接标注，IoC容器会扫描这些注解并实例化类。

2. 依赖注入：
    - 描述：依赖注入是Spring IoC的核心特性，允许对象之间的依赖关系通过外部容器自动管理，降低了代码之间的耦合。
    - 方法：
        - 构造器注入：通过构造器注入，Spring IoC容器会自动调用构造器，并将依赖对象注入到Bean中。

        - Setter注入：通过setter注入，Spring IoC容器会自动调用Bean的setter方法，并将依赖对象注入到Bean中。

        - 接口注入：通过接口注入，Spring IoC容器会自动调用Bean的接口方法，并将依赖对象注入到Bean中。

3. 初始化：
    - 描述：一旦Bean实例化并注入了依赖，IoC容器将调用初始化方法以设置Bean的初始状态。
    - 方法：
        - init-method：在Bean的配置文件中，可以设置初始化方法的名称，IoC容器在实例化Bean之后，会自动调用该方法。

        - @PostConstruct注解：在Bean类中，可以添加@PostConstruct注解，该注解会在Bean初始化之后自动调用。

        - 实现InitializingBean接口：在Bean类中，可以实现InitializingBean接口，并在Bean初始化时调用afterPropertiesSet方法。

4. 调用：
    - 描述：IoC容器创建Bean之后，Bean就可以被调用。
    - 方法：调用Bean的任意方法，Spring IoC容器会自动将依赖对象注入到该方法的参数中。

5. 销毁：
    - 描述：IoC容器销毁Bean之前，会调用销毁方法以释放资源。
    - 方法：
        - destroy-method：在Bean的配置文件中，可以设置销毁方法的名称，IoC容器在销毁Bean之前，会自动调用该方法。

        - @PreDestroy注解：在Bean类中，可以添加@PreDestroy注解，该注解会在Bean销毁之前自动调用。

### 4.2. Spring的循环依赖

Spring的循环依赖是指两个或两个以上的Bean之间存在循环依赖，导致Bean无法正常工作。Spring IoC容器通过控制反转（IoC）和依赖注入（DI）来解决循环依赖问题。

（注：Spring只是解决了单例模式下属性依赖的循环问题；Spring为了解决单例的循环依赖问题，使用了三级缓存。）
- 第一层缓存（singletonObjects）：单例对象缓存池，已经实例化并且属性赋值，这里的对象是成熟对象；

- 第二层缓存（earlySingletonObjects）：早期单例对象缓存池，已经实例化但是属性未赋值，这里的对象是半成熟对象；

- 第三层缓存（singletonFactories）：单例工厂缓存池，用于创建Bean的工厂对象，这里的对象是半成熟对象；

Spring IoC容器在实例化Bean时，会先检查该Bean是否在第一层缓存中，如果在，则直接返回该Bean的实例；如果不在，则会检查该Bean是否在第二层缓存中，如果在，则会调用该Bean的工厂方法创建Bean的实例，并将该Bean的实例放入第一层缓存中；如果不在，则会调用该Bean的构造器创建Bean的实例，并将该Bean的实例放入第二层缓存中。

“A对象setter依赖B对象，B对象setter依赖A对象”。
A首先完成了初始化的第一步，而且将本身提早曝光singletonFactories中，此时进行初始化的第二步，发现本身依赖对象B，此时就尝试去get(B)，发现B尚未被create，因此走create流程，B在初始化第一步的时候发现本身依赖了对象A，因而尝试get(A)，尝试一级缓存singletonObjects(确定没有，由于A还没初始化彻底)，尝试二级缓存earlySingletonObjects（也没有），尝试三级缓存singletonFactories，因为A经过ObjectFactory将本身提早曝光了，因此B可以经过ObjectFactory.getObject拿到A对象(半成品)，B拿到A对象后顺利完成了初始化阶段一、二、三，彻底初始化以后将本身放入到一级缓存singletonObjects中。此时返回A中，A此时能拿到B的对象顺利完成本身的初始化阶段二、三，最终A也完成了初始化，进去了一级缓存singletonObjects中，并且更加幸运的是，因为B拿到了A的对象引用，因此B如今hold住的A对象完成了初始化。

- 问：Spring为什么不能解决构造器的循环依赖？
答：构造器注入形成的循环依赖： 也就是beanB需要在beanA的构造函数中完成初始化，beanA也需要在beanB的构造函数中完成初始化，这种情况的结果就是两个bean都不能完成初始化，循环依赖难以解决。Spring解决循环依赖主要是依赖三级缓存，但是的在调用构造方法之前还未将其放入三级缓存之中，因此后续的依赖调用构造方法的时候并不能从三级缓存中获取到依赖的Bean，因此不能解决。

- 问：Spring为什么不能解决prototype作用域循环依赖？
答：prototype作用域的Bean在每次获取的时候都会创建一个新的实例，因此不存在循环依赖问题。

- 问：Spring为什么不能解决多例的循环依赖？
答：多实例Bean是每次调用一次getBean都会执行一次构造方法并且给属性赋值，根本没有三级缓存，因此不能解决循环依赖。

- 问：那么其它循环依赖如何解决？
    - 生成代理对象产生的循环依赖
        这类循环依赖问题解决方法很多，主要有：
        1. 使用@Lazy注解，延迟加载
        2. 使用@DependsOn注解，指定加载先后关系
        3. 修改文件名称，改变循环依赖类的加载顺序

    - 使用@DependsOn产生的循环依赖
        这类循环依赖问题要找到@DependsOn注解循环依赖的地方，迫使它不循环依赖就可以解决问题。

    - 多例循环依赖
        多例Bean的循环依赖问题，可以通过将Bean的scope设置为singleton解决。

    - 构造器循环依赖
        这类循环依赖问题可以通过使用@Lazy注解解决。


### 4.3. Spring的作用域

Spring的作用域包括singleton、prototype、request、session、global session、websocket等。

- singleton：单例作用域，在Spring IoC容器中，只会创建一个Bean的实例，Bean的实例在整个Spring IoC容器中都共享。

- prototype：原型作用域，在Spring IoC容器中，会为每个Bean创建一个新的实例。

- request：请求作用域，在Web应用中，每个HTTP请求都会创建一个新的Bean的实例。

- session：会话作用域，在Web应用中，每个HTTP会话都会创建一个新的Bean的实例。

## 5. Spring的事务管理

Spring事务管理是通过AOP（面向切面编程）实现的。Spring事务管理可以自动地管理事务的生命周期，包括事务的传播、事务的隔离级别、事务的传播行为、事务的回滚规则等。

Spring事务管理可以帮助开发者将事务管理从业务逻辑中分离出来，使得代码更加清晰、易于维护。

- 事务的传播：Spring事务管理可以实现事务的传播，即如果一个事务方法调用另一个事务方法，则可以选择是否在同一个事务中运行。

- 事务的隔离级别：Spring事务管理可以实现事务的隔离级别，即指定事务的隔离级别，如读未提交、读提交、可重复读、串行化等。

- 事务的传播行为：Spring事务管理可以实现事务的传播行为，即指定事务的传播行为，如REQUIRED、SUPPORTS、MANDATORY、NEVER等。
    - REQUIRED：如果存在事务，则加入该事务；如果没有事务，则创建一个新的事务。
    - SUPPORTS：如果存在事务，则加入该事务；如果没有事务，则不支持事务。
    - MANDATORY：如果存在事务，则加入该事务；如果没有事务，则抛出异常。
    - NEVER：如果存在事务，则抛出异常；如果没有事务，则不支持事务。

    （注：Spring事务管理默认使用REQUIRED传播行为。）

- 事务的回滚规则：Spring事务管理可以实现事务的回滚规则，即指定事务的回滚规则，如出现异常时是否回滚等。

## 6. Spring的MVC（模型-视图-控制器）

Spring MVC是Spring框架中的一个模块，它是一个基于Java的MVC框架。Spring MVC框架可以帮助开发者快速构建基于Spring的Web应用。

Spring MVC框架的主要组件包括：

- DispatcherServlet：Spring MVC框架的核心组件，负责处理HTTP请求，并将请求分派给相应的Controller。

- HandlerMapping：Spring MVC框架的组件，负责根据用户请求找到相应的Controller。

- HandlerAdapter：Spring MVC框架的组件，负责调用相应的Controller。

- ViewResolver：Spring MVC框架的组件，负责将Controller的处理结果生成View。

- ModelAndView：Spring MVC框架的组件，用于封装Controller的处理结果。

- View：Spring MVC框架的组件，用于呈现处理结果。

Spring MVC框架的常用注解包括：

- @RequestMapping：用于映射请求URL和Controller中的处理方法。

- @GetMapping：用于映射GET请求。

- @PostMapping：用于映射POST请求。

- @RequestParam：用于绑定请求参数到Controller中的处理方法参数。

- @PathVariable：用于绑定URL中的占位符到Controller中的处理方法参数。

- @ResponseBody：用于将Controller的处理结果以响应体的形式返回给客户端。

- @RestController：用于将Controller的处理结果以JSON或XML的形式返回给客户端。

- @ControllerAdvice：用于声明ControllerAdvice，处理全局的异常。

- @ExceptionHandler：用于声明异常处理方法，处理指定类型的异常。

- @ModelAttribute：用于声明在Controller的方法上，将数据绑定到Model中。

- @SessionAttributes：用于声明在Controller的方法上，将数据绑定到Session中。