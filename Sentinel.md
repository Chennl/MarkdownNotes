## Sentinel简介

> 随着微服务的流行，服务和服务之间的稳定性变得越来越重要。Sentinel 以流量为切入点，从流量控制、熔断降级、系统负载保护等多个维度保护服务的稳定性。

### **1、sentinel的特征**

- 丰富的应用场景：例如秒杀（即突发流量控制在系统容量可以承受的范围）、消息削峰填谷、集群流量控制、实时熔断下游不可用应用等
- 完备的实时监控:  Sentinel 同时提供实时的监控功能。您可以在控制台中看到接入应用的单台机器秒级数据，甚至 500 台以下规模的集群的汇总运行情况
- 广泛的开源生态： Sentinel 提供开箱即用的与其它开源框架/库的整合模块，例如与 Spring Cloud、Dubbo、gRPC 的整合。您只需要引入相应的依赖并进行简单的配置即可快速地接入 Sentinel
- 完善的 SPI 扩展点： Sentinel 提供简单易用、完善的 SPI 扩展接口。您可以通过实现扩展接口来快速地定制逻辑。例如定制规则管理、适配动态数据源等

### **2、sentinel的主要特性**

![img](sentinel\rofadfaz5tkja_55f104086cbd431187ee0cf7ce6de047.png)

### 3.sentinel的开源生态:

![img](sentinel\rofadfaz5tkja_e846705420864651a8f4f9de11332f66.png)

### 4、 sentinel 的架构图:

![img](sentinel\rofadfaz5tkja_5beaaee1f06e4526a44cad2b4bb7a03d.png)

### **5、sentinel的主要优势和特性**

- 轻量级，核心库无多余依赖，性能损耗小
- 方便接入，开源生态广泛
- 丰富的流量控制场景
- 易用的控制台，提供实时监控、机器发现、规则管理等能力
- 完善的扩展性设计，提供多样化的 SPI 接口，方便用户根据需求给 Sentinel 添加自定义的逻辑

### **6、sentinel与spring cloud Hystrix 对比**

|                | Sentinel                                       | Hystrix                       |
| -------------- | ---------------------------------------------- | ----------------------------- |
| 隔离策略       | 信号量隔离                                     | 线程池隔离/信号量隔离         |
| 熔断降级策略   | 基于响应时间或失败比率                         | 基于失败比率                  |
| 实时指标实现   | 滑动窗口                                       | 滑动窗口（基于 RxJava）       |
| 规则配置       | 支持多种数据源                                 | 支持多种数据源                |
| 扩展性         | 多个扩展点                                     | 插件的形式                    |
| 基于注解的支持 | 支持                                           | 支持                          |
| 限流           | 基于 QPS，支持基于调用关系的限流               | 有限的支持                    |
| 流量整形       | 支持慢启动、匀速器模式                         | 不支持                        |
| 系统负载保护   | 支持                                           | 不支持                        |
| 控制台         | 开箱即用，可配置规则、查看秒级监控、机器发现等 | 不完善                        |
| 常见框架的适配 | Servlet、Spring Cloud、Dubbo、gRPC 等          | Servlet、Spring Cloud Netflix |

**5、sentinel分为两个部分**

- 核心库（Java 客户端）： 不依赖任何框架/库，能够运行于所有 Java 运行时环境，同时对 Dubbo / Spring Cloud 等框架也有较好的支持
- 控制台（Dashboard）： 基于 Spring Boot 开发，打包后可以直接运行，不需要额外的 Tomcat 等应用容器

## Sentinel的控制规则

### 1.资源

资源，可以是任何东西，服务，服务里的方法，甚至是一段代码。使用 Sentinel 来进行资源保护，主要分为几个步骤:

- 定义资源

- 定义规则

- 检验规则是否生效

#### 1.1定义资源

https://sentinelguard.io/zh-cn/docs/basic-api-resource-rule.html 

- 方式四：注解方式定义资源

Sentinel 支持通过 `@SentinelResource` 注解定义资源并配置 `blockHandler` 和 `fallback` 函数来进行限流之后的处理。示例：

```java
// 原本的业务方法.
@SentinelResource(blockHandler = "blockHandlerForGetUser")
public User getUserById(String id) {
    throw new RuntimeException("getUserById command failed");
}

// blockHandler 函数，原方法调用被限流/降级/系统保护的时候调用
public User blockHandlerForGetUser(String id, BlockException ex) {
    return new User("admin");
}
```

注意 `blockHandler` 函数会在原方法被限流/降级/系统保护的时候调用，而 `fallback` 函数会针对所有类型的异常。请注意 `blockHandler` 和 `fallback` 函数的形式要求，更多指引可以参见 [Sentinel 注解支持文档](https://sentinelguard.io/zh-cn/docs/annotation-support.html)。



#### 1.2@SentinelResource 注解

> 注意：注解方式埋点不支持 private 方法。

`@SentinelResource` 用于定义资源，并提供可选的异常处理和 fallback 配置项。 `@SentinelResource` 注解包含以下属性：

- `value`：资源名称，必需项（不能为空）

- `entryType`：entry 类型，可选项（默认为 `EntryType.OUT`）

- `blockHandler` / `blockHandlerClass`: `blockHandler` 对应处理 `BlockException` 的函数名称，可选项。blockHandler 函数访问范围需要是 `public`，返回类型需要与原方法相匹配，参数类型需要和原方法相匹配并且最后加一个额外的参数，类型为 `BlockException`。blockHandler 函数默认需要和原方法在同一个类中。若希望使用其他类的函数，则可以指定 `blockHandlerClass` 为对应的类的 `Class` 对象，注意对应的函数必需为 static 函数，否则无法解析。

- **fallback**：fallback 函数名称，可选项，用于在抛出异常的时候提供 fallback 处理逻辑。fallback 函数可以针对所有类型的异常（除了exceptionsToIgnore里面排除掉的异常类型）进行处理。fallback 函数签名和位置要求：
  - 返回值类型必须与原函数返回值类型一致；
  - 方法参数列表需要和原函数一致，或者可以额外多一个 `Throwable` 类型的参数用于接收对应的异常。
  - fallback 函数默认需要和原方法在同一个类中。若希望使用其他类的函数，则可以指定 `fallbackClass` 为对应的类的 `Class` 对象，注意对应的函数必需为 static 函数，否则无法解析。
  
- **defaultFallback**（since 1.6.0）：默认的 fallback 函数名称，可选项，通常用于通用的 fallback 逻辑（即可以用于很多服务或方法）。默认 fallback 函数可以针对所以类型的异常（除了exceptionsToIgnore里面排除掉的异常类型）进行处理。若同时配置了 fallback 和 defaultFallback，则只有 fallback 会生效。defaultFallback 函数签名要求：
  - 返回值类型必须与原函数返回值类型一致；
  - 方法参数列表需要为空，或者可以额外多一个 `Throwable` 类型的参数用于接收对应的异常。
  - defaultFallback 函数默认需要和原方法在同一个类中。若希望使用其他类的函数，则可以指定 `fallbackClass` 为对应的类的 `Class` 对象，注意对应的函数必需为 static 函数，否则无法解析。
  
- `exceptionsToIgnore`（since 1.6.0）：用于指定哪些异常被排除掉，不会计入异常统计中，也不会进入 fallback 逻辑中，而是会原样抛出。

> 注：1.6.0 之前的版本 fallback 函数只针对降级异常（`DegradeException`）进行处理，**不能针对业务异常进行处理**。

特别地，若 blockHandler 和 fallback 都进行了配置，则被限流降级而抛出 `BlockException` 时只会进入 `blockHandler` 处理逻辑。若未配置 `blockHandler`、`fallback` 和 `defaultFallback`，则被限流降级时会将 `BlockException` **直接抛出**。

### 2.规则
#### 2.1 规则的种类

Sentinel 支持以下几种规则：**流量控制规则**、**熔断降级规则**、**系统保护规则**、**来源访问控制规则** 和 **热点参数规则**。

### 3. 流量控制规则 (FlowRule)

#### - 流量规则的定义

重要属性：

| Field           | 说明                                                         | 默认值                        |
| :-------------- | :----------------------------------------------------------- | ----------------------------- |
| resource        | 资源名，资源名是限流规则的作用对象                           |                               |
| count           | 限流阈值                                                     |                               |
| grade           | 限流阈值类型，QPS 或线程数模式                               | QPS 模式                      |
| limitApp        | 流控针对的调用来源                                           | `default`，代表不区分调用来源 |
| strategy        | 调用关系限流策略：直接、链路、关联                           | 根据资源本身（直接）          |
| controlBehavior | 流控效果（直接拒绝 / 排队等待 / 慢启动模式），不支持按调用关系限流 | 直接拒绝                      |

同一个资源可以同时有多个限流规则。

#### - 通过代码定义流量控制规则

理解上面规则的定义之后，我们可以通过调用 `FlowRuleManager.loadRules()` 方法来用硬编码的方式定义流量控制规则，比如：

```java
private static void initFlowQpsRule() {
    List<FlowRule> rules = new ArrayList<>();
    FlowRule rule1 = new FlowRule();
    rule1.setResource(resource);
    // Set max qps to 20
    rule1.setCount(20);
    rule1.setGrade(RuleConstant.FLOW_GRADE_QPS);
    rule1.setLimitApp("default");
    rules.add(rule1);
    FlowRuleManager.loadRules(rules);
}
```

更多详细内容可以参考 [流量控制](https://sentinelguard.io/zh-cn/docs/flow-control.html)。



### 4.熔断降级规则 (DegradeRule)

熔断降级规则包含下面几个重要的属性：

|       Field        | 说明                                                         | 默认值     |
| :----------------: | :----------------------------------------------------------- | :--------- |
|      resource      | 资源名，即规则的作用对象                                     |            |
|       grade        | 熔断策略，支持慢调用比例/异常比例/异常数策略                 | 慢调用比例 |
|       count        | 慢调用比例模式下为慢调用临界 RT（超出该值计为慢调用）；异常比例/异常数模式下为对应的阈值 |            |
|     timeWindow     | 熔断时长，单位为 s                                           |            |
|  minRequestAmount  | 熔断触发的最小请求数，请求数小于该值时即使异常比率超出阈值也不会熔断（1.7.0 引入） | 5          |
|   statIntervalMs   | 统计时长（单位为 ms），如 60*1000 代表分钟级（1.8.0 引入）   | 1000 ms    |
| slowRatioThreshold | 慢调用比例阈值，仅慢调用比例模式有效（1.8.0 引入）           |            |

同一个资源可以同时有多个降级规则。

理解上面规则的定义之后，我们可以通过调用 `DegradeRuleManager.loadRules()` 方法来用硬编码的方式定义流量控制规则。

```java
private static void initDegradeRule() {
    List<DegradeRule> rules = new ArrayList<>();
    DegradeRule rule = new DegradeRule(resource);
        .setGrade(CircuitBreakerStrategy.ERROR_RATIO.getType());
        .setCount(0.7); // Threshold is 70% error ratio
        .setMinRequestAmount(100)
        .setStatIntervalMs(30000) // 30s
        .setTimeWindow(10);
    rules.add(rule);
    DegradeRuleManager.loadRules(rules);
}
```

更多详情可以参考 [熔断降级](https://sentinelguard.io/zh-cn/docs/circuit-breaking.html)。



#### 4.1、流控规则

**1.1、流控规则的各个属性**

- 资源名： 唯一名称，默认请求路径，表示对该资源进行流控
- 针对来源： Sentinel可以针对调用者进行限流，填写微服务名，默认default（不区分来源）
- 阈值类型/单击阈值：
  QPS：（每秒钟的请求数量）：当调用该api的QPS达到阈值时，进行限流
  线程数：当调用该线程数达到阈值的时候，进行限流
- 是否集群：不需要集群
- 流控模式：
  直接： api达到限流条件时，直接限流
  关联： 当关联的资源达到阈值时，就限流自己
  链路： 只记录指定链路上的流量（指定资源从入口资源进来的流量，如果达到阈值，就进行限流）【api级别的针对来源】
- 流控效果：
  快速失败： 直接失败，抛异常
  Warm Up： 根据codeFactor（冷加载因子，默认3）的值，从阈值/codeFctor，经过预热时长，才达到设置的QPS阈值
  排队等待： 匀速排队，让请求以匀速的速度通过，阈值类型必须设置为QPS，否则无效

#### 4.2、熔断规则

除了流量控制以外，**对调用链路中不稳定的资源进行熔断降级**也是保障高可用的重要措施之一。一个服务常常会调用别的模块，可能是另外的一个远程服务、数据库，或者第三方 API 等。例如，支付的时候，可能需要远程调用银联提供的 API；查询某个商品的价格，可能需要进行数据库查询。然而，这个被依赖服务的稳定性是不能保证的。<font color=red>如果依赖的服务出现了不稳定的情况，请求的响应时间变长，那么调用服务的方法的响应时间也会变长，线程会产生堆积，最终可能耗尽业务自身的线程池，服务本身也变得不可用</font>。

<img src="sentinel\62410811-cd871680-b61d-11e9-9df7-3ee41c618644.png" alt="chain" style="zoom:50%;" />

现代微服务架构都是分布式的，由非常多的服务组成。不同服务之间相互调用，组成复杂的调用链路。以上的问题在链路调用中会产生放大的效果。复杂链路上的某一环不稳定，就可能会层层级联，最终导致整个链路都不可用。因此我们需要对不稳定的**弱依赖服务调用**进行熔断降级，暂时切断不稳定调用，避免局部不稳定因素导致整体的雪崩。熔断降级作为保护自身的手段，通常在客户端（调用端）进行配置。



**3.1、熔断规则的几种策略**

- **慢调用比例 (SLOW_REQUEST_RATIO)**：

  **选择以慢调用比例**作为**阈值**，需要设置允许的慢调用 RT（RT 即最大的响应时间），请求的响应时间大于该值则统计为慢调用。

  当单位统计时长（statIntervalMs）内请求数目大于设置的最小请求数目，并且慢调用的比例大于阈值，则接下来的熔断时长内请求会自动被熔断。

  经过熔断时长后熔断器会进入探测恢复状态（HALF-OPEN 状态），若接下来的一个请求响应时间小于设置的慢调用 RT 则结束熔断，若大于设置的慢调用 RT 则会再次被熔断

- **异常比例 (ERROR_RATIO)**：

  **当单位统计时长（statIntervalMs）内请求数目大于设置的最小请求数目，并且异常的比例大于阈值**，则接下来的熔断时长内请求会自动被熔断。经过熔断时长后熔断器会进入探测恢复状态（HALF-OPEN 状态），若接下来的一个请求成功完成（没有错误）则结束熔断，否则会再次被熔断。

  异常比率的阈值范围是 [0.0, 1.0]，代表 0% - 100%

- **异常数 (ERROR_COUNT)**：当单位统计时长内的异常数目超过阈值之后会自动进行熔断。经过熔断时长后熔断器会进入探测恢复状态（HALF-OPEN 状态），若接下来的一个请求成功完成（没有错误）则结束熔断，否则会再次被熔断

  ## 熔断降级规则说明

  熔断降级规则（DegradeRule）包含下面几个重要的属性：

  |       Field        | 说明                                                         | 默认值     |
  | :----------------: | :----------------------------------------------------------- | :--------- |
  |      resource      | 资源名，即规则的作用对象                                     |            |
  |       grade        | 熔断策略，支持慢调用比例/异常比例/异常数策略                 | 慢调用比例 |
  |       count        | 慢调用比例模式下为慢调用临界 RT（超出该值计为慢调用）；异常比例/异常数模式下为对应的阈值 |            |
  |     timeWindow     | 熔断时长，单位为 s                                           |            |
  |  minRequestAmount  | 熔断触发的最小请求数，请求数小于该值时即使异常比率超出阈值也不会熔断（1.7.0 引入） | 5          |
  |   statIntervalMs   | 统计时长（单位为 ms），如 60*1000 代表分钟级（1.8.0 引入）   | 1000 ms    |
  | slowRatioThreshold | 慢调用比例阈值，仅慢调用比例模式有效（1.8.0 引入）           |            |

https://developer.aliyun.com/article/1476696?spm=5176.26934562.main.2.6b9b5e41UzrsM5

https://www.aliyun.com/sswb/1060224.html?spm=5176.26934562.main.11.4c853618LIUdmy



## Sentinel 控制台

### 1. 概述

Sentinel 提供一个轻量级的开源控制台，它提供机器发现以及健康情况管理、监控（单机和集群），规则管理和推送的功能。

Sentinel 控制台包含如下功能:

- **查看机器列表以及健康情况**：收集 Sentinel 客户端发送的心跳包，用于判断机器是否在线。
- **监控 (单机和集群聚合)**：通过 Sentinel 客户端暴露的监控 API，定期拉取并且聚合应用监控信息，最终可以实现秒级的实时监控。
- **规则管理和推送**：统一管理推送规则。
- **鉴权**：生产环境中鉴权非常重要。这里每个开发者需要根据自己的实际情况进行定制。

### 2. 启动控制台

#### 2.1 获取控制台

从 [release 页面](https://github.com/alibaba/Sentinel/releases) 下载最新版本的控制台 jar 包。

#### 2.2 启动

> **注意**：启动 Sentinel 控制台需要 JDK 版本为 1.8 及以上版本。

使用如下命令启动控制台：

```bash
java -Dserver.port=8080 -Dcsp.sentinel.dashboard.server=localhost:8080 -Dproject.name=sentinel-dashboard -jar sentinel-dashboard.jar
```

其中 `-Dserver.port=8080` 用于指定 Sentinel 控制台端口为 `8080`。

Sentinel 控制台默认用户名和密码都是 `sentinel`。

> 注：若您的应用为 Spring Boot 或 Spring Cloud 应用，您可以通过 Spring 配置文件来指定配置，详情请参考 [Spring Cloud Alibaba Sentinel 文档](https://github.com/spring-cloud-incubator/spring-cloud-alibaba/wiki/Sentinel)。

### 3. 客户端接入控制台

控制台启动后，客户端需要按照以下步骤接入到控制台。

#### 3.1 引入JAR包

客户端需要引入 Transport 模块来与 Sentinel 控制台进行通信。您可以通过 `pom.xml` 引入 JAR 包:

```xml
<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-transport-simple-http</artifactId>
    <version>x.y.z</version>
</dependency>
```

#### 3.2 配置启动参数

启动: java -jar -Dserver.port=8858  sentinel-dashboard-1.8.8.jar

![image-20240907172115574](sentinel\image-20240907172115574.png)

除了修改 JVM 参数，也可以通过配置文件取得同样的效果。更详细的信息可以参考 [启动配置项](https://sentinelguard.io/zh-cn/docs/startup-configuration.html)。

#### 3.3 触发客户端初始化

**确保客户端有访问量**，Sentinel 会在**客户端首次调用的时候**进行初始化，开始向控制台发送心跳包。

> 注意：您还需要根据您的应用类型和接入方式引入对应的 [适配依赖](https://sentinelguard.io/zh-cn/docs/open-source-framework-integrations.html)，否则即使有访问量也不能被 Sentinel 统计。

### 4. 查看机器列表以及健康情况

当您在机器列表中看到您的机器，就代表着您已经成功接入控制台；如果没有看到您的机器，请检查配置，并通过 `${user.home}/logs/csp/sentinel-record.log.xxx` 日志来排查原因，详细的部分请参考 [日志文档](https://sentinelguard.io/zh-cn/docs/logs.html)。

## Spring-Cloud服务配置

#### **1、为服务打开sentinel的监控**

引入[sentinel依赖](https://zhida.zhihu.com/search?q=sentinel依赖&zhida_source=entity&is_preview=1)

```xml
    <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
            <version>${spring-cloud-alibaba.version}</version>
        </dependency>
```

#### 2. yml文件添加sentinel相关配置

```yaml
spring:
  application:
      name: message-service
  cloud:
    nacos:
      server-addr: 127.0.0.1:8848  #172.30.111.64:8848 #
      config:
        file-extension: yaml
    sentinel:
        transport:
          dashboard: 127.0.0.1:8858
```

#### 3. 启动服务并访问测试接口

```java
    @Autowired
    SMSService smsService;

    @RequestMapping("/send/{phone}/{msg}")
    public String sendSMS(@PathVariable("phone") String phone, @PathVariable("msg") String msg){
        System.out.printf("send message %s to %s%n",msg,phone);
        return String.format("send message %s to %s",msg,phone);
    }
```

#### 4.查看控制台

![image-20240907173614380](sentinel\image-20240907173614380.png)

![image-20240907173651469](sentinel\image-20240907173651469.png)

#### 5.流量控制规则

##### 5.1 流量控制规则

![image-20240907174243572](sentinel\image-20240907174243572.png)

资源名称就是我们的接口访问路径 然后我们一秒一次访问一下接口（正常） http://localhost:8081/api/send/13858097900/hellworld 可以看到正常返回数据

![WXWorkCapture_17257066604733](sentinel\WXWorkCapture_17257066604733.png)

接下来我们快速请求接口，一秒钟点击多次 发现已经被限流了

![WXWorkCapture_1725706780573](sentinel\WXWorkCapture_1725706780573.png)

##### **5.2、流控规则--[关联流控模式](https://zhida.zhihu.com/search?q=关联流控模式&zhida_source=entity&is_preview=1)**

我们模拟一下流控模式中的关联模式 关联： 当关联的资源达到阈值时，就限流自己，也就是说关联的资源（接口），QPS为1时，一秒内被多次请求的时候，自己的接口就会被限流。

我们在服务中创建一个关联接口 /now

```java
@RequestMapping("/now")
public String now(){
    System.out.printf("Time is: %s",DateTimeUtil.getCurrentTime());
    return String.format("Time is: %s",DateTimeUtil.getCurrentTime());
}
```

<img src="sentinel\image-20240907194030612.png" alt="image-20240907194030612" style="zoom:67%;" />

我们用jmeter模拟一下请求/api/now接口 一秒3个请求并循环.

在这期间我们访问/api/send/{phone}/{msg}接口 发现被限流了.

##### 5.3 **资源名称方式限流**与兜底方法

按照资源名称的方式限流，并对[限流流](https://zhida.zhihu.com/search?q=限流流&zhida_source=entity&is_preview=1)进行友好处理， 我们修改一下/user/getOrderNo接口的代码，增加@SentinelResource注解，并做后续处理 演示代码：

```java
@RequestMapping("/wx/{wxId}/{msg}")
@SentinelResource(value="sendWXResource",blockHandler = "sendWXBlockHandler", blockHandlerClass = MessageServiceController.class)
public String sendWX(@PathVariable(value="wxId") String wxId, @PathVariable("msg") String msg){
        System.out.printf("send message %s to %s%n",msg,wxId);
        return String.format("send message %s to %s",msg,wxId);
    }
/**
 * 限流后续操作方法
*/
public static String sendWXBlockHandler(String wxId, String msg, BlockException e){
        return  "不好意思，前方拥挤，请您稍后再试";
    }
```

<img src="sentinel\image-20240907221030378.png" alt="image-20240907221030378" style="zoom:67%;" />

快速访问我们的接口，发现返回的信息已经变成我们兜底方法自定义的返回提示了

![WXWorkCapture_17257183985437](sentinel\WXWorkCapture_17257183985437.png)

#### 6.**[熔断规则](https://zhida.zhihu.com/search?q=熔断规则&zhida_source=entity&is_preview=1)**

##### 6.1**熔断规则的几种策略**

- **慢调用比例 (SLOW_REQUEST_RATIO)**：选择以慢调用比例作为阈值，需要设置允许的慢调用 RT（即最大的响应时间），<font color=red>请求的响应时间大于该值则统计为慢调用</font>。当单位统计时长（statIntervalMs）内请求数目大于设置的最小请求数目，并且慢调用的比例大于阈值，则接下来的熔断时长内请求会自动被熔断。经过熔断时长后熔断器会进入探测恢复状态（HALF-OPEN 状态），若接下来的一个请求响应时间小于设置的慢调用 RT 则结束熔断，若大于设置的慢调用 RT 则会再次被熔断

- **异常比例 (ERROR_RATIO)**：当单位统计时长（statIntervalMs）内请求数目大于设置的最小请求数目，并且异常的比例大于阈值，则接下来的熔断时长内请求会自动被熔断。经过熔断时长后熔断器会进入探测恢复状态（HALF-OPEN 状态），若接下来的一个请求成功完成（没有错误）则结束熔断，否则会再次被熔断。异常比率的阈值范围是 [0.0, 1.0]，代表 0% - 100%

- **异常数 (ERROR_COUNT)**：当单位统计时长内的异常数目超过阈值之后会自动进行熔断。经过熔断时长后熔断器会进入探测恢复状态（HALF-OPEN 状态），若接下来的一个请求成功完成（没有错误）则结束熔断，否则会再次被熔断

##### 6.2 **新建一个熔断规则**

我们先写一个新的接口并且睡眠一秒，使用注解资源名称的形式，并创建自定义兜底方法 方法接口地址 /api/wecom/{wxId}/{msg}

```java
@RequestMapping("/wecom/{wxId}/{msg}")
    @SentinelResource(value="sendWeComResource",fallback = "sendWeComResource", fallbackClass  = MessageSentinelResourceHandler.class)
    public String sendWeCom(@PathVariable(value="wxId") String wxId, @PathVariable("msg") String msg){
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("我是sendWeComResource");
        return "我是sendWeComResource";

    }
```



创建单独的[handler类](https://zhida.zhihu.com/search?q=handler类&zhida_source=entity&is_preview=1)来处理兜底方法:

```java
@Component
public class MessageSentinelResourceHandler {
    public static String sendWeComResource(Throwable throwable){
        System.out.println("触发熔断，服务不可用");
        return "触发熔断，服务不可用";
    }
}
```

接着我们在控制台新建一个熔断规则（使用慢调用比例的策略）

<img src="sentinel\image-20240908132425747.png" alt="image-20240908132425747" style="zoom:67%;" />

添加的熔断规则解释一下： 在 2 秒的统计时间（统计时长）内，对资源接口的请求数大于 5 （最小请求数），请求响应时间（最大 RT）大于200毫秒的请求占比超过比例阈值 0.1 ，则下一个请求开始触发熔断，熔断时长是 5 秒，也就是 5 秒内，熔断器是打开状态，这期间的请求都会触发熔断, 5秒过后熔断器关闭，此时熔断器会进入探测恢复状态（HALF-OPEN 状态）。

若接下来的一个请求响应时间小于设置的慢调用 RT ，则结束熔断，若大于设置的慢调用 RT 则会再次被熔断。

**RT（平均响应时间）**： 当1s内持续进入5个请求，且对应请求的平均响应时间（秒级）均超过阈值，那么在接下来的时间窗口期内，对该方法的调用都会自动的熔断。注意Sentinel默认统计的RT上限是4900ms，超出此阈值的都会算作4900ms，若需要更改上限可以通过启动配置项 -Dcsp.sentinel.statistic.max.rt=xxx来配置

#### 7.热点规则

> 热点即经常访问的数据。很多时候我们希望统计某个热点数据中访问频次最高的数据，并对其访问进行限制，比如对某个商品id进行限制，或者对某个用户id进行限制

**7.1、热点规则属性** 参数索引：方法中参数的索引第几个参数 单机阈值：每秒达到单机阈值的数量就会触发兜底方法

**7.2、新建一个热点规则** 我们先创建一个测试方法，使用注解资源名称的形式，并创建自定义兜底方法

```java 
/**
     * 测试centinel热点规则限流
     * @param userId
     * @param shopId
     * @return
     */
    @GetMapping("/hotspot")
    @SentinelResource(value = "hotspotResource" , blockHandler = "hotspotResource", blockHandlerClass = UserSentinelResourceHandler.class)
    public String hotspot(@RequestParam(value = "userId" ,required = false) String userId,
                          @RequestParam(value = "shopId" ,required = false) String shopId){
        System.out.println("我是hotspot");
        return "我是hotspot";
    }
```

兜底方法:

```java
public static String hotspotResource(String userId, String shopId,BlockException blockException){
        System.out.println("您被认为恶意访问，触发热点限流");
        return "您被认为恶意访问，触发热点限流";
    }
```

然后我们在控制台新建热点规则

![img](sentinel\v2-dc65699c256a5b50da862f947e37e8d5_1440w.webp)

其中参数索引 0 代表的就是userId这个参数 我们访问一下接口 先访问带shopId的（一秒内多次访问） [http://localhost:9090/user/hotspot?shopId=4](https://link.zhihu.com/?target=http%3A//localhost%3A9090/user/hotspot%3FshopId%3D4) 正常返回

![img](sentinel\v2-fbdf7e54af271c2f4338c38a939ba720_1440w.webp)

再访问下带userId参数的（一秒内多次访问） [http://localhost:9090/user/hotspot?userId=4](https://link.zhihu.com/?target=http%3A//localhost%3A9090/user/hotspot%3FuserId%3D4) 返回降级函数的内容

![img](sentinel\v2-cf2d9480b59062e6409dbf9713a62fbe_1440w.webp)
