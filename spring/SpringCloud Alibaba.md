[Spring Cloud Alibaba Reference Documentation](https://spring-cloud-alibaba-group.github.io/github-pages/2021/en-us/index.html#_introduction)

## 1\. Introduction

## 1\. 引言

Spring Cloud Alibaba aims to provide a one-stop solution for microservices development. This project includes the required components for developing distributed applications and services, so that developers can develop distributed applications easily with the Spring Cloud programming models.

阿里巴巴旨在为微服务开发提供一站式解决方案。该项目包括开发分布式应用程序和服务所需的组件，这样开发人员就可以使用 Spring Cloud 编程模型轻松地开发分布式应用程序。

With Spring Cloud Alibaba, you only need to add a few annotations and configurations, and you will be able to use the distributed solutions of Alibaba for your applications, and build a distributed system of your own with Alibaba middleware.

使用阿里巴巴，你只需要添加一些注释和配置，你就可以使用阿里巴巴的分布式解决方案来开发你的应用程序，并使用阿里巴巴中间件构建一个你自己的分布式系统。

The features of Spring Cloud Alibaba:

阿里巴巴的特色:

1.  **Flow control and service degradation**：support WebServlet, WebFlux, OpenFeign, RestTemplate, Dubbo access to the function of limiting and degrading flow. It can modify the rules of limiting and degrading flow in real time through the console at run time, and it also supports the monitoring of limiting and degrading Metrics.
    
    流量控制和服务降级: 支持 WebServlet、 WebFlux、 OpenFeign、 RestTemplate、达博访问限制和降级流量的功能。它可以在运行时通过控制台实时修改限制和降低流量的规则，并支持对限制和降低流量的度量进行监控。
    
2.  **Service registration and discovery**：Service can be registered and clients can discover the instances using Spring-managed beans, auto integration Ribbon.
    
    服务注册和发现: 可以注册服务，客户端可以使用 Spring 管理的 bean 自动集成 Ribbon 发现实例。
    
3.  **Distributed configuration**：support for externalized configuration in a distributed system, auto refresh when configuration changes.
    
    分布式配置: 支持分布式系统中的外部化配置，当配置发生变化时自动刷新。
    
4.  **Rpc Service**：extend Spring Cloud client RestTemplate and OpenFeign to support calling Dubbo RPC services.
    
    RPC 服务: 扩展 Spring Cloud 客户端 RestTemplate 和 OpenFeign 以支持调用 Dubbo RPC 服务。
    
5.  **Event-driven**：support for building highly scalable event-driven microservices connected with shared messaging systems.
    
    事件驱动: 支持构建与共享消息传递系统连接的高度可伸缩的事件驱动微服务。
    
6.  **Distributed Transaction**：support for distributed transaction solution with high performance and ease of use.
    
    分布式事务: 支持具有高性能和易用性的分布式事务解决方案。
    
7.  **Alibaba Cloud Object Storage**：massive, secure, low-cost, and highly reliable cloud storage services. Support for storing and accessing any type of data in any application, anytime, anywhere.
    
    阿里巴巴云对象存储: 大规模、安全、低成本、高可靠的云存储服务。支持在任何应用程序、任何时间、任何地点存储和访问任何类型的数据。
    
8.  **Alibaba Cloud SchedulerX**：accurate, highly reliable, and highly available scheduled job scheduling services with response time within seconds.
    
    阿里巴巴云计划: 准确、高可靠、高可用的计划作业调度服务，响应时间在几秒钟内。
    
9.  **Alibaba Cloud SMS**： A messaging service that covers the globe, Alibaba SMS provides convenient, efficient, and intelligent communication capabilities that help businesses quickly contact their customers.
    
    阿里巴巴云端短讯: 阿里巴巴云端短讯是一项覆盖全球的讯息服务，提供方便、高效及智能的通讯功能，协助企业迅速联络客户。
    

Spring Cloud Alibaba also provide rich [examples](https://github.com/alibaba/spring-cloud-alibaba/tree/master/spring-cloud-alibaba-examples).

阿里巴巴也提供了丰富的例子。

## 2\. Dependency Management

## 依赖管理

If you’re a Maven Central user, add our BOM to your pom.xml <dependencyManagement> section. This will allow you to omit versions for any of the Maven dependencies and instead delegate versioning to the BOM.

如果您是 Maven Central 用户，请将我们的 BOM 添加到 pom.xml < depencyManagement > 部分。这将允许您省略任何 Maven 依赖项的版本，而是将版本控制委托给 BOM。

```
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-dependencies</artifactId>
            <version>2021.0.4.0</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

In the following sections, it will be assumed you are using the Spring Cloud Alibaba BOM and the dependency snippets will not contain versions.

在接下来的章节中，我们将假设您使用的是阿里巴巴的 Spring Cloud BOM，并且依赖项片段将不包含版本。

## 3\. Spring Cloud Alibaba Nacos Discovery

## 3\. 阿里巴巴玉米卷发现

Nacos is an easy-to-use dynamic service discovery, configuration and service management platform for building cloud native applications.

Nacos 是一个易于使用的动态服务发现、配置和服务管理平台，用于构建云本地应用程序。

With Spring Cloud Alibaba Nacos Discovery, you can quickly access the Nacos service registration feature based on Spring Cloud’s programming model.

使用 Spring Cloud 阿里巴巴 Nacos Discovery，你可以快速访问基于 Spring Cloud 编程模型的 Nacos 服务注册功能。

### 3.1. Service Registration/Discovery: Nacos Discovery

### 3.1. 服务注册/发现: 发现 Nacos

Service discovery is one of the key components in the microservices architecture. In such a architecture, configuring a service list for every client manually could be a daunting task, and makes dynamic scaling extremely difficult. Nacos Discovery helps you to register your service to the Nacos server automatically, and the Nacos server keeps track of the services and refreshes the service list dynamically. In addition, Nacos Discovery registers some of the metadata of the service instance, such as host, port, health check URL, homepage to Nacos. For details about how to download and start Nacos, refer to the [Nacos Website](https://nacos.io/zh-cn/docs/quick-start.html).

服务发现是微服务体系结构中的关键组件之一。在这样的体系结构中，为每个客户机手动配置服务列表可能是一项艰巨的任务，并且使得动态扩展变得极其困难。Nacos Discovery 帮助您将服务自动注册到 Nacos 服务器，Nacos 服务器跟踪服务并动态刷新服务列表。此外，Nacos Discovery 向 Nacos 注册了服务实例的一些元数据，如主机、端口、健康检查 URL、主页。有关如何下载和启动 Nacos 的详细信息，请参阅 Nacos 网站。

### 3.2. How to Introduce Nacos Discovery for service registration/discovery

### 3.2. 如何在服务注册/发现时引入「发现违禁品」

please use the starter with the group ID as `com.alibaba.cloud` and the artifact ID as `spring-cloud-starter-alibaba-nacos-discovery`.

请使用组 ID 为 com.alibaba.cloud 的 starter，工件 ID 为 spring-cloud-starter-alibaba-nacos-Discovery。

```
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

### 3.3. An example of using Nacos Discovery for service registration/discovery and call

### 3.3. 使用 Nacos Discovery 进行服务注册/发现和调用的示例

Nacos Discovery integrate with the Netflix Ribbon, RestTemplate or OpenFeign can be used for service-to-service calls.

Nacos Discovery 集成了 Netflix Ribbon、 RestTemplate 或 OpenFeign，可用于服务到服务的调用。

#### 3.3.1. Nacos Server Startup

#### 3.3.1. Nacos 服务器启动

For details about how to download and start Nacos, refer to the [Nacos Website](https://nacos.io/zh-cn/docs/quick-start.html).

有关如何下载和启动 Nacos 的详细信息，请参阅 Nacos 网站。

After Nacos Server starts, go to [http://ip:8848](http://ip:8848/) to view the console (default account name/password is nacos/nacos):

在 Nacos Server 启动后，转到 http://ip: 8848查看控制台(默认帐户名/密码是 Nacos/Nacos) :

![TB1XEfwbQH0gK0jSZPiXXavapXa 2790 1060](SpringCloud%20Alibaba.assets/TB1XEfwbQH0gK0jSZPiXXavapXa-2790-1060-1676858858541-10.png)

Figure 1. Nacos Dashboard 图1. Nacos 仪表盘

For more Nacos Server versions, you can download the latest version from [release page](https://github.com/alibaba/nacos/releases).

有关更多 Nacos 服务器版本，您可以从发布页面下载最新版本。

#### 3.3.2. Start a Provider Application

#### 3.3.2启动提供者应用程序

The following sample illustrates how to register a service to Nacos.

下面的示例说明如何向 Nacos 注册服务。

-   Configuration of pom.xml The following is a complete example of pom.xml：
    
    Xml 的配置以下是 pom.xml 的一个完整示例:
    

pom.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>open.source.test</groupId>
    <artifactId>nacos-discovery-test</artifactId>
    <version>1.0-SNAPSHOT</version>
    <name>nacos-discovery-test</name>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>${spring.boot.version}</version>
        <relativePath/>
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring.cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>${spring.cloud.alibaba.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

-   Configuration of application.properties Some of the basic configurations of Nacos must be included in application.properties(or application.yaml), as shown below：
    
    Properties 的配置一些 Nacos 的基本配置必须包含在 application.properties (或 application.yaml)中，如下所示:
    

application.properties 应用性能

```
server.port=8081
spring.application.name=nacos-provider
spring.cloud.nacos.discovery.server-addr=127.0.0.1:8848
management.endpoints.web.exposure.include=*
```

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>If you do not want to use Nacos for service registration and discovery, you can set <span contenteditable="true">如果您不想使用 Nacos 进行服务注册和发现，可以设置</span><code>spring.cloud.nacos.discovery</code> to <span>到</span><code>false</code>.</td></tr></tbody></table>

-   The following is a sample for starting Provider:
    
    下面是启动 Provider 的示例:
    

```
@SpringBootApplication
@EnableDiscoveryClient
public class NacosProviderDemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(NacosProviderDemoApplication.class, args);
    }

    @RestController
    public class EchoController {
        @GetMapping(value = "/echo/{string}")
        public String echo(@PathVariable String string) {
            return "Hello Nacos Discovery " + string;
        }
    }
}
```

Now you can see the registered services on the Nacos console.

现在您可以在 Nacos 控制台上看到已注册的服务。

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>Before you start the provider application, please start Nacos first. Refer to <span contenteditable="true">在启动提供者应用程序之前，请先启动 Nacos</span><a href="https://nacos.io/zh-cn/docs/quick-start.html">Naco Website<span> Naco 网站</span></a> for more details. <span contenteditable="true">了解更多细节</span></td></tr></tbody></table>

#### 3.3.3. Start a Consumer Application

#### 3.3.3启动消费者应用程序

It might not be as easy as starting a provider application, because the consumer needs to call the RESTful service of the provider. In this example, we will use the most primitive way, that is, combining the LoadBalanceClient and RestTemplate explicitly to access the RESTful service. You can refer to section 1.2 for pom.xml and application.properties configurations. The following is the sample code for starting a consumer application.

这可能不像启动提供程序应用程序那么容易，因为使用者需要调用提供程序的 RESTful 服务。在本例中，我们将使用最原始的方法，即显式地组合 LoadBalanceClient 和 RestTemplate 来访问 RESTful 服务。您可以参考1.2节了解 pom.xml 和 application.properties 配置。下面是启动使用者应用程序的示例代码。

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>You can also access the service by using RestTemplate and FeignClient with load balancing. <span contenteditable="true">还可以通过使用带负载平衡的 RestTemplate 和 FeignClient 访问服务</span></td></tr></tbody></table>

```
@SpringBootApplication
@EnableDiscoveryClient
public class NacosConsumerApp {

    @RestController
    public class NacosController{

        @Autowired
        private LoadBalancerClient loadBalancerClient;
        @Autowired
        private RestTemplate restTemplate;

        @Value("${spring.application.name}")
        private String appName;

        @GetMapping("/echo/app-name")
        public String echoAppName(){
            //Access through the combination of LoadBalanceClient and RestTemplate
            ServiceInstance serviceInstance = loadBalancerClient.choose("nacos-provider");
            String path = String.format("http://%s:%s/echo/%s",serviceInstance.getHost(),serviceInstance.getPort(),appName);
            System.out.println("request path:" +path);
            return restTemplate.getForObject(path,String.class);
        }

    }

    //Instantiate RestTemplate Instance
    @Bean
    public RestTemplate restTemplate(){

        return new RestTemplate();
    }

    public static void main(String[] args) {

        SpringApplication.run(NacosConsumerApp.class,args);
    }
}
```

In this example, we injected a LoadBalancerClient instance, and instantiated a RestTemplate manually. At the same time, we injected the configuration value of `spring.application.name` into the application, so that the current application name can be displayed when calling the service of the provider.

在本例中，我们注入了一个 LoadBalancerClient 实例，并手动实例化了一个 RestTemplate。同时，我们将 spring.application.name 的配置值注入应用程序，以便在调用提供者的服务时显示当前的应用程序名称。

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>Please start Nacos before you start the consumer application. For details, please refer to <span contenteditable="true">请在开始消费者申请前启动 Nacos。有关详情，请参阅</span><a href="https://nacos.io/zh-cn/docs/quick-start.html">Nacos Website<span> Nacos 网站</span></a>.</td></tr></tbody></table>

Next, access the `[http://ip:port/echo/app-name](https://spring-cloud-alibaba-group.github.io/github-pages/2021/en-us/index.html#_introductionhttp://ip:port/echo/app-name)` interface provided by the consumer. Here we started the port of 8082. The access result is shown below：

接下来，访问消费者提供的 http://ip:port/echo/app-name 接口。在这里我们开始端口8082。查阅结果如下:

```
Address：http://127.0.0.1:8082/echo/app-name
Access result： Hello Nacos Discovery nacos-consumer
```

### 3.4. Nacos Discovery Endpoint

### 3.4. Nacos Discovery 端点

Nacos Discovery provides an Endpoint internally with a corresponding endpoint id of `nacosdiscovery`.

Nacos Discovery 在内部提供一个端点，其中包含相应的 nacosDiscovery 端点 id。

Endpoint exposed json contains two properties:

端点公开的 json 包含两个属性:

1.  subscribe: Shows the current service subscribers
    
    订阅: 显示当前的服务订阅者
    
2.  NacosDiscoveryProperties: Shows the current basic Nacos configurations of the current service
    
    显示当前服务的当前基本 Nacos 配置
    

The followings shows how a service instance accesses the Endpoint:

下面显示了服务实例如何访问端点:

```
{
  "subscribe": [
    {
      "jsonFromServer": "",
      "name": "nacos-provider",
      "clusters": "",
      "cacheMillis": 10000,
      "hosts": [
        {
          "instanceId": "30.5.124.156#8081#DEFAULT#nacos-provider",
          "ip": "30.5.124.156",
          "port": 8081,
          "weight": 1.0,
          "healthy": true,
          "enabled": true,
          "cluster": {
            "serviceName": null,
            "name": null,
            "healthChecker": {
              "type": "TCP"
            },
            "defaultPort": 80,
            "defaultCheckPort": 80,
            "useIPPort4Check": true,
            "metadata": {

            }
          },
          "service": null,
          "metadata": {

          }
        }
      ],
      "lastRefTime": 1541755293119,
      "checksum": "e5a699c9201f5328241c178e804657e11541755293119",
      "allIPs": false,
      "key": "nacos-provider",
      "valid": true
    }
  ],
  "NacosDiscoveryProperties": {
    "serverAddr": "127.0.0.1:8848",
    "endpoint": "",
    "namespace": "",
    "logName": "",
    "service": "nacos-provider",
    "weight": 1.0,
    "clusterName": "DEFAULT",
    "metadata": {

    },
    "registerEnabled": true,
    "ip": "30.5.124.201",
    "networkInterface": "",
    "port": 8082,
    "secure": false,
    "accessKey": "",
    "secretKey": ""
  }
}
```

### 3.5. Weight Route

### 3.5公斤

#### 3.5.1. Spring Cloud Loadbalancer

#### 3.5.1. Spring Cloud 负载均衡器

pom.xml

```
<dependencies>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-loadbalancer</artifactId>
    </dependency>
</dependencies>
```

application.properties 应用性能

```
spring.cloud.loadbalancer.ribbon.enabled=false
spring.cloud.loadbalancer.nacos.enabled=true
```

### 3.6. More Information about Nacos Discovery Starter Configurations

### 3.6更多有关 Nacos Discovery 启动器配置的信息

The following shows the other configurations of the starter of Nacos Discovery:

下面显示了 Nacos Discovery 起动机的其他配置:

<table><colgroup><col> <col> <col> <col></colgroup><tbody><tr><td><p>Configuration</p><p>配置</p></td><td><p>Key</p><p>钥匙</p></td><td><p>Default Value</p><p>默认值</p></td><td><p>Description</p><p>描述</p></td></tr><tr><td><p>Server address</p><p contenteditable="true">服务器地址</p></td><td><p><code>spring.cloud.nacos.discovery.server-addr</code></p><p contenteditable="true">Server-addr</p></td><td></td><td><p>IP and port of the Nacos Server listener</p><p contenteditable="true">Nacos 服务器侦听器的 IP 和端口</p></td></tr><tr><td><p>Service name</p><p>服务名称</p></td><td><p><code>spring.cloud.nacos.discovery.service</code></p><p contenteditable="true">Spring Cloud.nacos.Discovery y.service</p></td><td><p><code>${spring.application.name}</code></p><p contenteditable="true">Spring.application.name</p></td><td><p>Name the current service</p><p contenteditable="true">命名当前服务</p></td></tr><tr><td><p>Weight</p><p>体重</p></td><td><p><code>spring.cloud.nacos.discovery.weight</code></p><p contenteditable="true">SpringCloud.nacos 发现，重量</p></td><td><p><code>1</code></p></td><td><p>Value range: 1 to 100. The bigger the value, the greater the weight</p><p contenteditable="true">值范围: 1到100。值越大，权重越大</p></td></tr><tr><td><p>Network card name</p><p>网卡名称</p></td><td><p><code>spring.cloud.nacos.discovery.network-interface</code></p><p contenteditable="true">Network-interface</p></td><td></td><td><p>If the IP address is not specified, the registered IP address is the IP address of the network card. If this is not specified either, the IP address of the first network card will be used by default.</p><p contenteditable="true">如果未指定 IP 地址，则注册的 IP 地址是网卡的 IP 地址。如果这也没有指定，默认情况下将使用第一张网卡的 IP 地址。</p></td></tr><tr><td><p>Registered IP address</p><p contenteditable="true">注册 IP 地址</p></td><td><p><code>spring.cloud.nacos.discovery.ip</code></p><p contenteditable="true">Spring Cloud.nacos.Discovery. ip</p></td><td></td><td><p>Highest priority</p><p contenteditable="true">最高优先级</p></td></tr><tr><td><p>Registered IP address Type</p><p contenteditable="true">注册 IP 地址类型</p></td><td><p><code>spring.cloud.nacos.discovery.ip-type</code></p><p contenteditable="true">Spring Cloud.nacos.Discovery. ip-type</p></td><td><p><code>IPv4</code></p></td><td><p>IPv4 and IPv6 can be configured, If there are multiple IP addresses of the same type of network card, and you want to specify a specific network segment address, you can use <code>spring.cloud.inetutils.preferred-networks</code> to configure the filter address</p><p contenteditable="true">可以配置 IPv4和 IPv6，如果同一种网卡有多个 IP 地址，并且你想指定一个特定的网段地址，你可以使用 spring.clod.inetutils.preder- 网络来配置过滤器地址</p></td></tr><tr><td><p>Registered port</p><p>注册港口</p></td><td><p><code>spring.cloud.nacos.discovery.port</code></p><p contenteditable="true">Spring Cloud.nacos.Discovery. port</p></td><td><p><code>-1</code></p></td><td><p>Will be detected automatically by default. Do not need to be configured.</p><p contenteditable="true">默认情况下将自动检测。不需要进行配置。</p></td></tr><tr><td><p>Namespace</p><p>命名空间</p></td><td><p><code>spring.cloud.nacos.discovery.namespace</code></p><p>名称空间</p></td><td></td><td><p>A typical scenario is to isolate the service registration for different environment, such as resource (configurations, services etc.) isolation between testing and production environment</p><p contenteditable="true">典型的场景是将服务注册隔离到不同的环境中，例如在测试和生产环境之间隔离资源(配置、服务等)</p></td></tr><tr><td><p>AccessKey</p><p>访问密钥</p></td><td><p><code>spring.cloud.nacos.discovery.access-key</code></p><p contenteditable="true">Spring.Cloud.nacos.Discovery. access-key</p></td><td></td><td><p>Alibaba Cloud account accesskey</p><p contenteditable="true">阿里巴巴云端帐户存取密码匙</p></td></tr><tr><td><p>SecretKey</p><p contenteditable="true">“秘密钥匙”</p></td><td><p><code>spring.cloud.nacos.discovery.secret-key</code></p><p contenteditable="true">Spring Cloud Nacos Discovery Secret-Key</p></td><td></td><td><p>Alibaba Cloud account secretkey</p><p contenteditable="true">阿里巴巴云端帐户密钥</p></td></tr><tr><td><p>Metadata</p><p>元数据</p></td><td><p><code>spring.cloud.nacos.discovery.metadata</code></p><p contenteditable="true">发现元数据</p></td><td></td><td><p>You can define some of the metadata for your services in the Map format</p><p contenteditable="true">您可以用 Map 格式为您的服务定义一些元数据</p></td></tr><tr><td><p>Log file name</p><p contenteditable="true">日志文件名</p></td><td><p><code>spring.cloud.nacos.discovery.log-name</code></p><p contenteditable="true">Log-name</p></td><td></td><td></td></tr><tr><td><p>Cluster Name</p><p>群组名称</p></td><td><p><code>spring.cloud.nacos.discovery.cluster-name</code></p><p>名称</p></td><td><p><code>DEFAULT</code></p><p>默认</p></td><td><p>Cluster name of Nacos</p><p contenteditable="true">Nacos 集群名称</p></td></tr><tr><td><p>Endpoint</p><p>终点</p></td><td><p><code>spring.cloud.nacos.discovery.endpoint</code></p><p contenteditable="true">Spring Cloud.nacos.Discovery y.endpoint</p></td><td></td><td><p>The domain name of a certain service in a specific region. You can retrieve the server address dynamically with this domain name</p><p contenteditable="true">特定区域中某个服务的域名。您可以使用此域名动态检索服务器地址</p></td></tr><tr><td><p>Integrate LoadBalancer or not</p><p contenteditable="true">是否集成负载平衡器</p></td><td><p><code>spring.cloud.loadbalancer.nacos.enabled</code></p><p>启用了</p></td><td><p><code>false</code></p><p>假的</p></td><td></td></tr><tr><td><p>Enable Nacos Watch</p><p contenteditable="true">启用 Nacos 手表</p></td><td><p><code>spring.cloud.nacos.discovery.watch.enabled</code></p><p>启用</p></td><td><p><code>true</code></p><p>没错</p></td><td><p>set to false to close watch</p><p contenteditable="true">设定为假以密切观察</p></td></tr></tbody></table>

## 4\. Spring Cloud Alibaba Nacos Config

## 4\. 阿里巴巴玉米饼配对

Nacos is an easy-to-use dynamic service discovery, configuration and service management platform for building cloud native applications.

Nacos 是一个易于使用的动态服务发现、配置和服务管理平台，用于构建云本地应用程序。

Use Spring Cloud Alibaba Nacos Config to quickly access Nacos configuration management capabilities based on Spring Cloud’s programming model.

使用 Spring Cloud 阿里巴巴 Nacos 配置快速访问基于 Spring Cloud 编程模型的 Nacos 组态管理功能。

### 4.1. How to Introduce Nacos Config for configuration

### 4.1. 如何为配置引入 Nacos 配置

please use the starter with the group ID as `com.alibaba.cloud` and the artifact ID as `spring-cloud-starter-alibaba-nacos-config`.

请使用组 ID 为 com.alibaba.cloud 的 starter，工件 ID 为 spring-cloud-starter-alibaba-nacos-config。

```
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
```

### 4.2. Quickstart

### 4.2快速启动

Nacos Config uses DataId and GROUP to determine a configuration.

Nacos 配置使用 DataId 和 GROUP 来确定配置。

The following figure shows that the DataId uses `myDataid`, GROUP uses `DEFAULT_GROUP`, and configures a configuration item of the format Properties:

下图显示了 DataId 使用 myDataid，GROUP 使用 DEFAULT \_ GROUP，并配置一个格式为 Properties 的形态项目:

![TB1N2nxbRr0gK0jSZFnXXbRRXXa 2448 1194](SpringCloud%20Alibaba.assets/TB1N2nxbRr0gK0jSZFnXXbRRXXa-2448-1194-1676858858541-12.png)

Figure 2. Nacos Config Item 图2. Nacos 配置项

#### 4.2.1. Initialize Nacos Server

#### 4.2.1. 初始化 Nacos 服务器

For specific startup methods, refer to the "Nacos Server Startup" section of the Spring Cloud Alibaba Nacos Discovery section.

有关特定的启动方法，请参阅 Spring Cloud 阿里巴巴 Nacos Discovery 部分的“ Nacos 服务器启动”部分。

After the Nacos Server is started, add how to configure it:

启动 Nacos Server 后，添加如何配置它:

```
Data ID:    nacos-config.properties

Group  :    DEFAULT_GROUP

Configuration format:    Properties

Configuration content:   user.name=nacos-config-properties
            user.age=90
```

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>The default file extension of DataId is properties. <span contenteditable="true">DataId 的默认文件扩展名是属性</span></td></tr></tbody></table>

##### Usage on the Client

##### 客户端的使用

If you want to use Nacos to manage externalized configurations for your applications, please use the starter with the group ID as `com.alibaba.cloud` and the artifact ID as `spring-cloud-starter-alibaba-nacos-config`.

如果您想使用 Nacos 来管理应用程序的外部化配置，请使用具有组 ID com.alibaba.cloud 的 starter，以及具有 spring-cloud-starter-alibaba-Nacos-config 的工件 ID。

```
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
```

Now we can create a standard Spring Boot application.

现在我们可以创建一个标准的 SpringBoot 应用程序。

```
@SpringBootApplication
public class NacosConfigApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext applicationContext = SpringApplication.run(NacosConfigApplication.class, args);
        String userName = applicationContext.getEnvironment().getProperty("user.name");
        String userAge = applicationContext.getEnvironment().getProperty("user.age");
        System.err.println("user name :" +userName+"; age: "+userAge);
    }
}
```

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>Note that when your <span contenteditable="true">请注意，当你</span><code><code>spring-cloud-alibaba</code>’s version is <code>``2021.1</code></code>, since the <span>自从</span><code><code>nacos</code></code> gets the configuration in the <span>中的配置</span><code><code>bootstrap.yml </code></code>file Will be loaded before the <span>文件将在</span><code><code>application.yml</code></code> file. According to the official documentation of spring mentioned [bootstrap](<span contenteditable="true"> 根据提到的 Spring 的官方文件[ bootstrap ](</span><a href="https://docs.spring.io/spring-cloud-config/docs/current/reference/html/#config-first-bootstrap">https://docs.spring.io/spring-cloud-config/docs/current/reference/html/#config-first-bootstrap</a>) To solve this problem, we recommend that you add the following dependencies to the project root <span contenteditable="true">)为了解决这个问题，我们建议您向项目根目录添加以下依赖项</span><code><code>pom.xml</code></code> file <span>文件</span></td></tr></tbody></table>

```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bootstrap</artifactId>
    <version>3.1.1</version>
</dependency>
```

Before running this example, we need to configure the address of the Nacos server in bootstrap.properties. For example:

在运行这个示例之前，我们需要在 bootstrap.properties 中配置 Nacos 服务器的地址，例如:

bootstrap.properties

```
# DataId By default, the `spring.application.name` configuration is combined with the file extension (the configuration format uses properties by default), and the GROUP is not configured to use DEFAULT_GROUP by default. Therefore, the Nacos Config configuration corresponding to the configuration file has a DataId of nacos-config.properties and a GROUP of DEFAULT_GROUP
spring.application.name=nacos-config
spring.cloud.nacos.config.server-addr=127.0.0.1:8848
```

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>If you use domain name to access Nacos, the format of <span contenteditable="true">如果使用域名访问 Nacos，则</span><code>spring.cloud.nacos.config.server-addr</code> should be <span>应该是</span><code>Domain name:port</code>. For example, if the Nacos domain name is abc.com.nacos, and the listerner port is 80, then the configuration should be <code>spring.cloud.nacos.config.server-addr=abc.com.nacos:80</code>. Port 80 cannot be omitted. <span contenteditable="true">。端口80不能省略</span></td></tr></tbody></table>

Run this example and you can see the following output:

运行此示例，可以看到以下输出:

```
2018-11-02 14:24:51.638  INFO 32700 --- [main] c.a.demo.provider.NacosConfigApplication    : Started NacosConfigApplication in 14.645 seconds (JVM running for 15.139)
user name :nacos-config-properties; age: 90
2018-11-02 14:24:51.688  INFO 32700 --- [-127.0.0.1:8848] s.c.a.AnnotationConfigApplicationContext : Refreshing org.springframework.context.annotation.AnnotationConfigApplicationContext@a8c5e74: startup date [Fri Nov 02 14:24:51 CST 2018]; root of context hierarchy
```

### 4.3. Add Configurations with DataId in YAML Format

### 使用 YAML 格式的 DataId 添加配置

Nacos Config supports yaml format as well. You only need to complete the following 2 steps.

Nacos 配置也支持 yaml 格式。

1、In the bootstrap.properties file, add the following line to claim that the format of DataId is yaml. As follows:

1、在 bootstrap.properties 文件中，添加以下代码行，声明 DataId 的格式为 yaml:

bootstrap.properties

```
spring.cloud.nacos.config.file-extension=yaml
```

2、Add a configuration with the DataId in yaml format on the Nacos console, as shown below:

2、在 Nacos 控制台上以 yaml 格式添加一个 DataId 配置，如下所示:

```
Data ID:        nacos-config.yaml

Group  :        DEFAULT_GROUP

Configuration format:        YAML

Configuration content:        user.name: nacos-config-yaml
                              user.age: 68
```

After completing the preivous two steps, restart the testing program and you will see the following result.

在完成前两个步骤之后，重新启动测试程序，您将看到以下结果。

```
2018-11-02 14:59:00.484  INFO 32928 --- [main] c.a.demo.provider.NacosConfigApplication:Started NacosConfigApplication in 14.183 seconds (JVM running for 14.671)
user name :nacos-config-yaml; age: 68
2018-11-02 14:59:00.529  INFO 32928 --- [-127.0.0.1:8848] s.c.a.AnnotationConfigApplicationContext : Refreshing org.springframework.context.annotation.AnnotationConfigApplicationContext@265a478e: startup date [Fri Nov 02 14:59:00 CST 2018]; root of context hierarchy
```

### 4.4. Support Dynamic Configuration Udpates

### 4.4. 支持动态配置更新

Nacos Config also supports dynamic configuration updates. The code for starting Spring Boot application testing is as follows:

Nacos Config 还支持动态配置更新:

```
@SpringBootApplication
public class NacosConfigApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext applicationContext = SpringApplication.run(NacosConfigApplication.class, args);
        while(true) {
            //When configurations are refreshed dynamically, they will be updated in the Enviroment, therefore here we retrieve configurations from Environment every other second.
            String userName = applicationContext.getEnvironment().getProperty("user.name");
            String userAge = applicationContext.getEnvironment().getProperty("user.age");
            System.err.println("user name :" + userName + "; age: " + userAge);
            TimeUnit.SECONDS.sleep(1);
        }
    }
}
```

When user.name is changed, the latest value can be retrieved from the application, as shown below:

更改 user.name 时，可以从应用程序检索最新值，如下所示:

```
user name :nacos-config-yaml; age: 68
user name :nacos-config-yaml; age: 68
user name :nacos-config-yaml; age: 68
2018-11-02 15:04:25.069  INFO 32957 --- [-127.0.0.1:8848] o.s.boot.SpringApplication               : Started application in 0.144 seconds (JVM running for 71.752)
2018-11-02 15:04:25.070  INFO 32957 --- [-127.0.0.1:8848] s.c.a.AnnotationConfigApplicationContext : Closing org.springframework.context.annotation.AnnotationConfigApplicationContext@10c89124: startup date [Fri Nov 02 15:04:25 CST 2018]; parent: org.springframework.context.annotation.AnnotationConfigApplicationContext@6520af7
2018-11-02 15:04:25.071  INFO 32957 --- [-127.0.0.1:8848] s.c.a.AnnotationConfigApplicationContext : Closing org.springframework.context.annotation.AnnotationConfigApplicationContext@6520af7: startup date [Fri Nov 02 15:04:24 CST 2018]; root of context hierarchy
//Read the updated value from Enviroment
user name :nacos-config-yaml-update; age: 68
user name :nacos-config-yaml-update; age: 68
```

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>You can disable automatic refresh with this setting`spring.cloud.nacos.config.refresh.enabled=false`. <span contenteditable="true">您可以使用这个设置‘ spring.clod.nacos.config.resh.enable = false’禁用自动刷新</span></td></tr></tbody></table>

### 4.5. Support configurations at the profile level

### 支持配置文件级别的配置

When configurations are loaded by Nacos Config, basic configurations with DataId of `${spring.application.name}. ${file-extension:properties}` , and DataId of `${spring.application.name}-${profile}. ${file-extension:properties}` are also loaded. If you need to use different configurations from different environments, you can use the `${spring.profiles.active}` configuration provided by Spring.

当配置由 Nacos 配置加载时，DataId 为 ${ spring.application.name }的基本配置。${文件扩展名: properties }和 ${ spring.application.name }-${ profile }的 DataId。${文件扩展名: 属性}也被加载。如果需要使用来自不同环境的不同配置，可以使用 Spring 提供的 ${ Spring.profiles.active }配置。

```
spring.profiles.active=develop
```

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>When specified in configuration files, ${spring.profiles.active} must be placed in bootstrap.properties. <span contenteditable="true">在配置文件中指定时，必须将 ${ spring.profiles.active }放在 bootstrap.properties 中</span></td></tr></tbody></table>

Add a basic configuration in Nacos, with a DataId of nacos-config-develop.yaml, as shown below:

在 Nacos 中添加一个基本配置，其 DataId 为 Nacos-config-development. yaml，如下所示:

```
Data ID:        nacos-config-develop.yaml

Group  :        DEFAULT_GROUP

Configuration format:        YAML

Configuration content:        current.env: develop-env
```

Run the following Spring Boot application testing code:

运行以下 Spring Boot 应用程序测试代码:

```
@SpringBootApplication
public class NacosConfigApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext applicationContext = SpringApplication.run(NacosConfigApplication.class, args);
        while(true) {
            String userName = applicationContext.getEnvironment().getProperty("user.name");
            String userAge = applicationContext.getEnvironment().getProperty("user.age");
            //Get the current deployment environment
            String currentEnv = applicationContext.getEnvironment().getProperty("current.env");
            System.err.println("in "+currentEnv+" enviroment; "+"user name :" + userName + "; age: " + userAge);
            TimeUnit.SECONDS.sleep(1);
        }
    }
}
```

After started, you can see the output as follows in the console:

启动之后，您可以在控制台中看到如下输出:

```
in develop-env enviroment; user name :nacos-config-yaml-update; age: 68
2018-11-02 15:34:25.013  INFO 33014 --- [ Thread-11] ConfigServletWebServerApplicationContext : Closing org.springframework.boot.web.servlet.context.AnnotationConfigServletWebServerApplicationContext@6f1c29b7: startup date [Fri Nov 02 15:33:57 CST 2018]; parent: org.springframework.context.annotation.AnnotationConfigApplicationContext@63355449
```

To switch to the production environment, you only need to change the parameter of `${spring.profiles.active}`. As show below:

要切换到生产环境，只需更改 ${ spring.profiles.active }的参数:

```
spring.profiles.active=product
```

At the same time, add the basic configuration with the DataId in the Nacos of your production environment. For example, you can add the configuration with the DataId of nacos-config-product.yaml in Nacos of your production environment:

同时，在生产环境的 Nacos 中使用 DataId 添加基本配置。例如，可以使用 nacos-config-product 的 DataId 添加配置。你的生产环境:

```
Data ID:        nacos-config-product.yaml

Group  :        DEFAULT_GROUP

Configuration format:        YAML

Configuration content:        current.env: product-env
```

Start the testing program and you will see the following result:

启动测试程序，您将看到以下结果:

```
in product-env enviroment; user name :nacos-config-yaml-update; age: 68
2018-11-02 15:42:14.628  INFO 33024 --- [Thread-11] ConfigServletWebServerApplicationContext : Closing org.springframework.boot.web.servlet.context.AnnotationConfigServletWebServerApplicationContext@6aa8e115: startup date [Fri Nov 02 15:42:03 CST 2018]; parent: org.springframework.context.annotation.AnnotationConfigApplicationContext@19bb07ed
```

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>In this example, we coded the configuration in the configuration file by using the <span contenteditable="true">在此示例中，我们在配置文件中使用</span><code>spring.profiles.active=&lt;profilename&gt;</code> method. In real scenarios, this variable needs to be different in different environment. You can use the <span contenteditable="true">在实际的场景中，此变量在不同的环境中需要不同</span><code>-Dspring.profiles.active=&lt;profile&gt;</code> parameter to specify the configuration so that you can switch between different environments easily. <span contenteditable="true">参数来指定配置，以便您可以轻松地在不同的环境之间切换</span></td></tr></tbody></table>

### 4.6. Support Custom Namespaces

### 4.6. 支持自定义名称空间

For details about namespaces in Nacos, refer to [Nacos Concepts](https://nacos.io/zh-cn/docs/concepts.html)

有关 Nacos 中的名称空间的详细信息，请参阅 Nacos 概念

> Namespaces are used to isolate configurations for different tenants. Groups and Data IDs can be the same across different namespaces. Typical scenarios of namespaces is the isolation of configurations for different environments, for example, isolation between development/testing environments and production environments(configurations and services and so on). 名称空间用于隔离不同租户的配置。组和数据 ID 可以在不同的名称空间中相同。名称空间的典型场景是不同环境的配置隔离，例如，开发/测试环境与生产环境(配置和服务等)之间的隔离

The “Public” namespace of Nacos is used if no namespace is specified in `${spring.cloud.nacos.config.namespace}`. You can also specify a custom namespace in the following way：

如果 ${ spring.clod.Nacos.config.nampace }中没有指定名称空间，则使用 Nacos 的“ Public”名称空间。还可以按以下方式指定自定义命名空间:

```
spring.cloud.nacos.config.namespace=b3404bc0-d7dc-4855-b519-570ed34b62d7
```

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>This configuration must be in the bootstrap.properties file. The value of <span contenteditable="true">此配置必须在 bootstrap.properties 文件中</span><code>spring.cloud.nacos.config.namespace</code> is the id of the namespace, and the value of id can be retrieved from the Nacos console. Do not select other namespaces when adding configurations. Otherwise configurations cannot be retrieved properly. <span contenteditable="true">是名称空间的 id，id 的值可以从 Nacos 控制台检索到。添加配置时不要选择其他命名空间。否则无法正确检索配置</span></td></tr></tbody></table>

### 4.7. Support Custom Groups

### 4.7. 支援自订群组

DEFAULT\_GROUP is used by default when no `{spring.cloud.nacos.config.group}` configuration is defined. If you need to define your own group, you can define it in the following property:

当没有定义{ spring.cloud. nacos.config.group }配置时，默认使用 DEFAULT \_ GROUP。如果需要定义自己的组，可以在以下属性中定义:

```
spring.cloud.nacos.config.group=DEVELOP_GROUP
```

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>This configuration must be in the bootstrap.properties file, and the value of Group must be the same with the value of <span contenteditable="true">此配置必须在 bootstrap.properties 文件中，Group 的值必须与</span><code>spring.cloud.nacos.config.group</code>.</td></tr></tbody></table>

### 4.8. Support Custom Data Id

### 4.8. 支持自定义数据 ID

As of Spring Cloud Alibaba Nacos Config, data id can be self-defined. For detailed design of this part, refer to [Github issue](https://github.com/alibaba/spring-cloud-alibaba/issues/141). The following is a complete sample:

在阿里巴巴的 Spring Cloud Nacos Config 中，数据 id 可以自定义。有关此部分的详细设计，请参阅 Github 问题。下面是一个完整的例子:

```
spring.application.name=opensource-service-provider
spring.cloud.nacos.config.server-addr=127.0.0.1:8848

# config external configuration
# 1. Data Id is in the default group of DEFAULT_GROUP, and dynamic refresh of configurations is not supported.
spring.cloud.nacos.config.ext-config[0].data-id=ext-config-common01.properties

# 2. Data Id is not in the default group, and dynamic refresh of configurations is not supported.
spring.cloud.nacos.config.ext-config[1].data-id=ext-config-common02.properties
spring.cloud.nacos.config.ext-config[1].group=GLOBALE_GROUP

# 3. Data Id is not in the default group and dynamic referesh of configurations is supported.
spring.cloud.nacos.config.ext-config[2].data-id=ext-config-common03.properties
spring.cloud.nacos.config.ext-config[2].group=REFRESH_GROUP
spring.cloud.nacos.config.ext-config[2].refresh=true
```

-   Support multiple data ids by configuring `spring.cloud.nacos.config.ext-config[n].data-id`.
    
    通过配置 spring.clod.nacos.config.ext-config \[ n \] . data-id 支持多个数据 id。
    
-   Customize the group of data id by configuring `spring.cloud.nacos.config.ext-config[n].group`. If not specified, DEFAULT\_GROUP is used.
    
    通过配置 spring.clod.nacos.config.ext-config \[ n \] . group 来定制数据 id 组。
    
-   Control whether this data id supports dynamic refresh of configurations is supported when configurations are changed by configuring `spring.cloud.nacos.config.ext-config[n].refresh`. It’s not supported by default.
    
    通过配置 spring.clod.nacos.config.ext-config \[ n \] ，控制在更改配置时是否支持动态刷新配置。刷新。默认情况下不支持。
    

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>When multiple Data Ids are configured at the same time, the priority is defined by the value of “n” in <span contenteditable="true">当同时配置多个数据 ID 时，优先级由</span><code>spring.cloud.nacos.config.ext-config[n].data-id</code>. The bigger the value, the higher the priority. <span contenteditable="true">。价值越大，优先级越高</span></td></tr></tbody></table>

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>The value of <span>的价值</span><code>spring.cloud.nacos.config.ext-config[n].data-id</code> must have a file extension, and it could be properties or yaml/yml. The setting in <span contenteditable="true">必须有一个文件扩展名，它可以是属性或 yaml/yml</span><code>spring.cloud.nacos.config.file-extension</code> does not have any impact on the custom Data Id file extension. <span contenteditable="true">对自定义 DataId 文件扩展名没有任何影响</span></td></tr></tbody></table>

The configuration of custom Data Id allows the sharing of configurations among multiple applications, and also enables support of multiple configurations for one application.

自定义 DataId 的配置允许在多个应用程序之间共享配置，并且还允许为一个应用程序支持多个配置。

To share the data id among multiple applications in a clearer manner, you can also use the following method:

为了更清晰地在多个应用程序之间共享数据 id，您还可以使用以下方法:

```
spring.cloud.nacos.config.shared-dataids=bootstrap-common.properties,all-common.properties
spring.cloud.nacos.config.refreshable-dataids=bootstrap-common.properties
```

-   Multiple shared data ids can be configured using `spring.cloud.nacos.config.shared-dataids` , and the data ids are separted by commas.
    
    可以使用 spring.clod.nacos.config.share-dataid 配置多个共享数据 id，数据 id 用逗号分隔。
    
-   `spring.cloud.nacos.config.refreshable-dataids` is used to control which data ids will be refreshed dynamically when configurations are updated, and that the latest configuration values can be retrieved by applications. Data ids are separated with commas. If not specified, all shared data ids will not be dynamically refreshed.
    
    Refreshable-dataid 用于控制在更新配置时动态刷新哪些数据 id，以及应用程序可以检索到最新的配置值。数据 id 用逗号分隔。如果未指定，则不会动态刷新所有共享数据 ID。
    

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>When using <span>使用时</span><code>spring.cloud.nacos.config.shared-dataids</code> to configure multiple shared data ids, we agree on the following priority between the shared configurations: Priorities are decided based on the order in which the configurations appear. The one that occurs later is higher in priority than the one that appears first. <span contenteditable="true">为了配置多个共享数据 ID，我们在共享配置之间商定以下优先级: 根据配置出现的顺序决定优先级。后面出现的那个优先级比前面出现的那个高</span></td></tr></tbody></table>

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>When using <span>使用时</span><code>spring.cloud.nacos.config.shared-dataids</code>, the data Id must have a file extension, and it could be properties or yaml/yml. And the configuration in <span contenteditable="true">数据 Id 必须有一个文件扩展名，它可以是属性或 yaml/yml</span><code>spring.cloud.nacos.config.file-extension</code> does not have any impact on the customized Data Id file extension. <span contenteditable="true">不会对定制的 DataId 文件扩展名产生任何影响</span></td></tr></tbody></table>

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>When <span>什么时候</span><code>spring.cloud.nacos.config.refreshable-dataids</code> specifies the data ids that support dynamic refresh, the corresponding values of the data ids should also specify file extensions. <span contenteditable="true">指定支持动态刷新的数据 id，数据 id 的相应值也应指定文件扩展名</span></td></tr></tbody></table>

### 4.9. Nacos Config Endpoint

### 4.9. Nacos 配置端点

Nacos Config provides an Endpoint internally with a corresponding endpoint id of `nacos-config`.

Nacos Config 在内部提供一个端点，其中对应的端点 id 为 Nacos-Config。

Endpoint exposed json contains three properties:

端点暴露的 json 包含三个属性:

1.  Sources: Current application configuration data information
    
    来源: 当前应用程序配置数据信息
    
2.  RefreshHistory: Configuration refresh history
    
    刷新历史记录: 配置刷新历史记录
    
3.  NacosConfigProperties: Shows the current basic Nacos configurations of the current service
    
    NacosConfigProperties: 显示当前服务的当前基本 Nacos 配置
    

The followings shows how a service instance accesses the Endpoint:

下面显示了服务实例如何访问端点:

```
{
"NacosConfigProperties": {
"serverAddr": "127.0.0.1:8848",
"encode": null,
"group": "DEFAULT_GROUP",
"prefix": null,
"fileExtension": "properties",
"timeout": 3000,
"endpoint": null,
"namespace": null,
"accessKey": null,
"secretKey": null,
"contextPath": null,
"clusterName": null,
"name": null,
"sharedDataids": "base-common.properties,common.properties",
"refreshableDataids": "common.properties",
"extConfig": null
},
"RefreshHistory": [{
"timestamp": "2019-07-29 11:20:04",
"dataId": "nacos-config-example.properties",
"md5": "7d5d7f1051ff6571e2ec9f90887d9d91"
}],
"Sources": [{
"lastSynced": "2019-07-29 11:19:04",
"dataId": "common.properties"
}, {
"lastSynced": "2019-07-29 11:19:04",
"dataId": "base-common.properties"
}, {
"lastSynced": "2019-07-29 11:19:04",
"dataId": "nacos-config-example.properties"
}]
}
```

### 4.10. Disable Nacos Config AutoConfiguration

### 4.10. 禁用 Nacos 配置自动配置

set spring.cloud.nacos.config.enabled = false to disable Spring Cloud Nacos Config AutoConfiguration.

设置 Spring.clod.Nacos.Config.enable = false 禁用 Spring Cloud Nacos Config AutoConfiguration。

### 4.11. More Information about Nacos Config Starter Configurations

### 4.11. 更多有关 Nacos 配置启动器配置的信息

The following shows the other configurations of the starter of Nacos Config:

下面显示了 Nacos Config 启动器的其他配置:

<table><colgroup><col> <col> <col> <col></colgroup><tbody><tr><td><p>Configuration</p><p>配置</p></td><td><p>Key</p><p>钥匙</p></td><td><p>Default Value</p><p>默认值</p></td><td><p>Description</p><p>描述</p></td></tr><tr><td><p>Server address</p><p contenteditable="true">服务器地址</p></td><td><p><code>spring.cloud.nacos.config.server-addr</code></p><p contenteditable="true">Server-addr</p></td><td></td><td><p>IP and port of the Nacos Server listener</p><p contenteditable="true">Nacos 服务器侦听器的 IP 和端口</p></td></tr><tr><td><p>Dataid from nacos config</p><p contenteditable="true">来自 nacos 配置的数据</p></td><td><p><code>spring.cloud.nacos.config.name</code></p></td><td></td><td><p>First take the prefix, then go to the name, and finally take spring.application.name</p><p contenteditable="true">首先输入前缀，然后输入名字，最后输入 spring.application.name</p></td></tr><tr><td><p>Dataid from nacos config</p><p contenteditable="true">来自 nacos 配置的数据</p></td><td><p><code>spring.cloud.nacos.config.prefix</code></p><p>前缀</p></td><td></td><td><p>First take the prefix, then go to the name, and finally take spring.application.name</p><p contenteditable="true">首先输入前缀，然后输入名字，最后输入 spring.application.name</p></td></tr><tr><td><p>Encode for nacos config content</p><p contenteditable="true">对 nacos 配置内容进行编码</p></td><td><p><code>spring.cloud.nacos.config.encode</code></p><p contenteditable="true">Nacos.config.encode</p></td><td></td><td><p>Encode for nacos config content</p><p contenteditable="true">对 nacos 配置内容进行编码</p></td></tr><tr><td><p>GROUP for nacos config</p><p contenteditable="true">玉米饼配置组</p></td><td><p><code>spring.cloud.nacos.config.group</code></p><p contenteditable="true">Config. group</p></td><td><p><code>DEFAULT_GROUP</code></p><p contenteditable="true">默认值 _ group</p></td><td><p>GROUP for nacos config</p><p contenteditable="true">玉米饼配置组</p></td></tr><tr><td><p>The suffix of nacos config dataId, also the file extension of config content.</p><p contenteditable="true">Nacos config dataId 的后缀，也是 config 内容的文件扩展名。</p></td><td><p><code>spring.cloud.nacos.config.fileExtension</code></p><p contenteditable="true">Config. fileExtension</p></td><td><p><code>properties</code></p><p>物业</p></td><td><p>The suffix of nacos config dataId, also the file extension of config content(now support properties or yaml(yml))</p><p contenteditable="true">Nacos config dataId 的后缀，也是 config 内容的文件扩展名(现在支持属性或 yaml (yml))</p></td></tr><tr><td><p>Timeout for get config from nacos</p><p contenteditable="true">从 nacos 获取配置的超时</p></td><td><p><code>spring.cloud.nacos.config.timeout</code></p><p>超时</p></td><td><p><code>3000</code></p></td><td><p>Timeout for get config from nacos</p><p contenteditable="true">从 nacos 获取配置的超时</p></td></tr><tr><td><p>Endpoint</p><p>终点</p></td><td><p><code>spring.cloud.nacos.config.endpoint</code></p><p contenteditable="true">Config. endpoint</p></td><td></td><td><p>Endpoint</p><p>终点</p></td></tr><tr><td><p>Namespace</p><p>命名空间</p></td><td><p><code>spring.cloud.nacos.config.namespace</code></p><p>名称空间</p></td><td></td><td><p>Namespace</p><p>命名空间</p></td></tr><tr><td><p>AccessKey</p><p>访问密钥</p></td><td><p><code>spring.cloud.nacos.config.accessKey</code></p><p contenteditable="true">Nacos.config.accKey</p></td><td></td><td><p>Alibaba Cloud account accesskey</p><p contenteditable="true">阿里巴巴云端帐户存取密码匙</p></td></tr><tr><td><p>SecretKey</p><p contenteditable="true">“秘密钥匙”</p></td><td><p><code>spring.cloud.nacos.config.secretKey</code></p><p>配置密钥</p></td><td></td><td><p>Alibaba Cloud account secretkey</p><p contenteditable="true">阿里巴巴云端帐户密钥</p></td></tr><tr><td><p>The context path of Nacos Server</p><p contenteditable="true">Nacos 服务器的上下文路径</p></td><td><p><code>spring.cloud.nacos.config.contextPath</code></p><p contenteditable="true">Config. contextPath</p></td><td></td><td><p>The context path of Nacos Server</p><p contenteditable="true">Nacos 服务器的上下文路径</p></td></tr><tr><td><p>Cluster name</p><p>群集名称</p></td><td><p><code>spring.cloud.nacos.config.clusterName</code></p><p contenteditable="true">Conco.config.ClusterName</p></td><td></td><td><p>Cluster name</p><p>群集名称</p></td></tr><tr><td><p>Dataid for Shared Configuration</p><p contenteditable="true">共享配置的数据援助</p></td><td><p><code>spring.cloud.nacos.config.sharedDataids</code></p><p contenteditable="true">Config. sharedDataid</p></td><td></td><td><p>Dataid for Shared Configuration, split by ","</p><p contenteditable="true">共享配置的数据援助，分割方式为“ ,”</p></td></tr><tr><td><p>Dynamic refresh dataid for Shared Configuration</p><p contenteditable="true">共享配置的动态刷新数据援助</p></td><td><p><code>spring.cloud.nacos.config.refreshableDataids</code></p><p contenteditable="true">Config</p></td><td></td><td><p>Dynamic refresh dataid for Shared Configuration, split by ","</p><p contenteditable="true">共享配置的动态刷新数据援助，分为“ ,”</p></td></tr><tr><td><p>custom dataid</p><p contenteditable="true">自定义数据援助</p></td><td><p><code>spring.cloud.nacos.config.extConfig</code></p><p contenteditable="true">Config. extConfig</p></td><td></td><td><p>It’s a List，build up by <code>Config</code> POJO. <code>Config</code> has 3 attributes, <code>dataId</code>, <code>group</code> and <code>refresh</code></p><p contenteditable="true">它是一个 List，由 Config POJO 构建。 Config 有3个属性 dataId、 group 和 refresh</p></td></tr></tbody></table>

## 5\. Spring Cloud Alibaba Sentinel

## 5\. 《阿里巴巴哨兵报》

### 5.1. Introduction of Sentinel

### 5.1《哨兵站》简介

As microservices become popular, the stability of service calls is becoming increasingly important. [Sentinel](https://github.com/alibaba/Sentinel) takes "flow" as the breakthrough point, and works on multiple fields including flow control, circuit breaking and load protection to protect service reliability.

随着微服务的普及，服务调用的稳定性变得越来越重要。“哨兵”以“流量”为切入点，在流量控制、断路器、负载保护等多个领域开展工作，以保证服务的可靠性。

[Sentinel](https://github.com/alibaba/Sentinel) has the following features:

御天敌具有以下特点:

-   **Rich Scenarios**： Sentinel has supported the key scenarios of Alibaba’s Double 11 Shopping Festivals for over 10 years, such as second kill(i.e., controlling sudden bursts of traffic flow so that it’s within the acceptable range of the system capacity), message load shifting, circuit breaking of unreliable downstream applications.
    
    丰富的场景: 近十多年来，Sentinel 一直支持阿里巴巴双11购物节的关键场景，例如第二次杀死(即，控制突然爆发的流量，使其在系统容量可接受的范围内) ，消息负载转移，不可靠的下游应用程序断路。
    
-   **Comprehensive Real-Time Monitoring**： Sentinel provides real-time monitoring capability. You can see the monitoring data of your servers at the accuracy of seconds, and even the overall runtime status of a cluster with less than 500 nodes.
    
    全面实时监控: 哨兵提供实时监控能力。您可以以秒为单位查看服务器的监视数据，甚至可以查看少于500个节点的集群的整体运行时状态。
    
-   **Extensive Open-Source Ecosystem**： Sentinel provides out-of-box modules that can be easily integrated with other open-source frameworks/libraries, such as Spring Cloud, Dubbo, and gRPC. To use Sentinel, you only need to introduce the related dependency and make a few simple configurations.
    
    广泛的开源生态系统: Sentinel 提供开箱即用的模块，可以很容易地与其他开源框架/库集成，如 Spring Cloud、达博和 gRPC。要使用 Sentinel，您只需要引入相关的依赖项并进行一些简单的配置。
    
-   **Sound SPI Extensions**： Sentinel provides easy-to-use and sound SPI extension interfaces. You can customize logics with the SPI extensions quickly, for example, you can define your own rule management, or adapt to specific data sources.
    
    健全的 SPI 扩展: Sentinel 提供易于使用和健全的 SPI 扩展接口。您可以使用 SPI 扩展快速定制逻辑，例如，您可以定义自己的规则管理，或者适应特定的数据源。
    

### 5.2. How to Use Sentinel

### 5.2如何使用御天敌

If you want to use Sentinel in your project, please use the starter with the group ID as `com.alibaba.cloud` and the artifact ID as `spring-cloud-starter-alibaba-sentinel`.

如果您想在您的项目中使用 Sentinel，请使用具有组 ID com.alibaba.cloud 的 starter，以及具有 spring-cloud-starter-alibaba-Sentinel 的工件 ID。

```
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
</dependency>
```

The following is a simple example of how to use Sentinel:

下面是如何使用 Sentinel 的一个简单例子:

```
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(ServiceApplication.class, args);
    }

}

@RestController
public class TestController {

    @GetMapping(value = "/hello")
    @SentinelResource("hello")
    public String hello() {
        return "Hello Sentinel";
    }

}
```

The @SentinelResource annotation is used to identify if a resource is rate limited or degraded. In the above sample, the 'hello' attribute of the annotation refers to the resource name.

@ SentinelResource 注释用于识别资源是否受速率限制或降级。在上面的示例中，注释的‘ hello’属性引用资源名称。

@SentinelResource also provides attributes such as `blockHandler`, `blockHandlerClass`, and `fallback` to identify rate limiting or degradation operations. For more details, refer to [Sentinel Annotation Support](https://github.com/alibaba/Sentinel/wiki/%E6%B3%A8%E8%A7%A3%E6%94%AF%E6%8C%81).

@ SentinelResource 还提供诸如 lockHandler、 lockHandlerClass 和后退等属性来识别速率限制或降级操作。有关详细信息，请参阅哨兵注释支持。

The above examples are all used in the WebServlet environment. Sentinel currently supports WebFlux and needs to cooperate with the `spring-boot-starter-webflux` dependency to trigger the WebFlux-related automation configuration in sentinel starter.

上面的示例都在 WebServlet 环境中使用。Sentinel 目前支持 WebFlux，并且需要与 spring-boot-starter-webflow 依赖关系进行合作，以触发 Sentinel starter 中与 WebFlux 相关的自动化配置。

```
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(ServiceApplication.class, args);
    }

}

@RestController
public class TestController {

    @GetMapping("/mono")
    @SentinelResource("hello")
    public Mono<String> mono() {
return Mono.just("simple string")
.transform(new SentinelReactorTransformer<>("otherResourceName"));
    }

}
```

##### Sentinel Dashboard

##### 御天敌仪表盘

Sentinel dashboard is a lightweight console that provides functions such as machine discovery, single-server resource monitoring, overview of cluster resource data, as well as rule management. To use these features, you only need to complete a few steps.

哨兵仪表板是一个轻量级控制台，提供机器发现、单服务器资源监视、集群资源数据概览以及规则管理等功能。要使用这些特性，您只需要完成几个步骤。

**Note**: The statistics overview for clusters only supports clusters with less than 500 nodes, and has a latency of about 1 to 2 seconds.

注意: 集群的统计概述只支持少于500个节点的集群，延迟大约为1到2秒。

![dashboard](https://github.com/alibaba/Sentinel/wiki/image/dashboard.png)

Figure 3. Sentinel Dashboard 图3哨兵仪表盘

To use the Sentinel dashboard, simply complete the following 3 steps.

要使用 Sentinel 指示板，只需完成以下3个步骤。

###### Get the Dashboard

###### 去拿仪表盘

You can download the latest dashboard JAR file from the [Release Page](https://github.com/alibaba/Sentinel/releases).

您可以从发布页面下载最新的仪表板 JAR 文件。

You can also get the latest source code to build your own Sentinel dashboard：

您还可以获得最新的源代码来构建您自己的 Sentinel 指示板:

-   Download the [Dashboard](https://github.com/alibaba/Sentinel/tree/master/sentinel-dashboard) project.
    
    下载 Dashboard 项目。
    
-   Run the following command to package the code into a FatJar: `mvn clean package`
    
    运行以下命令将代码打包成 FatJar: mvn clean 包
    

###### Start the Dashboard

###### 启动仪表盘

Sentinel dashboard is a standard SpringBoot application, and you can run the JAR file in the Spring Boot mode.

Sentinel 指示板是一个标准的 SpringBoot 应用程序，您可以在 SpringBoot 模式下运行 JAR 文件。

```
java -Dserver.port=8080 -Dcsp.sentinel.dashboard.server=localhost:8080 -Dproject.name=sentinel-dashboard -jar sentinel-dashboard.jar
```

If there is conflict with the 8080 port, you can use `-Dserver.port=new port` to define a new port.

如果与8080端口有冲突，可以使用-Dserver.port = new port 来定义一个新端口。

#### 5.2.2. Configure the Dashboard

#### 5.2.2. 配置仪表板

application.yml

```
spring:
  cloud:
    sentinel:
      transport:
        port: 8719
        dashboard: localhost:8080
```

The port number specified in `spring.cloud.sentinel.transport.port` will start an HTTP Server on the corresponding server of the application, and this server will interact with the Sentinel dashboard. For example, if a rate limiting rule is added in the Sentinel dashboard, the the rule data will be pushed to and recieved by the HTTP Server, which in turn registers the rule to Sentinel.

Port 中指定的端口号将在应用程序的相应服务器上启动 HTTP 服务器，该服务器将与 Sentinel 指示板交互。例如，如果在 Sentinel 仪表板中添加了速率限制规则，则规则数据将被推送到 HTTP 服务器并由其接收，而 HTTP 服务器又将规则注册到 Sentinel。

For more information about Sentinel dashboard, please refer to [Sentinel Dashboard](https://github.com/alibaba/Sentinel/wiki/%E6%8E%A7%E5%88%B6%E5%8F%B0).

有关哨兵仪表板的详细信息，请参阅哨兵仪表板。

### 5.3. OpenFeign Support

### 5.3 OpenFeign 支持

Sentinel is compatible with the [OpenFeign](https://github.com/OpenFeign/feign) component. To use it, in addition to introducing the `sentinel-starter` dependency, complete the following 2 steps:

Sentinel 与 OpenFeign 组件兼容。要使用它，除了引入哨兵启动依赖外，还要完成以下两个步骤:

-   Enable the Sentinel support for feign in the properties file. `feign.sentinel.enabled=true`
    
    在属性文件中启用对佯装的哨兵支持
    
-   Add the `openfeign starter` dependency to trigger and enable `sentinel starter`:
    
    添加 openfeign 启动器依赖项来触发并启用前哨启动器:
    

```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

This is a simple usage of `FeignClient`:

这是 FeignClient 的一个简单用法:

```
@FeignClient(name = "service-provider", fallback = EchoServiceFallback.class, configuration = FeignConfiguration.class)
public interface EchoService {
    @GetMapping(value = "/echo/{str}")
    String echo(@PathVariable("str") String str);
}

class FeignConfiguration {
    @Bean
    public EchoServiceFallback echoServiceFallback() {
        return new EchoServiceFallback();
    }
}

class EchoServiceFallback implements EchoService {
    @Override
    public String echo(@PathVariable("str") String str) {
        return "echo fallback";
    }
}
```

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>The resource name policy in the corresponding interface of Feign is：httpmethod:protocol://requesturl. All the attributes in the <span contenteditable="true">Feign 对应接口中的资源名策略是: httpmethod: protocol://requesturl</span><code>@FeignClient</code> annotation is supported by Sentinel. <span contenteditable="true">Sentinel 支持注释</span></td></tr></tbody></table>

The corresponding resource name of the `echo` method in the `EchoService` interface is `GET:http://service-provider/echo/{str}`.

EchoService 接口中 echo 方法的对应资源名是 GET: http://service-provider/echo/{ str }。

### 5.4. RestTemplate Support

### 5.4. RestTemplate 支持

Spring Cloud Alibaba Sentinel supports the protection of `RestTemplate` service calls using Sentinel. To do this, you need to add the `@SentinelRestTemplate` annotation when constructing the `RestTemplate` bean.

阿里巴巴春云哨兵支持使用哨兵保护 RestTemplate 服务调用。为此，您需要在构建 RestTemplate bean 时添加@SentinelRestTemplate 注释。

```
@Bean
@SentinelRestTemplate(blockHandler = "handleException", blockHandlerClass = ExceptionUtil.class)
public RestTemplate restTemplate() {
    return new RestTemplate();
}
```

The attribute of the `@SentinelRestTemplate` annotation support flow control(`blockHandler`, `blockHandlerClass`) and circuit breaking(`fallback`, `fallbackClass`).

@ SentinelRestTemplate 注释的属性支持流控制(lockHandler，lockHandlerClass)和断路(Fallback，falbackClass)。

\==

The `blockHandler` or `fallback` is the static method of `blockHandlerClass` or `fallbackClass`.

LockHandler 或 Fallback 是 lockHandlerClass 或 falbackClass 的静态方法。

The parameter and return value of method in `@SentinelRestTemplate` is same as `org.springframework.http.client.ClientHttpRequestInterceptor#interceptor`, but it has one more parameter `BlockException` to catch the exception by Sentinel.

@ SentinelRestTemplate 中方法的参数和返回值与 org.springframework.http.client 相同。ClientHttpRequestInterceptor # 拦截器，但它还有一个参数 BlockException 来捕获 Sentinel 的异常。

The method signature of `handleException` in `ExceptionUtil` above should be like this:

上面 ExceptionUtil 中 handleException 的方法签名应该如下所示:

```
public class ExceptionUtil {
    public static ClientHttpResponse handleException(HttpRequest request, byte[] body, ClientHttpRequestExecution execution, BlockException exception) {
        ...
    }
}
```

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>When the application starts, it will check if the <span contenteditable="true">当应用程序启动时，它将检查</span><code>@SentinelRestTemplate</code> annotation corresponding to the flow control or circuit breaking method exists, if it does not exist, it will throw an exception. <span contenteditable="true">注释对应的流量控制或断路方法存在，如果不存在，它将抛出异常</span></td></tr></tbody></table>

The attribute of the `@SentinelRestTemplate` annotation is optional.

@ SentinelRestTemplate 注释的属性是可选的。

It will return `RestTemplate request block by sentinel` when you using `RestTemplate` blocked by Sentinel. You can override it by your own logic. We provide `SentinelClientHttpResponse` to handle the response.

当您使用被哨兵阻止的 RestTemplate 时，它将逐个返回 RestTemplate 请求块。你可以用自己的逻辑来覆盖它。我们提供 SentinelClientHttpResponse 来处理响应。

Sentinel RestTemplate provides two granularities for resource rate limiting:

Sentinel RestTemplate 为限制资源利率提供了两种粒度:

-   `httpmethod:schema://host:port/path`： Protocol, host, port and path
    
    模式://host: port/path: Protocol，host，port and path
    
-   `httpmethod:schema://host:port`： Protocol, host and port
    
    模式://host: port: Protocol，host and port
    

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>Take Http GET <span contenteditable="true">以 Http GET 为例</span><code><a href="https://www.taobao.com/test">https://www.taobao.com/test</a></code> as an example. The corresponding resource names have two levels of granularities, <span contenteditable="true">对应的资源名称具有两个级别的粒度,</span><code>GET:https://www.taobao.com</code> and <span>还有</span><code>GET:https://www.taobao.com/test</code>.</td></tr></tbody></table>

### 5.5. Dynamic Data Source Support

### 5.5动态数据源支持

`SentinelProperties` provide `datasource` attribute to configure datasource.

SentinelProperties 提供数据源属性来配置数据源。

For example, 4 data sources are configures：

例如，4个数据源是配置:

```
spring.cloud.sentinel.datasource.ds1.file.file=classpath: degraderule.json
spring.cloud.sentinel.datasource.ds1.file.rule-type=flow

#spring.cloud.sentinel.datasource.ds1.file.file=classpath: flowrule.json
#spring.cloud.sentinel.datasource.ds1.file.data-type=custom
#spring.cloud.sentinel.datasource.ds1.file.converter-class=JsonFlowRuleListConverter
#spring.cloud.sentinel.datasource.ds1.file.rule-type=flow

spring.cloud.sentinel.datasource.ds2.nacos.server-addr=127.0.0.1:8848
spring.cloud.sentinel.datasource.ds2.nacos.data-id=sentinel
spring.cloud.sentinel.datasource.ds2.nacos.group-id=DEFAULT_GROUP
spring.cloud.sentinel.datasource.ds2.nacos.data-type=json
spring.cloud.sentinel.datasource.ds2.nacos.rule-type=degrade

spring.cloud.sentinel.datasource.ds3.zk.path = /Sentinel-Demo/SYSTEM-CODE-DEMO-FLOW
spring.cloud.sentinel.datasource.ds3.zk.server-addr = localhost:2181
spring.cloud.sentinel.datasource.ds3.zk.rule-type=authority

spring.cloud.sentinel.datasource.ds4.apollo.namespace-name = application
spring.cloud.sentinel.datasource.ds4.apollo.flow-rules-key = sentinel
spring.cloud.sentinel.datasource.ds4.apollo.default-flow-rule-value = test
spring.cloud.sentinel.datasource.ds4.apollo.rule-type=param-flow
```

This method follows the configuration of Spring Cloud Stream Binder. `TreeMap` is used for storage internally, and comparator is `String.CASE_INSENSITIVE_ORDER`.

此方法遵循 Spring Cloud Stream Binder 的配置。 TreeMap 用于内部存储，比较器为 String.CASE \_ INSENSITIVE \_ ORDER。

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>d1, ds2, ds3, ds4 are the names of <span contenteditable="true">D1，ds2，ds3，ds4是</span><code>ReadableDataSource</code>, and can be coded as you like. The <span contenteditable="true">，并且可以根据您的喜好进行编码</span><code>file</code>, <code>zk</code>, <code>nacos</code> , <code>apollo</code> refer to the specific data sources. The configurations following them are the specific configurations of these data sources respecitively. <span contenteditable="true">下面的配置分别是这些数据源的特定配置</span></td></tr></tbody></table>

Every data source has 3 common configuration items: `data-type`, `converter-class` and `rule-type`.

每个数据源都有3个常见的配置项: 数据类型、转换器类和规则类型。

`data-type` refers to `Converter`. Spring Cloud Alibaba Sentinel provides two embedded values by default: `json` and `xml` (the default is json if not specified). If you do not want to use the embedded `json` or `xml` `Converter`, you can also fill in `custom` to indicate that you will define your own `Converter`, and then configure the `converter-class`. You need to specify the full path of the class for this configuration.

Data-type 指的是 Converter。Spring Cloud 阿里巴巴 Sentinel 默认提供两个嵌入值: json 和 xml (如果没有指定，默认值是 json)。如果不想使用嵌入式 json 或 xml Converter，还可以填写自定义内容，指示您将定义自己的 Converter，然后配置转换器类。您需要为此配置指定类的完整路径。

`rule-type` refers to the rule type in datasource(`flow`，`degrade`，`authority`，`system`, `param-flow`, `gw-flow`, `gw-api-group`).

Rule-type 是指数据源中的规则类型(流、降级、权限、系统、参数流、 gw-flow、 gw-api-group)。

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>XML format is not supported by default. To make it effective, you need to add the <span contenteditable="true">默认情况下不支持 XML 格式</span><code>jackson-dataformat-xml</code> dependency. <span>依赖性</span></td></tr></tbody></table>

To learn more about how dynamic data sources work in Sentinel, refer to [Dynamic Rule Extension](https://github.com/alibaba/Sentinel/wiki/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%99%E6%89%A9%E5%B1%95).

要了解更多关于动态数据源如何在 Sentinel 中工作的信息，请参考动态规则扩展。

### 5.6. Support Spring Cloud Gateway

### 支持 Spring Cloud Gateway

If you want to use Sentinel Starter with Spring Cloud Gateway, you need to add the `spring-cloud-alibaba-sentinel-gateway` dependency and add the `spring-cloud-starter-gateway` dependency to let Spring Cloud Gateway AutoConfiguration class in the module takes effect:

如果你想在 Spring Cloud Gateway 中使用 Sentinel Starter，你需要添加 Spring-Cloud-alibaba-Sentinel-Gate 依赖关系，并添加 Spring-Cloud-Starter-Gate 依赖关系，这样模块中的 Spring Cloud Gateway AutoConfiguration 类才能生效:

```
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
</dependency>

<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-alibaba-sentinel-gateway</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
```

### 5.7. Circuit Breaker: Spring Cloud Circuit Breaker With Sentinel & Configuring Sentinel Circuit Breakers

### 5.7. 断路器: 带哨兵的弹簧云断路器及配置哨兵断路器

#### 5.7.1. Default Configuration

#### 5.7.1默认配置

To provide a default configuration for all of your circuit breakers create a `Customizer` bean that is passed a `SentinelCircuitBreakerFactory` or `ReactiveSentinelCircuitBreakerFactory`. The `configureDefault` method can be used to provide a default configuration.

要为所有断路器提供默认配置，请创建一个定制器 bean，该 bean 通过一个 SentinelCircuitBreakerFactory 或 ReactiveSentinelCircuitBreakerFactory。ConfigureDefault 方法可用于提供默认配置。

```
@Bean
public Customizer<SentinelCircuitBreakerFactory> defaultCustomizer() {
return factory -> factory.configureDefault(id -> new SentinelConfigBuilder(id)
.build());
}
```

You can choose to provide default circuit breaking rules via `SentinelConfigBuilder#rules(rules)`. You can also choose to load circuit breaking rules later elsewhere using `DegradeRuleManager.loadRules(rules)` API of Sentinel, or via Sentinel dashboard.

您可以选择通过 SentinelConfigBuilder # rules (rules)提供默认断路规则。您还可以选择稍后在其他地方使用 Sentinel 的 DegradeRuleManager.loadRules (rules) API 或通过 Sentinel 仪表板加载断路规则。

##### Reactive Example

##### 被动反应示例

```
@Bean
public Customizer<ReactiveSentinelCircuitBreakerFactory> defaultCustomizer() {
return factory -> factory.configureDefault(id -> new SentinelConfigBuilder(id)
.build());
}
```

#### 5.7.2. Specific Circuit Breaker Configuration

#### 5.7.2特定断路器配置

Similarly to providing a default configuration, you can create a `Customizer` bean this is passed a `SentinelCircuitBreakerFactory`.

与提供默认配置类似，您可以创建一个 Customizer bean，它被传递一个 SentinelCircuitBreakerFactory。

```
@Bean
public Customizer<SentinelCircuitBreakerFactory> slowCustomizer() {
String slowId = "slow";
List<DegradeRule> rules = Collections.singletonList(
new DegradeRule(slowId).setGrade(RuleConstant.DEGRADE_GRADE_RT)
.setCount(100)
.setTimeWindow(10)
);
return factory -> factory.configure(builder -> builder.rules(rules), slowId);
}
```

##### Reactive Example

##### 被动反应示例

```
@Bean
public Customizer<ReactiveSentinelCircuitBreakerFactory> customizer() {
List<DegradeRule> rules = Collections.singletonList(
new DegradeRule().setGrade(RuleConstant.DEGRADE_GRADE_RT)
.setCount(100)
.setTimeWindow(10)
);
return factory -> factory.configure(builder -> builder.rules(rules), "foo", "bar");
}
```

### 5.8. Sentinel Endpoint

### 5.8哨兵终端

Sentinel provides an Endpoint internally with a corresponding endpoint id of `sentinel`.

Sentinel 在内部为端点提供了一个对应的端点 id。

Endpoint exposed json contains multi properties:

端点暴露的 json 包含多个属性:

1.  appName: application name
    
    AppName: 应用程序名
    
2.  logDir: the directory of log
    
    LogDir: log 目录
    
3.  logUsePid: log name with pid ot not
    
    LogUsePid: 带有 pid ot not 的 log 名称
    
4.  blockPage: redirect page after sentinel block
    
    重定向前哨块后的页面
    
5.  metricsFileSize: the size of metrics file
    
    度量文件的大小
    
6.  metricsFileCharset: metrics file charset
    
    指标文件字符集
    
7.  totalMetricsFileCount: the total file count of of metrics file
    
    Total MetricsFileCount: 度量文件的总文件计数
    
8.  consoleServer: sentinel dashboard address
    
    前哨仪表板地址
    
9.  clientIp: client ip
    
    客户 IP
    
10.  heartbeatIntervalMs: client heartbeat interval with dashboard
     
    HearbeatIntervalMs: 带指示板的客户端心跳间隔
    
11.  clientPort: the client needs to expose the port to interact with the dashboard
     
    ClientPort: 客户端需要公开端口以与仪表板进行交互
    
12.  coldFactor: cold factor
     
    冷因子: 冷因子
    
13.  filter: CommonFilter related properties, such as order, urlPatterns and enable
     
    Filter: CommonFilter 相关属性，例如 order、 urlPattern 和 able
    
14.  datasource: datasource configuration info by client
     
    数据源: 客户端的数据源配置信息
    
15.  rules: the rule that the client takes effect internally contains flowRules, degradeRules, systemRules, authorityRule, paramFlowRule
     
    Rules: 客户机在内部生效的规则包含 flofRules、 decleRules、 systemRules、 authorityRule、 parmFlowRule
    

The followings shows how a service instance accesses the Endpoint:

下面显示了服务实例如何访问端点:

```
{
"blockPage": null,
"appName": "sentinel-example",
"consoleServer": "localhost:8080",
"coldFactor": "3",
"rules": {
"flowRules": [{
"resource": "GET:http://www.taobao.com",
"limitApp": "default",
"grade": 1,
"count": 0.0,
"strategy": 0,
"refResource": null,
"controlBehavior": 0,
"warmUpPeriodSec": 10,
"maxQueueingTimeMs": 500,
"clusterMode": false,
"clusterConfig": null
}, {
"resource": "/test",
"limitApp": "default",
"grade": 1,
"count": 0.0,
"strategy": 0,
"refResource": null,
"controlBehavior": 0,
"warmUpPeriodSec": 10,
"maxQueueingTimeMs": 500,
"clusterMode": false,
"clusterConfig": null
}, {
"resource": "/hello",
"limitApp": "default",
"grade": 1,
"count": 1.0,
"strategy": 0,
"refResource": null,
"controlBehavior": 0,
"warmUpPeriodSec": 10,
"maxQueueingTimeMs": 500,
"clusterMode": false,
"clusterConfig": null
}]
},
"metricsFileCharset": "UTF-8",
"filter": {
"order": -2147483648,
"urlPatterns": ["/*"],
"enabled": true
},
"totalMetricsFileCount": 6,
"datasource": {
"ds1": {
"file": {
"dataType": "json",
"ruleType": "FLOW",
"converterClass": null,
"file": "...",
"charset": "utf-8",
"recommendRefreshMs": 3000,
"bufSize": 1048576
},
"nacos": null,
"zk": null,
"apollo": null,
"redis": null
}
},
"clientIp": "30.5.121.91",
"clientPort": "8719",
"logUsePid": false,
"metricsFileSize": 52428800,
"logDir": "...",
"heartbeatIntervalMs": 10000
}
```

### 5.9. Configuration

### 5.9. 配置

The following table shows that when there are corresponding bean types in `ApplicationContext`, some actions will be taken:

下表显示，当 ApplicationContext 中有相应的 bean 类型时，将采取一些操作:

<table><colgroup><col> <col> <col></colgroup><tbody><tr><td><p>Existing Bean Type</p><p contenteditable="true">现有的豆类</p></td><td><p>Action</p><p>开拍</p></td><td><p>Function</p><p>功能</p></td></tr><tr><td><p><code>UrlCleaner</code></p></td><td><p><code>WebCallbackManager.setUrlCleaner(urlCleaner)</code></p><p contenteditable="true">SetUrlCleaner (urlCleaner)</p></td><td><p>Resource cleaning(resource(for example, classify all URLs of /foo/:id to the /foo/* resource))</p><p contenteditable="true">资源清理(资源(例如，将/foo/: id 的所有 URL 分类到/foo/* 资源)</p></td></tr><tr><td><p><code>UrlBlockHandler</code></p></td><td><p><code>WebCallbackManager.setUrlBlockHandler(urlBlockHandler)</code></p><p contenteditable="true">SetUrlBlockHandler (urlBlockHandler)</p></td><td><p>Customize rate limiting logic</p><p contenteditable="true">自定义速率限制逻辑</p></td></tr><tr><td><p><code>RequestOriginParser</code></p></td><td><p><code>WebCallbackManager.setRequestOriginParser(requestOriginParser)</code></p><p contenteditable="true">SetRequestOriginParser (requestOriginParser)</p></td><td><p>Setting the origin</p><p>设定原点</p></td></tr></tbody></table>

The following table shows all the configurations of Spring Cloud Alibaba Sentinel:

下表显示所有阿里巴巴春云哨兵的配置:

<table><colgroup><col> <col> <col></colgroup><tbody><tr><td><p>Configuration</p><p>配置</p></td><td><p>Description</p><p>描述</p></td><td><p>Default Value</p><p>默认值</p></td></tr><tr><td><p><code>spring.application.name</code> or <code>project.name</code></p><p contenteditable="true">Spring.application.name 或项目名称</p></td><td><p>Project Name Of Sentinel</p><p contenteditable="true">哨兵计划名称</p></td><td></td></tr><tr><td><p><code>spring.cloud.sentinel.enabled</code></p><p contenteditable="true">启用了 spring.cloud</p></td><td><p>Whether Sentinel automatic configuration takes effect</p><p contenteditable="true">哨兵自动配置是否生效</p></td><td><p>true</p><p>没错</p></td></tr><tr><td><p><code>spring.cloud.sentinel.eager</code></p><p contenteditable="true">Spring Cloud 乌云，哨兵，渴望</p></td><td><p>Whether to trigger Sentinel initialization in advance</p><p contenteditable="true">是否提前触发哨兵初始化</p></td><td><p>false</p><p>假的</p></td></tr><tr><td><p><code>spring.cloud.sentinel.transport.port</code></p><p contenteditable="true">Spring.cloud Sentinel Transport t.port</p></td><td><p>Port for the application to interact with Sentinel dashboard. An HTTP Server which uses this port will be started in the application</p><p contenteditable="true">应用程序与 Sentinel 仪表板交互的端口。使用此端口的 HTTP 服务器将在应用程序中启动</p></td><td><p>8719</p></td></tr><tr><td><p><code>spring.cloud.sentinel.transport.dashboard</code></p><p contenteditable="true">Spring.cloud 哨兵，运输，仪表盘</p></td><td><p>Sentinel dashboard address</p><p contenteditable="true">哨兵仪表盘地址</p></td><td></td></tr><tr><td><p><code>spring.cloud.sentinel.transport.heartbeatIntervalMs</code></p><p contenteditable="true">Spring.cloud Sentinel.Transport t.hearbeatIntervalMs</p></td><td><p>Hearbeat interval between the application and Sentinel dashboard</p><p contenteditable="true">应用程序和 Sentinel 仪表板之间的心跳间隔</p></td><td></td></tr><tr><td><p><code>spring.cloud.sentinel.transport.client-ip</code></p><p contenteditable="true">Spring.Cloud.Sentinel.Transport t.client-ip</p></td><td><p>The client IP of this configuration will be registered to the Sentinel Server side.</p><p contenteditable="true">此配置的客户端 IP 将注册到哨兵服务器端。</p></td><td></td></tr><tr><td><p><code>spring.cloud.sentinel.filter.order</code></p><p contenteditable="true">Spring.clod.Sentinel.filter.order</p></td><td><p>Loading order of Servlet Filter. The filter will be constructed in the Starter</p><p contenteditable="true">Servlet 过滤器的加载顺序。过滤器将在启动器中构建</p></td><td><p>Integer.MIN_VALUE</p><p contenteditable="true">MIN _ VALUE</p></td></tr><tr><td><p><code>spring.cloud.sentinel.filter.url-patterns</code></p><p contenteditable="true">过滤器，网址-模式</p></td><td><p>Data type is array. Refers to the collection of Servlet Filter ULR patterns</p><p contenteditable="true">数据类型是数组。指 Servlet 过滤器 ULR 模式的集合</p></td><td><p>/*</p></td></tr><tr><td><p><code>spring.cloud.sentinel.filter.enabled</code></p><p contenteditable="true">启用了 spring.cloud</p></td><td><p>Enable to instance CommonFilter</p><p contenteditable="true">启用实例 CommonFilter</p></td><td><p>true</p><p>没错</p></td></tr><tr><td><p><code>spring.cloud.sentinel.metric.charset</code></p><p contenteditable="true">Spring.Cloud SentinelMetric.charset</p></td><td><p>metric file character set</p><p contenteditable="true">公制文件字符集公制文件字符集</p></td><td><p>UTF-8</p></td></tr><tr><td><p><code>spring.cloud.sentinel.metric.fileSingleSize</code></p></td><td><p>Sentinel metric single file size</p><p contenteditable="true">哨兵单文件大小</p></td><td></td></tr><tr><td><p><code>spring.cloud.sentinel.metric.fileTotalCount</code></p></td><td><p>Sentinel metric total file number</p><p contenteditable="true">哨兵指标文件总数</p></td><td></td></tr><tr><td><p><code>spring.cloud.sentinel.log.dir</code></p></td><td><p>Directory of Sentinel log files</p><p contenteditable="true">哨兵日志文件目录</p></td><td></td></tr><tr><td><p><code>spring.cloud.sentinel.log.switch-pid</code></p><p contenteditable="true">Switch-pid</p></td><td><p>If PID is required for Sentinel log file names</p><p contenteditable="true">如果哨兵日志文件名称需要 PID</p></td><td><p>false</p><p>假的</p></td></tr><tr><td><p><code>spring.cloud.sentinel.servlet.blockPage</code></p></td><td><p>Customized redirection URL. When rate limited, the request will be redirected to the pre-defined URL</p><p contenteditable="true">自定义重定向 URL。当速率受限时，请求将被重定向到预定义的 URL</p></td><td></td></tr><tr><td><p><code>spring.cloud.sentinel.flow.coldFactor</code></p><p contenteditable="true">Spring.Cloud SentinelFlow ColdFactor</p></td><td><p><a href="https://github.com/alibaba/Sentinel/wiki/%E9%99%90%E6%B5%81---%E5%86%B7%E5%90%AF%E5%8A%A8">ColdFactor</a></p></td><td><p>3</p></td></tr><tr><td><p><code>spring.cloud.sentinel.scg.fallback.mode</code></p></td><td><p>Response mode after Spring Cloud Gateway circuit break (select <code>redirect</code> or <code>response</code>)</p><p contenteditable="true">弹簧云网关断路后的响应模式(选择重定向或响应)</p></td><td></td></tr><tr><td><p><code>spring.cloud.sentinel.scg.fallback.redirect</code></p><p contenteditable="true">Spring.clod.Sentinel.scg.falback. redirect</p></td><td><p>Spring Cloud Gateway response mode is the redirect URL corresponding to 'redirect' mode</p><p contenteditable="true">SpringCloudGateway 响应模式是对应于“重定向”模式的重定向 URL</p></td><td></td></tr><tr><td><p><code>spring.cloud.sentinel.scg.fallback.response-body</code></p><p contenteditable="true">Spring.Cloud.Sentinel.scg.falback response-body</p></td><td><p>Spring Cloud Gateway response mode is response content corresponding to 'response' mode</p><p contenteditable="true">SpringCloudGateway 响应模式是对应于“响应”模式的响应内容</p></td><td></td></tr><tr><td><p><code>spring.cloud.sentinel.scg.fallback.response-status</code></p><p>回复状态</p></td><td><p>Spring Cloud Gateway response mode is the response code corresponding to 'response' mode</p><p contenteditable="true">Spring Cloud Gateway 响应模式是与“ response”模式对应的响应代码</p></td><td><p>429</p></td></tr><tr><td><p><code>spring.cloud.sentinel.scg.fallback.content-type</code></p><p contenteditable="true">Content-type</p></td><td><p>The Spring Cloud Gateway response mode is the content-type corresponding to the 'response' mode.</p><p contenteditable="true">Spring Cloud Gateway 响应模式是与“ response”模式对应的内容类型。</p></td><td><p>application/json</p><p contenteditable="true">应用程序/json</p></td></tr></tbody></table>

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>These configurations will only take effect in servlet environment. RestTemplate and Feign will not take effect for these configurations. <span contenteditable="true">这些配置仅在 servlet 环境中生效，RestTemplate 和 Feign 不会对这些配置生效</span></td></tr></tbody></table>

## 6\. Spring Cloud Alibaba RocketMQ Binder

## 6\. 阿里巴巴春云火箭 MQ 活页夹

### 6.1. Introduction of RocketMQ

### 6.1. RocketMQ 简介

[RocketMQ](https://rocketmq.apache.org/) is an open-source distributed message system. It is based on highly available distributed cluster technologies and provides message publishing and subscription service with low latency and high stability. RocketMQ is widely used in a variety of industries, such as decoupling of asynchronous communication, enterprise solutions, financial settlements, telecommunication, e-commerce, logistics, marketing, social media, instant messaging, mobile applications, mobile games, videos, IoT, and Internet of Vehicles.

RocketMQ 是一个开源的分布式消息系统。它基于高度可用的分布式集群技术，提供低延迟和高稳定性的消息发布和订阅服务。RocketMQ 广泛应用于多个行业，如异步通信解耦、企业解决方案、金融结算、电信、电子商务、物流、营销、社交媒体、即时通讯、移动应用、手机游戏、视频、物联网和车辆互联网。

It has the following features:

它具有以下特点:

-   Strict order of message sending and consumption
    
    严格的消息发送和消费顺序
    
-   Rich modes of message pulling
    
    丰富的消息拉取模式
    
-   Horizontal scalability of consumers
    
    消费者的水平可伸缩性
    
-   Real-time message subscription
    
    实时邮件订阅
    
-   Billion-level message accumulation capability
    
    十亿级消息积累能力
    

### 6.2. RocketMQ Usages

### 6.2 RocketMQ 用法

-   Download RocketMQ
    
    下载 RocketMQ
    

The decompressed directory is as follows:

解压缩后的目录如下:

```
apache-rocketmq
├── LICENSE
├── NOTICE
├── README.md
├── benchmark
├── bin
├── conf
└── lib
```

-   Start NameServer
    
    启动 NameServer
    

```
nohup sh bin/mqnamesrv &
tail -f ~/logs/rocketmqlogs/namesrv.log
```

-   Start Broker
    
    开始吧，布洛克
    

```
nohup sh bin/mqbroker -n localhost:9876 &
tail -f ~/logs/rocketmqlogs/broker.log
```

-   Send and Receive Messages
    
    发送和接收消息
    

```
sh bin/tools.sh org.apache.rocketmq.example.quickstart.Producer
```

Output when the message is successfully sent: `SendResult [sendStatus=SEND_OK, msgId= …`

成功发送消息时的输出: SendResult \[ sendStatus = SEND \_ OK，msgId = ..

```
sh bin/tools.sh org.apache.rocketmq.example.quickstart.Consumer
```

Output when the message is successfully received： `ConsumeMessageThread_%d Receive New Messages: [MessageExt…`

成功接收邮件时的输出: ConsumeMessageThread \_% d ReceiveNewMessages: \[ MessageExt..

-   Disable Server
    
    关闭服务器
    

```
sh bin/mqshutdown broker
sh bin/mqshutdown namesrv
```

### 6.3. Introduction of Spring Cloud Stream

### 6.3. Spring Cloud Stream 介绍

Spring Cloud Stream is a microservice framework used to build architectures based on messages. It helps you to create production-ready single-server Spring applications based on SpringBoot, and connects with Broker using `Spring Integration`.

SpringCloudStream 是一个微服务框架，用于构建基于消息的体系结构。它可以帮助您基于 SpringBoot 创建生产就绪的单服务器 Spring 应用程序，并使用 Spring Integration 与 Broker 连接。

Spring Cloud Stream provides unified abstractions of message middleware configurations, and puts forward concepts such as publish-subscribe, consumer groups and partition.

Spring Cloud Stream 提供了消息中间件配置的统一抽象，并提出了发布-订阅、消费者组和分区等概念。

There are two concepts in Spring Cloud Stream: Binder and Binding

Spring Cloud Stream 中有两个概念: Binder 和 Binding

-   Binder: A component used to integrate with external message middleware, and is used to create binding. Different message middleware products have their own binder implementations.
    
    Binder: 用于与外部消息中间件集成的组件，用于创建绑定。不同的消息中间件产品有自己的活页夹实现。
    

For example, `Kafka` uses `KafkaMessageChannelBinder`, `RabbitMQ` uses `RabbitMessageChannelBinder`, while `RocketMQ` uses `RocketMQMessageChannelBinder`.

例如，Kafka 使用 KafkaMessageChannelBinder，RabbitMQ 使用 RabbitMessageChannelBinder，而 RocketMQ 使用 RocketMQMessageChannelBinder。

-   Binding: Includes Input Binding and Output Binding.
    
    绑定: 包括输入绑定和输出绑定。
    

Binding serves as a bridge between message middleware and the provider and consumer of the applications. Developers only need to use the Provider or Consumer to produce or consume data, and do not need to worry about the interactions with the message middleware.

绑定作为消息中间件与应用程序的提供者和使用者之间的桥梁。开发人员只需使用提供者或消费者生成或使用数据，不需要担心与消息中间件的交互。

![SCSt overview](SpringCloud%20Alibaba.assets/SCSt-overview-1676858858541-15.png)

Figure 4. Spring Cloud Stream 图4. Spring Cloud Stream

Now let’s use Spring Cloud Stream to write a simple code for sending and receiving messages:

现在让我们使用 Spring Cloud Stream 编写一个发送和接收消息的简单代码:

```
MessageChannel messageChannel = new DirectChannel();

// Message subscription
((SubscribableChannel) messageChannel).subscribe(new MessageHandler() {
    @Override
    public void handleMessage(Message<? > message) throws MessagingException {
        System.out.println("receive msg: " + message.getPayload());
    }
});

// Message sending
messageChannel.send(MessageBuilder.withPayload("simple msg").build());
```

All the message types in this code are provided by the \`spring-messaging\`module. It shields the lower-layer implementations of message middleware. If you would like to change the message middleware, you only need to configure the related message middleware information in the configuration file and modify the binder dependency.

此代码中的所有消息类型都由“ spring-message”模块提供。它屏蔽了消息中间件的底层实现。如果希望更改消息中间件，只需在配置文件中配置相关的消息中间件信息并修改绑定器依赖项。

**The lower layer of Spring Cloud Stream also implements various code abstractions based on the previous code.**

SpringCloudStream 的下层还基于前面的代码实现各种代码抽象。

### 6.4. How to use Spring Cloud Alibaba RocketMQ Binder

### 如何使用阿里巴巴云计算 RocketmQ 绑定器

For using the Spring Cloud Alibaba RocketMQ Binder, you just need to add it to your Spring Cloud Stream application, using the following Maven coordinates:

要使用阿里巴巴的 Spring Cloud RocketMQ Binder，你只需要将它添加到你的 Spring Cloud Stream 应用程序中，使用以下的 Maven 坐标:

```
<dependency>
  <groupId>com.alibaba.cloud</groupId>
  <artifactId>spring-cloud-stream-binder-rocketmq</artifactId>
</dependency>
```

Alternatively, you can also use the Spring Cloud Stream RocketMQ Starter:

或者，你也可以使用 Spring Cloud Stream RocketMQ Starter:

```
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-stream-rocketmq</artifactId>
</dependency>
```

### 6.5. How Spring Cloud Alibaba RocketMQ Binder Works

### 6.5. 阿里巴巴春云 RocketmQ Binder 的工作原理

This is the implementation architecture of Spring Cloud Stream RocketMQ Binder:

这是 Spring Cloud Stream RocketMQ Binder 的实现架构:

![TB1v8rcbUY1gK0jSZFCXXcwqXXa 1236 773](SpringCloud%20Alibaba.assets/TB1v8rcbUY1gK0jSZFCXXcwqXXa-1236-773-1676858858541-17.png)

Figure 5. SCS RocketMQ Binder 图5. SCS RocketMQ Binder

The implementation of RocketMQ Binder depend on the [RocketMQ-Spring](https://github.com/apache/rocketmq-spring) framework.

RocketMQ Binder 的实现取决于 RocketMQ-Spring 框架。

RocketMQ Spring framework is an integration of RocketMQ and Spring Boot. It provides three main features:

RocketMQ Spring 框架是 RocketMQ 和 Spring Boot 的集成，它提供了三个主要特性:

1.  `RocketMQTemplate`: Sending messages, including synchronous, asynchronous, and transactional messages.
    
    RocketMQTemplate: 发送消息，包括同步、异步和事务性消息。
    
2.  `@RocketMQTransactionListener`: Listen and check for transaction messages.
    
    @ RocketMQTransactionListener: 侦听并检查事务消息。
    
3.  `@RocketMQMessageListener`: Consume messages.
    
    @ RocketMQMessageListener: 消费消息。
    

`RocketMQMessageChannelBinder` is a standard implementation of Binder, it will build `RocketMQInboundChannelAdapter` and `RocketMQMessageHandler` internally.

RocketMQMessageChannelBinder 是 Binder 的标准实现，它将在内部构建 RocketMQInbound ChannelAdapter 和 RocketMQMessageHandler。

`RocketMQMessageHandler` will construct `RocketMQTemplate` based on the Binding configuration. `RocketMQTemplate` will convert the `org.springframework.messaging.Message` message class of `spring-messaging` module to the RocketMQ message class `org.apache.rocketmq.common .message.Message` internally, then send it out.

RocketMQMessageHandler 将基于 Binding 配置构造 RocketMQTemplate。RocketMQTemplate 将转换 org.springframework.message。Spring 消息传递模块的消息消息类到 RocketMQ 消息类 org.apache.RocketMQ.common。信息。内部消息，然后发送出去。

`RocketMQInboundChannelAdapter` will also construct `RocketMQListenerBindingContainer` based on the Binding configuration, and `RocketMQListenerBindingContainer` will start the RocketMQ `Consumer` to receive the messages.

RocketMQInbound ChannelAdapter 还将基于 Binding 配置构造 RocketMQListenerBindingContainer，RocketMQListenerBindingContainer 将启动 RocketMQ 用户来接收消息。

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>RocketMQ Binder Application can also be used to configure rocketmq.** to trigger RocketMQ Spring related AutoConfiguration <span contenteditable="true">RocketMQ Binder 应用程序还可以用来配置 RocketMQ.* * 以触发 RocketMQ Spring 相关的 AutoConfiguration</span></td></tr></tbody></table>

Currently Binder supports setting the relevant key in `Header` to set the properties of the RocketMQ message.

目前，Binder 支持在 Header 中设置相关键以设置 RocketMQ 消息的属性。

For example, `TAGS`, `DELAY`, `TRANSACTIONAL_ARG`, `KEYS`, `WAIT_STORE_MSG_OK`, `FLAG` represent the labels corresponding to the RocketMQ message.

例如，TAGS、 DELAY、 TRANSACTION \_ ARG、 KEYS、 WAIT \_ STORE \_ MSG \_ OK、 FLAG 表示对应于 RocketMQ 消息的标签。

```
MessageBuilder builder = MessageBuilder.withPayload(msg)
    .setHeader(RocketMQHeaders.TAGS, "binder")
    .setHeader(RocketMQHeaders.KEYS, "my-key")
    .setHeader(MessageConst.PROPERTY_DELAY_TIME_LEVEL, "1");
Message message = builder.build();
output().send(message);
```

Or use StreamBridge

或者用 StreamBridge

```
MessageBuilder builder = MessageBuilder.withPayload(msg)
    .setHeader(RocketMQHeaders.TAGS, "binder")
    .setHeader(RocketMQHeaders.KEYS, "my-key")
    .setHeader(MessageConst.PROPERTY_DELAY_TIME_LEVEL, "1");
Message message = builder.build();
streamBridge.send("producer-out-0", message);
```

### 6.6. Configuration Options

### 6.6. 配置选项

#### 6.6.1. RocketMQ Binder Properties

#### 6.6.1. RocketMQ 粘合剂属性

spring.cloud.stream.rocketmq.binder.name-server

The name server of RocketMQ Server(Older versions use the namesrv-addr configuration item).

RocketMQ 服务器的名称服务器(旧版本使用 namesrv-addr 形态项目)。

Default: `127.0.0.1:9876`.

默认值: 127.0.0.1:9876。

spring.cloud.stream.rocketmq.binder.access-key Access-key

The AccessKey of Alibaba Cloud Account.

阿里巴巴云端帐户的登入密码。

spring.cloud.stream.rocketmq.binder.secret-key Spring.clod.stream Rocketmq.binder.secret-key

The SecretKey of Alibaba Cloud Account.

阿里巴巴云端帐户的密钥。

spring.cloud.stream.rocketmq.binder.enable-msg-trace 启用-msg-trace

Enable Message Trace feature for all producers and consumers.

为所有生产者和消费者启用消息跟踪特性。

spring.cloud.stream.rocketmq.binder.customized-trace-topic 自定义跟踪主题

The trace topic for message trace.

消息跟踪的跟踪主题。

Default: `RMQ_SYS_TRACE_TOPIC`.

默认值: RMQ \_ SYS \_ TRACE \_ TOPIC。

spring.cloud.stream.rocketmq.binder.access-channel 通道

The commercial version of rocketmq message trajectory topic is adaptive,the value is CLOUD

商业版的 Rocketmq 消息弹道主题是自适应的，值是 CLOUD

#### 6.6.2. RocketMQ Consumer Properties

The following properties are available for RocketMQ producers only and must be prefixed with `spring.cloud.stream.rocketmq.bindings.<channelName>.consumer.`.

以下属性只对 RocketMQ 生产者可用，并且必须以 spring.cloud. stream. RocketMQ.bindings 为前缀。

enable 启动

Enable Consumer Binding.

启用消费者绑定。

tags 标签

Consumer subscription tags expression, tags split by `||`.

消费者订阅标记表达式，标记由 | | 分割。

sql

Consumer subscription sql expression.

使用者订阅 sql 表达式。

broadcasting 广播

Control message mode, if you want all subscribers receive message all message, broadcasting is a good choice.

控制消息模式，如果想让所有订阅者收到所有消息，广播是一个不错的选择。

Default: `false`.

默认值: false。

orderly 勤杂工

Receiving message concurrently or orderly.

并发地或有序地接收消息。

Default: `false`.

默认值: false。

delayLevelWhenNextConsume 当下一次消费时

Message consume retry strategy for concurrently consume:

并发消费的消息消费重试策略:

-   \-1,no retry,put into DLQ directly
    
    \-1，无需重试，直接输入 DLQ
    
-   0,broker control retry frequency
    
    代理控制重试频率
    
-   \>0,client control retry frequency
    
    \> 0，客户端控制重试频率
    

suspendCurrentQueueTimeMillis 暂停当前队列时间米利斯

Time interval of message consume retry for orderly consume.

消息使用的时间间隔为有序使用而重试。

#### 6.6.3. RocketMQ Provider Properties

#### 6.6.3 RocketMQ Provider Properties

The following properties are available for RocketMQ producers only and must be prefixed with `spring.cloud.stream.rocketmq.bindings.<channelName>.producer.`.

以下属性只对 RocketMQ 生产者可用，并且必须以 spring.cloud. stream. RocketMQ.bindings 为前缀。

enable 启动

Enable Producer Binding.

启用生产者绑定。

group 小组

Producer group name.

制片人组名。

maxMessageSize

Maximum allowed message size in bytes.

允许的最大消息大小(以字节为单位)。

Default: `8249344`.

默认值: 8249344。

transactional 交易

Send Transactional Message.

发送事务性消息。

Default: `false`.

默认值: false。

sync 同步

Send message in synchronous mode.

以同步模式发送消息。

Default: `false`.

默认值: false。

vipChannelEnabled 启用 vipChannel

Send message with vip channel.

用 VIP 频道发送消息。

sendMessageTimeout 发送消息超时

Millis of send message timeout.

发送消息超时的 Millis。

compressMessageBodyThreshold 消息体阈值

Compress message body threshold, namely, message body larger than 4k will be compressed on default.

压缩消息体阈值，即默认情况下大于4k 的消息体将被压缩。

retryTimesWhenSendFailed 当发送失败时 retryTimesWhen 发送失败

Maximum number of retry to perform internally before claiming sending failure in synchronous mode.

声明在同步模式下发送失败之前在内部执行的最大重试次数。

retryTimesWhenSendAsyncFailed 当 SendA 同步失败时

Maximum number of retry to perform internally before claiming sending failure in asynchronous mode.

以异步模式声明发送失败之前在内部执行的最大重试次数。

retryNextServer

Indicate whether to retry another broker on sending failure internally.

指示是否在内部发送失败时重试另一个代理。

Default: `false`.

默认值: false。

## 7\. 阿里巴巴云计算

ANS(Application Naming Service) is a component of EDAS. Spring Cloud Alibaba Cloud ANS provides the commercial version of service registration and discovery in conformity with the Spring Cloud specifications, so that you can develop your applications locally and run them on the cloud.

ANS (应用程序命名服务)是 EDAS 的一个组件。阿里巴巴云计算 ANS 提供符合 Spring Cloud 规范的服务注册和发现的商业版本，以便您可以在本地开发应用程序并在云上运行它们。

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>EDAS currently supports direct deployment of Nacos Discovery applications <span contenteditable="true">EDAS 目前支持直接部署 Nacos Discovery 应用程序</span></td></tr></tbody></table>

### 7.1. How to Introduce Spring Cloud Alibaba Cloud ANS

### 7.1. 如何引入阿里巴巴云端服务

If you want to use ANS in your project, please use the starter with the group ID as `com.alibaba.cloud` and the artifact ID as `spring-cloud-starter-alicloud-ans`.

如果您想在您的项目中使用 ANS，请使用带有组 ID com.alibaba.cloud 的 starter，以及带有 spring-cloud-starter-alicloud-ANS 的工件 ID。

```
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alicloud-ans</artifactId>
</dependency>
```

### 7.2. Use ANS to Register Service

### 7.2使用「注册易」登记服务

When Spring Cloud AliCloud ANS Starter is introduced on the client, the metadata of the service such as IP, port number and weright will be registered to the registration center automatically. The client will maintain heartbeat with the server to prove that it is capable of providing service properly.

当客户端引入 Spring Cloud AliCloud ANS Starter 时，服务的元数据(如 IP、端口号和权重)将自动注册到注册中心。客户端将与服务器一起维护心跳，以证明它能够正确地提供服务。

The following is a simple illustration.

下面是一个简单的说明。

```
@SpringBootApplication
@EnableDiscoveryClient
@RestController
public class ProviderApplication {

    @RequestMapping("/")
    public String home() {
        return "Hello world";
    }

    public static void main(String[] args) {
        SpringApplication.run(ProviderApplication.class, args);
    }

}
```

As the service will registered to the registration center, we will need to configure the address of the registration center. We also need to add the following address in application.properties.

由于服务将注册到注册中心，我们需要配置注册中心的地址。我们还需要在 application.properties 中添加以下地址。

```
# The application name will be used as the service name, therefore it is mandatory.
spring.application.name=ans-provider
server.port=18081
# The following is the IP and port number of the registration center.
spring.cloud.alicloud.ans.server-list=127.0.0.1
spring.cloud.alicloud.ans.server-port=8080
```

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>By now the registration center is not started yet, so you will get an error message if your application is started. Therefore, start the registration center before you start your application. <span contenteditable="true">现在注册中心还没有启动，所以如果您的应用程序已经启动，您将收到一条错误消息。因此，在启动应用程序之前，请先启动注册中心</span></td></tr></tbody></table>

### 7.3. Start Registration Center

### 7.3开始注册中心

ANS uses two types of registration centers. One is the free lightweight configuration center and the other is the registration center on cloud, which is provided through EDAS. Generally, you can use the lightweight version for application development and local testing, and use EDAS for canary deployment or production.

ANS 使用两种类型的注册中心。一个是免费的轻量级配置中心，另一个是通过 EDAS 提供的云注册中心。通常，您可以使用轻量级版本进行应用程序开发和本地测试，并使用 EDAS 进行部署或生产。

#### 7.3.1. Start Lightweight Configuration Center

#### 启动轻量级配置中心

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>You only need to perform step 1(Download lightweight configuration center) and step 2(Start lightweight configuration center). Step 3(Configure hosts) is not required if you use ANS at the same time. <span contenteditable="true">您只需要执行步骤1(下载轻量级配置中心)和步骤2(启动轻量级配置中心)。如果同时使用 ANS，则不需要步骤3(配置主机)</span></td></tr></tbody></table>

After you start the lightweight configuration center, start ProviderApplication directly, and you will be able to register your service to the configuration center. The default port of the lightweight configuration center is 8080, therefore you can open [http://127.0.0.1:8080](http://127.0.0.1:8080/), click “Services” on the left and see the registered service.

启动轻量级配置中心之后，直接启动 ProviderApplication，就可以将服务注册到配置中心。轻量级配置中心的默认端口是8080，因此你可以打开 http://127.0.0.1:8080，点击左边的“服务”，查看注册的服务。

#### 7.3.2. User Registration Center on the Cloud

#### 7.3.2. 云上的用户注册中心

Using the registration center on the cloud saves you from the tedious work of server maintenance while at the same time provides a better stability. There is no difference at the code level between using the registration center on cloud and lightweight configuration center, but there are some differences in configurations.

使用云上的注册中心可以使您避免单调乏味的服务器维护工作，同时提供更好的稳定性。在云上使用注册中心和轻量级配置中心在代码级别上没有区别，但是在配置上有一些区别。

The following is a simple sample of using the registration center on the cloud.

下面是在云上使用注册中心的一个简单示例。

```
# The application name will be used the service name, and is therefore mandatory.
spring.application.name=ans-provider
# Configure your own port number
server.port=18081
# The following is the IP and port number of the configuration center. The default value is 127.0.0.1 and 8080, so the following lines can be omitted.
spring.cloud.alicloud.ans.server-mode=EDAS
spring.cloud.alicloud.access-key=Your Alibaba Cloud AK
spring.cloud.alicloud.secret-key=Your Alibaba Cloud SK
spring.cloud.alicloud.edas.namespace=cn-xxxxx
```

The default value of server-mode is LOCAL. If you want to use the registration center on cloud, you need to change it to EDAS.

Server-mode 的默认值是 LOCAL。如果您想在云上使用注册中心，则需要将其更改为 EDAS。

Access-key and secret-key are the AK/SK of your Alibaba Cloud account. Register an Alibaba Cloud account first and log on to the Cloud Console [Alibaba Cloud AK/SK](https://usercenter.console.aliyun.com/#/manage/ak) to copy your AccessKey ID and Access Key Secret. If you haven’t created one, click the “Create AccessKey” button.

访问密钥和机密密钥是你的阿里巴巴云帐户的 AK/SK。先注册一个阿里巴巴云帐户，然后登录到云控制台阿里巴巴云 AK/SK，复制你的 AccessKey ID 和 Access Key Secret。如果您还没有创建一个，请单击“ CreateAccessKey”按钮。

Namespace is a concept in EDAS, which is used to isolate environments, such as testing environment and production environment. To find your namespace, click to [Sign up for EDAS](https://common-buy.aliyun.com/?spm=5176.11451019.0.0.6f5965c0Uq5tue&commodityCode=edaspostpay#/buy) first. You will not be charged under the pay-as-you-go mode. Then log on to the [EDAS Console](https://edas.console.aliyun.com/#/namespaces?regionNo=cn-hangzhou) and you will be able to see your namespace, for example cn-hangzhou.

命名空间是 EDAS 中的一个概念，用于隔离环境，比如测试环境和生产环境。要查找名称空间，请先单击“注册 EDAS”。您将不会在现收现付模式下被收费。然后登录到 EDAS 控制台，您将能够看到您的名称空间，例如 cn-hangzhou。

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>EDAS provides application hosting service and will fill in all configurations automatically for the hosted applications. <span contenteditable="true">EDAS 提供应用程序宿主服务，并将自动填写宿主应用程序的所有配置</span></td></tr></tbody></table>

## 8\. Spring Cloud Alibaba Cloud ACM

## 8\. 阿里巴巴云计算

Spring Cloud AliCloud ACM is an implementation of the commercial product Application Configuration Management(ACM) in the client side of Spring Cloud, and is free of charge.

Spring Cloud AliCloud ACM 是 Spring Cloud 客户端商业产品应用程序组态管理(ACM)的一个实现，并且是免费的。

Use Spring Cloud AliCloud ACM to quickly access ACM configuration management capabilities based on Spring Cloud’s programming model.

根据 Spring Cloud 的编程模型，使用 Spring Cloud AliCloud ACM 快速访问 ACM 组态管理功能。

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>Currently EDAS already supports direct deployment of the Nacos Config app. <span contenteditable="true">目前，EDAS 已经支持直接部署 Nacos Config 应用程序</span></td></tr></tbody></table>

### 8.1. How to Introduce Spring Cloud Alibaba Cloud ACM

### 8.1. 如何引入阿里巴巴云端

If you want to use ACM in your project, please use the starter with the group ID as `com.alibaba.cloud` and the artifact ID as `spring-cloud-starter-alicloud-acm`.

如果您想在您的项目中使用 ACM，请使用带有组 ID com.alibaba.cloud 的 starter，以及带有 spring-cloud-starter-alicloud-ACM 的工件 ID。

```
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alicloud-acm</artifactId>
</dependency>
```

### 8.2. Use ACM to Manage Configurations

### 8.2. 使用 ACM 管理配置

When Spring Cloud Alibaba Cloud ACM Starter is introduced into the client, the application will automatically get configuration information from the configuration management server when it starts, and inject the configuration into Spring Environment.

当客户端引入阿里巴巴云计算 ACM 启动器时，应用程序将在启动时自动从组态管理服务器获取配置信息，并将配置注入到 Spring Environment 中。

The following is a simple illustration.

下面是一个简单的说明。

```
@SpringBootApplication
public class ProviderApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext applicationContext = SpringApplication.run(ProviderApplication.class, args);
        String userName = applicationContext.getEnvironment().getProperty("user.name");
        String userAge = applicationContext.getEnvironment().getProperty("user.age");
        System.err.println("user name :" +userName+"; age: "+userAge);
    }
}
```

As we need to obtain configuration information from the configuration server, we will need to configure the address of the server. We also need to add the following information in bootstrap.properties.

由于我们需要从配置服务器获取配置信息，因此需要配置服务器的地址。我们还需要在 bootstrap.properties 中添加以下信息。

```
# Required. The application name will be used as part of the keyword to get the configuration key from the server.
spring.application.name=acm-config
server.port=18081
# The following is the IP and port number of the configuration server.
spring.cloud.alicloud.acm.server-list=127.0.0.1
spring.cloud.alicloud.acm.server-port=8080
```

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>By now the configuration center is not started yet, so you will get an error message if your application is started. Therefore, start the configuration center before you start your application. <span contenteditable="true">现在配置中心还没有启动，因此如果您的应用程序已经启动，您将收到一条错误消息。因此，在启动应用程序之前先启动配置中心</span></td></tr></tbody></table>

#### 8.2.1. Start Configuration Center

#### 8.2.1启动配置中心

ACM uses two types of configuration centers. One is lightweight configuration center, the other is ACM which is used on Alibaba Cloud. Generally, you can use the lightweight version for application development and local testing, and use ACM for canary deployment or production.

ACM 使用两种类型的配置中心。一个是轻量级配置中心，另一个是阿里巴巴云上使用的 ACM。通常，您可以使用轻量级版本进行应用程序开发和本地测试，并使用 ACM 进行部署或生产。

##### Use Lightweight Configuration Center

##### 使用轻量级配置中心

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>You only need to perform step 1(Download lightweight configuration center) and step 2(Start lightweight configuration center). <span contenteditable="true">您只需要执行步骤1(下载轻量级配置中心)和步骤2(启动轻量级配置中心)</span></td></tr></tbody></table>

##### Use ACM on the Alibaba Cloud

##### 在阿里巴巴云上使用 ACM

Using ACM on the cloud saves you from the tedious work of server maintenance while at the same time provides a better stability. There is no difference at the code level between using ACM on cloud and lightweight configuration center, but there are some differences in configurations.

在云上使用 ACM 可以使您避免单调乏味的服务器维护工作，同时提供更好的稳定性。在云上使用 ACM 和轻量级配置中心在代码级别上没有区别，但是在配置上有一些区别。

The following is a simple sample of using ACM. You can view configuration details on [ACM Console](https://acm.console.aliyun.com/)

下面是一个使用 ACM 的简单示例。您可以在 ACM 控制台上查看配置详细信息

```
# The application name will be used as part of the keyword to obtain configuration key from the server, and is mandatory.
spring.application.name=acm-config
# Configure your own port number
server.port=18081
# The following is the IP and port number of the configuration center.
spring.cloud.alicloud.acm.server-mode=EDAS
spring.cloud.alicloud.access-key=Your Alibaba Cloud AK
spring.cloud.alicloud.secret-key=Your Alibaba Cloud SK
spring.cloud.alicloud.acm.endpoint=acm.aliyun.com
spring.cloud.alicloud.acm.namespace=Your ACM namespace(You can find the namespace on the ACM console)
```

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>EDAS provides application hosting service and will fill in all configurations about ACM automatically for the hosted applications. <span contenteditable="true">EDAS 提供应用程序宿主服务，并将为宿主应用程序自动填写有关 ACM 的所有配置</span></td></tr></tbody></table>

#### 8.2.2. Add Configuration in the Configuration Center

#### 8.2.2. 在配置中心添加配置

1.  After you start the lightweight configuration center, add the following configuration on the console.
    
    启动轻量级配置中心后，在控制台上添加以下配置。
    

```
Group:      DEFAULT_GROOUP

DataId:     acm-config.properties

Content:    user.name=james
            user.age=18
```

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>The format of dataId is <span contenteditable="true">DataId 的格式是</span><code>{prefix}. {file-extension}</code>. “prefix” is obtained from spring.application.name by default, and the value of “file-extension” is "properties” by default.</td></tr></tbody></table>

#### 8.2.3. Start Application Verification

#### 8.2.3开始应用程序验证

Start the following example and you can see that the value printed on the console is the value we configured in the lightweight configuration center.

启动下面的示例，可以看到控制台上打印的值是我们在轻量级配置中心中配置的值。

```
user name :james; age: 18
```

### 8.3. Modify Configuration File Extension

### 修改配置文件扩展名

The default file extension of dataId in spring-cloud-starter-alicloud-acm is properties. In addition to properties, yaml is also supported. You can set the file extension using spring.cloud.alicloud.acm.file-extension. Just set it to `yaml` or \`yml\`for yaml format.

Spring-cloud-starter-alicloud-acm 中 dataId 的默认文件扩展名是 properties。除了属性之外，还支持 yaml。您可以使用 spring.clod.alicloud.acm.file 扩展名来设置文件扩展名。只要设置为 yaml 或‘ yml’为 yaml 格式。

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>After you change the file extension, you need to make corresponding format changes in the DataID and content of the configuration center. <span contenteditable="true">更改文件扩展名后，需要在 DataID 和配置中心的内容中进行相应的格式更改</span></td></tr></tbody></table>

### 8.4. Dynamic Configuration Refresh

### 8.4. 动态配置刷新

spring-cloud-starter-alicloud-acm supports dynamic configuration updates. RefreshEvent in Spring is published when you update configuration in the configuration center. All classes with @RefreshScope and @ConfigurationProperties annotations will be refreshed automatically.

Spring-cloud-starter-alicloud-acm 支持动态配置更新。当您在配置中心更新配置时，将发布 Spring 中的 RefreshEvent。所有带有@RefreshScope 和@ConfigurationProperties 注释的类都将自动刷新。

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>You can disable automatic refresh by this setting: spring.cloud.alicloud.acm.refresh.enabled=false</td></tr></tbody></table>

### 8.5. Configure Profile Granularity

### 8.5. 配置配置文件粒度

When configuration is loaded by spring-cloud-starter-alicloud-acm, configuration with DataId {spring.application.name}. {file-extension} will be loaded first. If there is content in spring.profiles.active, the content of spring.profile, and configuration with the dataid format of{spring.application.name}-{profile}. {file-extension} will also be loaded in turn, and the latter has higher priority.

当通过 spring-cloud-starter-alicloud-acm 加载配置时，使用 DataId { spring.application.name }进行配置。将首先加载{ file-扩展名}。如果 spring.profiles.active 中有内容，那么 spring.profile 的内容，以及使用{ spring.application.name }-{ profile }的 dataid 格式的配置。也将依次加载{ file-extension } ，后者具有更高的优先级。

spring.profiles.active is the configuration metadata, and should also be configured in bootstrap.properties or bootstrap.yaml. For example, you can add the following content in bootstrap.properties.

Active 是配置元数据，也应该在 bootstrap.properties 或 bootstrap.yaml 中配置。例如，可以在 bootstrap.properties 中添加以下内容。

```
spring.profiles.active={profile-name}
```

Note: You can also configure the granularity through JVM parameters such as -Dspring.profiles.active=develop or --spring.profiles.active=develop, which have higher priority. Just follow the specifications of Spring Boot.

注意: 还可以通过 JVM 参数配置粒度，例如-Dspring.profiles.active = development 或—— spring.profiles.active = development，这些参数具有更高的优先级。只要遵循 Spring Boot 的规范就可以了。

### 8.6. Support Custom ACM Timeout

### 8.6. 支持自定义 ACM 超时

the default timeout of ACM client get config from sever is 3000 ms . If you need to define a timeout, set configuration `spring.cloud.alicloud.acm.timeout`,the unit is millisecond.

ACM 客户端从服务器获取配置的默认超时为3000ms。如果需要定义超时，请设置配置 spring.clod.alicloud.acm.timeout，该单元为毫秒。

### 8.7. Support Custom Group Configurations

### 8.7. 支持自定义组配置

DEFAULT\_GROUP is used by default when no `{spring.cloud.alicloud.acm.group}` configuration is defined. If you need to define your own group, you can use the following method:

当没有定义{ spring.cloud. alicloud.acm.group }配置时，默认使用 DEFAULT \_ GROUP。如果您需要定义自己的组，可以使用以下方法:

```
spring.cloud.alicloud.acm.group=DEVELOP_GROUP
```

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>This configuration must be placed in the bootstrap.properties file, and the value of Group must be the same with the value of <span contenteditable="true">此配置必须放在 bootstrap.properties 文件中，Group 的值必须与</span><code>spring.cloud.alicloud.acm.group</code>.</td></tr></tbody></table>

#### 8.7.1. Support Shared Configurations

#### 8.7.1. 支持共享配置

ACM provides a solution to share the same configuration across multiple applications. You can do this by adding the `spring.application.group` configuration in Bootstrap.

ACM 提供了跨多个应用程序共享相同配置的解决方案。为此，可以在 Bootstrap 中添加 spring.application.group 配置。

```
spring.application.group=company.department.team
```

Then, you application will retrieve configurations from the following DataId in turn before it retrieves its own configuration: company:application.properties, company.department:application.properties, company.department.team:application.properties. After that, it also retrieves configuration from {spring.application.group}: {spring.application.name}. {file-extension} The later in order, the higer the priority, and the unique configuration of the application itself has the highest priority.

然后，应用程序将依次从以下 DataId 中检索配置，然后再检索自己的配置: company: application.properties、 company.department: application.properties、 company.department.team: application.properties。之后，它还从{ spring.application.group } : { spring.application.name }检索配置。{文件扩展名}顺序越晚，优先级越高，应用程序本身的唯一配置具有最高的优先级。

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>The default suffix of DataId is properties, and you can change it using spring.cloud.alicloud.acm.file-extension. <code>{spring.application.group}: {spring.application.name}. {file-extension}</code> .</td></tr></tbody></table>

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>If you configured <span contenteditable="true">如果你配置了</span><code>spring.profiles.active</code> , then the DataId format of <span contenteditable="true">的 DataId 格式</span><code>{spring.application.group}: {spring.application.name}-{spring.profiles.active}. {file-extension}</code> is also supported, and has higher priority than <span contenteditable="true">也支持，并且优先级高于</span><code>{spring.application.group}: {spring.application.name}. {file-extension}</code></td></tr></tbody></table>

### 8.8. Actuator Endpoint

### 8.8执行器端点

the Actuator endpoint of ACM is `/acm`, `config` represents the ACM metadata configuration information, `runtime.sources` corresponds to the configuration information obtained from the ACM server and the last refresh time, `runtime.refreshHistory` corresponds to the dynamic refresh history.

ACM 的致动器端点是/ACM，config 表示 ACM 元数据配置信息，runtime.source 对应于从 ACM 服务器获得的配置信息，最后一次刷新时间 runtime.resh History 对应于动态刷新历史。

## 9\. Spring Cloud Alibaba Cloud OSS

## 9\. 阿里巴巴云计算开放源码软件

OSS（Object Storage Service）is a storage product on Alibaba Cloud. Spring Cloud Alibaba Cloud OSS provides the commercialized storage service in conformity with Spring Cloud specifications. We provide easy-to-use APIs and supports the integration of Resource in the Spring framework.

OSS (对象存储服务)是阿里巴巴云上的存储产品。阿里巴巴云计算开放源码软件提供符合 Spring Cloud 规范的商业化存储服务。我们提供易于使用的 API，并支持在 Spring 框架中集成 Resource。

### 9.1. How to Introduce Spring Cloud Alibaba Cloud OSS

### 9.1. 如何介绍阿里巴巴云计算开放源码软件

We’ve released Spring Cloud Alibaba version 0.2.1. You will need to add dependency management POM first.

我们已经发布了阿里巴巴版本0.2.1。

```
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-dependencies</artifactId>
            <version>0.2.2.BUILD-SNAPSHOT</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

Next we need to introduce Spring Cloud Alibaba Cloud OSS Starter.

接下来我们需要介绍阿里巴巴云计算开源软件。

```
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alicloud-oss</artifactId>
</dependency>
```

### 9.2. How to Use OSS API

### 9.2. 如何使用 OSS API

#### 9.2.1. Configure OSS

#### 9.2.1. 配置 OSS

Before you start to use Spring Cloud Alibaba Cloud OSS, please add the following configurations in application.properties.

在开始使用阿里巴巴云计算开放源码软件之前，请在 application.properties 中添加以下配置。

```
spring.cloud.alicloud.access-key=Your Alibaba Cloud AK
spring.cloud.alicloud.secret-key=Your Alibaba Cloud SK
spring.cloud.alicloud.oss.endpoint=***.aliyuncs.com
```

access-key and secret-key is the AK/SK of your Alibaba Cloud account. If you don’t have one, please register an account first, and log on to [Alibaba Cloud AK/SK Management](https://usercenter.console.aliyun.com/#/manage/ak) to get your AccessKey ID and Access Key Secret . If you haven’t create the AccessKeys, click “Create AccessKey” to create one.

Access-key 和 secret-key 是阿里巴巴云帐户的 AK/SK。如果您没有，请先注册一个帐户，登录阿里巴巴云 AK/SK 管理获得您的访问密钥 ID 和访问密钥机密。如果尚未创建 AccessKey，请单击“创建 AccessKey”以创建一个。

For endpoint information, please refer to the OSS [Documentation](https://help.aliyun.com/document_detail/31837.html) and get the endpoint for your region.

有关端点信息，请参考 OSS 文档并获取您所在地区的端点。

#### 9.2.2. Introduce OSS API

#### 9.2.2. 介绍 OSS API

The OSS API of Spring Cloud Alibaba Cloud OSS is based on the official OSS SDK, and includes APIs for uploading, downloading, viewing files.

阿里巴巴云计算的开放源码软件接口基于官方的开放源码软件开发工具包，包括用于上传、下载和查看文件的接口。

Here is a simple application that uses the OSS API.

下面是一个使用 OSS API 的简单应用程序。

```
@SpringBootApplication
public class OssApplication {

    @Autowired
    private OSS ossClient;

    @RequestMapping("/")
    public String home() {
        ossClient.putObject("bucketName", "fileName", new FileInputStream("/your/local/file/path"));
        return "upload success";
    }

    public static void main(String[] args) throws URISyntaxException {
        SpringApplication.run(OssApplication.class, args);
    }

}
```

Log on to the [OSS Console](https://oss.console.aliyun.com/overview), click “Create New Bucket” and create a bucket as instructed. Replace the bucket name in the “bucketname” of the previous code with your new bucket name. "fileName” can be any name you like, and "/your/local/file/path” can be any local file path. Next you can run \`curl [http://127.0.0.1:port](https://spring-cloud-alibaba-group.github.io/github-pages/2021/en-us/index.html#_introductionhttp://127.0.0.1:port) number/ to upload your files, and you will see your file on the [OSS Console](https://oss.console.aliyun.com/overview).

登录到 OSS 控制台，点击“创建新的桶”并按照指示创建一个桶。用新的桶名替换前面代码的“桶名”中的桶名。“ fileName”可以是您喜欢的任何名称，“/your/local/file/path”可以是任何本地文件路径。接下来，你可以运行“ curl http://127.0.0.1:port 号/”来上传你的文件，然后你就可以在操作系统控制台上看到你的文件了。

For more instructions on OSS APIs, please refer to [OSS SDK Documentation](https://help.aliyun.com/document_detail/32008.html).

有关 OSS API 的详细说明，请参阅 OSS SDK 文档。

### 9.3. Integrate with the Resource Specifications of Spring

### 9.3. 与 Spring 的资源规范集成

Spring Cloud Alibaba Cloud OSS integrates the Resource of the Spring framework, which allows you to use the OSS resources easily.

阿里巴巴云计算开放源码软件集成了 Spring 框架的资源，这使得你可以很容易的使用开放源码软件资源。

The following is a simple example of how to use Resource.

下面是如何使用 Resource 的简单示例。

```
@SpringBootApplication
public class OssApplication {

    @Value("oss://bucketName/fileName")
    private Resource file;

    @GetMapping("/file")
    public String fileResource() {
        try {
            return "get file resource success. content: " + StreamUtils.copyToString(
                file.getInputStream(), Charset.forName(CharEncoding.UTF_8));
        } catch (Exception e) {
            return "get resource fail: " + e.getMessage();
        }
    }

    public static void main(String[] args) throws URISyntaxException {
        SpringApplication.run(OssApplication.class, args);
    }

}
```

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>A prerequisite for the above sample is that you need to have a bucket named “bucketName” on OSS, and you have a file named “fileName” in this bucket. <span contenteditable="true">上述示例的一个先决条件是，您需要在 OSS 上有一个名为“ bucket etName”的 bucket，并且在这个 bucket 中有一个名为“ fileName”的文件</span></td></tr></tbody></table>

### 9.4. Use STS Authentication

### 9.4使用 STS 认证

In addition to AccessKeys, Spring Cloud Alibaba Cloud OSS also supports STS authentication. STS is an authentication method with temporary security tokens, and is usually used for a third party to access its resources temporarily.

除了 AccessKeys，阿里巴巴云计算开放源码软件还支持 STS 认证。STS 是一种带有临时安全令牌的身份验证方法，通常用于第三方临时访问其资源。

For a third party to access resources temporarily, it only needs to complete the following configurations.

对于第三方临时访问资源，它只需要完成以下配置。

```
spring.cloud.alicloud.oss.authorization-mode=STS
spring.cloud.alicloud.oss.endpoint=***.aliyuncs.com
spring.cloud.alicloud.oss.sts.access-key=Your authenticated AK
spring.cloud.alicloud.oss.sts.secret-key=Your authenticated SK
spring.cloud.alicloud.oss.sts.security-token=Your authenticated ST
```

Among which, spring.cloud.alicloud.oss.authorization-mode is the enumeration type. Fill in STS here means that STS authentication is used. For endpoint information, refer to the [OSS Documentation](https://help.aliyun.com/document_detail/31837.html) and fill in the endpoint for your region.

授权模式是其中的枚举类型。在这里填写 STS 意味着使用了 STS 身份验证。有关端点信息，请参考 OSS 文档并填写您所在区域的端点。

Access-key, secret-key and the security-token need to be issued by the authentication side. For more information about STS, refer to [STS Documentation](https://help.aliyun.com/document_detail/31867.html).

认证方需要发出访问密钥、机密密钥和安全令牌。有关 STS 的详细信息，请参阅 STS 文档。

### 9.5. More Configurations for the Client

### 9.5. 客户端的更多配置

In addition to basic configurations, Spring Cloud Alibaba Cloud OSS also supports many other configurations, which are also included in the application.properties file.

除了基本配置之外，阿里巴巴云计算开放源码软件还支持许多其他配置，这些配置也包含在 application.properties 文件中。

Here are some examples.

这里有一些例子。

```
spring.cloud.alicloud.oss.authorization-mode=STS
spring.cloud.alicloud.oss.endpoint=***.aliyuncs.com
spring.cloud.alicloud.oss.sts.access-key=Your authenticated AK
spring.cloud.alicloud.oss.sts.secret-key=Your authenticated SK
spring.cloud.alicloud.oss.sts.security-token=Your authenticated ST

spring.cloud.alicloud.oss.config.connection-timeout=3000
spring.cloud.alicloud.oss.config.max-connections=1000
```

For more configurations, refer to the table at the bottom of [OSSClient Configurations](https://help.aliyun.com/document_detail/32010.html).

有关更多配置，请参考 OSSClientConfiguration 底部的表。

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>In most cases, you need to connect the parameter names with “-” for the parameters in the table of <span contenteditable="true">在大多数情况下，需要用“-”连接</span><a href="https://help.aliyun.com/document_detail/32010.html">OSSClient Configurations<span> OSSClient 配置</span></a> with “-”, and all letters should be in lowercase. For example, ConnectionTimeout should be changed to connection-timeout. <span contenteditable="true">例如，ConnectionTimeout 应该更改为 connect-timeout</span></td></tr></tbody></table>

## 10\. Spring Cloud Alibaba Cloud SchedulerX

## 10\. 阿里巴巴春季云计划

SchedulerX（Distributed job scheduling） is a component of EDAS, an Alibaba Cloud product. Spring Cloud Alibaba Cloud SchedulerX provides distributed job scheduling in conformity with the Spring Cloud specifications. SchedulerX provides timed job scheduling service with high accuracy with seconds, high stability and high availabiliy, and supports multiple job types, such as simple single-server jobs, simple multi-host jobs, script jobs, and grid jobs.

SchedulerX (分布式作业调度)是阿里巴巴云产品 EDAS 的一个组件。阿里巴巴云端调度系统提供符合 Spring Cloud 规范的分布式作业调度。SchedulerX 提供高精度、高时间、高稳定性和高可用性的定时作业调度服务，支持多种作业类型，如简单的单服务器作业、简单的多主机作业、脚本作业和网格作业。

### 10.1. How to Introduce Spring Cloud Alibaba Cloud SchedulerX

### 10.1. 如何介绍阿里巴巴春季云计划

If you want to use SchedulerX in your project, please use the starter with the group ID as `com.alibaba.cloud` and the artifact ID as `spring-cloud-starter-alicloud-schedulerX`.

如果您想在您的项目中使用 SchedulerX，请使用带组 ID com.alibaba.cloud 的 starter，并使用带工件 ID spring-cloud-starter-alicloud-SchederX。

```
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alicloud-schedulerX</artifactId>
</dependency>
```

### 10.2. Start SchedulerX

### 10.2. 启动计划 X

After Spring Cloud Alibaba Cloud SchedulerX Starter is introduced into the client, you only need to complete a few simple configurations and you will be able to initialize the SchedulerX service automatically.

在客户端引入阿里巴巴云计划程序之后，你只需要完成一些简单的配置，就可以自动初始化 SchedulerX 服务了。

The following is a simple example.

下面是一个简单的示例。

```
@SpringBootApplication
public class ScxApplication {

    public static void main(String[] args) {
        SpringApplication.run(ScxApplication.class, args);
    }

}
```

Add the following configurations in the application.properties file.

在 application.properties 文件中添加以下配置。

```
server.port=18033
# cn-test is the test region of SchedulerX
spring.cloud.alicloud.scx.group-id=***
spring.cloud.alicloud.edas.namespace=cn-test
```

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>When you create a group, please select the “test” region. <span contenteditable="true">创建组时，请选择“测试”区域</span></td></tr></tbody></table>

### 10.3. Compile a simple job

### 10.3. 编译一个简单的任务

Simple job is the most commonly used job type. You only need to implement the ScxSimpleJobProcessor interface.

简单作业是最常用的作业类型。

The following is a sample of a simple single-server job.

下面是一个简单的单服务器作业示例。

```
public class SimpleTask implements ScxSimpleJobProcessor {

@Override
public ProcessResult process(ScxSimpleJobContext context) {
System.out.println("-----------Hello world---------------");
ProcessResult processResult = new ProcessResult(true);
return processResult;
}

}
```

### 10.4. Job Scheduling

### 10.4作业计划

Go to the [SchedulerX Jobs](https://edas.console.aliyun.com/#/edasSchedulerXJob?regionNo=cn-test) page, select the “Test” region, and click “Create Job” on the upper-right corner to create a job, as shown below.

转到 SchedulerX Jobs 页面，选择“ Test”区域，然后单击右上角的“ Create Job”创建作业，如下所示。

```
Job Group： Test——***-*-*-****
Job process interface：SimpleTask
Type： Simple Single-Server Job
Quartz Cron Expression： Default Option——0 * * * * ?
Job Description： Empty
Custom Parameters： Empty
```

The job above is a “Simple Single-Server Job”, and specified a Cron expression of "0 \* \* \* \* ?" . This means that the job will be executed once and once only in every minute.

上面的作业是一个“简单的单服务器作业”，并指定了一个 Cron 表达式“0 \* \* \* \* ?”.这意味着作业每分钟只执行一次。

### 10.5. Usage in Production Environment

### 10.5. 在生产环境中的应用

The previous examples shows how to use SchedulerX in the “Test” region, which is mainly used for local testing.

前面的示例演示如何在“ Test”区域中使用 SchedulerX，该区域主要用于本地测试。

At the production level, you need to complete some other configurations in addition to the group-id and namespace as mentioned above. See examples below:

在生产级别，除了上面提到的 group-id 和名称空间之外，还需要完成其他一些配置。见下面的例子:

```
server.port=18033
# cn-test is the test region of SchedulerX
spring.cloud.alicloud.scx.group-id=***
spring.cloud.alicloud.edas.namespace=***
# If your application runs on EDAS, you do not need to configure the following.
spring.cloud.alicloud.access-key=***
spring.cloud.alicloud.secret-key=***
# The following configurations are not mandatory. You can refer to the SchedulerX documentation for details.
spring.cloud.alicloud.scx.domain-name=***
```

The way to get the group-id is the same as described in the previous examples, and you can get the namespace by clicking “Namespaces” in the left-side navigation pane of the EDAS console.

获取 group-id 的方法与前面示例中描述的相同，您可以通过单击 EDAS 控制台左侧导航窗格中的“ Namespaces”来获取名称空间。

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>Group-id must be created within a namespace. <span contenteditable="true">必须在名称空间中创建 Group-id</span></td></tr></tbody></table>

Access-key and secret-key are the AK/SK of your Alibaba Cloud account. If you deploy you applications on EDAS, then you do not need to fill in this information. Otherwise please go to [Security Information](https://usercenter.console.aliyun.com/#/manage/ak) to get your AccessKeys.

访问密钥和机密密钥是你的阿里巴巴云帐户的 AK/SK。如果在 EDAS 上部署应用程序，则不需要填写此信息。否则，请前往安全信息获取您的访问密钥。

## 11\. Spring Cloud Alibaba Cloud SMS

## 11\. 阿里巴巴云彩短信

SMS（Short Message Service）is a messaging service that covers the globe, Alibaba SMS provides convenient, efficient, and intelligent communication capabilities that help businesses quickly contact their customers.

短讯服务是一项覆盖全球的讯息服务，阿里巴巴的短讯服务提供方便、快捷及智能的通讯功能，协助企业迅速与客户联络。

Spring Cloud Alibaba Cloud SMS provide an easier-to-use API for quick access to Alibaba Cloud’s SMS service based on Spring Cloud Alibaba SMS.

Spring Cloud 阿里巴巴云短信提供了一个易于使用的 API，可以快速访问阿里巴巴基于 Spring Cloud 阿里巴巴短信的短信服务。

### 11.1. How to Introduce Spring Cloud Alibaba Cloud SMS

### 11.1. 如何介绍阿里巴巴云端短讯

If you want to use SMS in your project, please use the starter with the group ID as `com.alibaba.cloud` and the artifact ID as `spring-cloud-starter-alicloud-sms`.

如果您想在您的项目中使用 SMS，请使用具有组 ID com.alibaba.cloud 的 starter，以及具有 spring-cloud-starter-alicloud-SMS 的工件 ID。

```
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alicloud-sms</artifactId>
</dependency>
```

### 11.2. How to use SMS API

### 11.2. 如何使用短信 API

#### 11.2.1. Configure SMS

#### 11.2.1. 配置短信

Before you start to use Spring Cloud Alibaba Cloud SMS, please add the following configurations in application.properties.

在开始使用阿里巴巴云服务之前，请在 application.properties 中添加以下配置。

```
spring.cloud.alicloud.access-key=AK
spring.cloud.alicloud.secret-key=SK
```

access-key and secret-key is the AK/SK of your Alibaba Cloud account. If you don’t have one, please register an account first, and log on to [Alibaba Cloud AK/SK Management](https://usercenter.console.aliyun.com/#/manage/ak) to get your AccessKey ID and Access Key Secret . If you haven’t create the AccessKeys, click “Create AccessKey” to create one.

Access-key 和 secret-key 是阿里巴巴云帐户的 AK/SK。如果您没有，请先注册一个帐户，登录阿里巴巴云 AK/SK 管理获得您的访问密钥 ID 和访问密钥机密。如果尚未创建 AccessKey，请单击“创建 AccessKey”以创建一个。

#### 11.2.2. Introduce SMS API

#### 11.2.2. 介绍短讯应用程式介面

The SMS API in Spring Cloud Alicloud SMS is based on Alibaba Cloud SMS SDK. It has a single SMS sending, multiple SMS bulk sending, SMS query, SMS message (SMS receipt message and Upstream SMS message) class operation API.

Spring Cloud 中的短信接口 Alicloud 短信基于阿里巴巴云短信 SDK。它具有单短信发送、多短信批量发送、短信查询、短信息(SMS 接收消息和上游短信)类操作 API。

The following is a simple example of how to use SMS api to send short message:

以下是如何使用 SMSapi 发送短信的一个简单示例:

```
@SpringBootApplication
public class SmsApplication {

    @Autowired
    private ISmsService smsService;

    @RequestMapping("/batch-sms-send.do")
    public SendBatchSmsResponse batchsendCheckCode(
            @RequestParam(name = "code") String code) {

        SendSmsRequest request = new SendSmsRequest();
        // Required:the mobile number
        request.setPhoneNumbers("152******");
        // Required:SMS-SignName-could be found in sms console
        request.setSignName("******");
        // Required:Template-could be found in sms console
        request.setTemplateCode("******");
        // Required:The param of sms template.For exmaple, if the template is "Hello,your verification code is ${code}". The param should be like following value
        request.setTemplateParam("{\"code\":\"" + code + "\"}");
        SendSmsResponse sendSmsResponse ;
        try {
            sendSmsResponse = smsService.sendSmsRequest(request);
        }
        catch (ClientException e) {
            e.printStackTrace();
            sendSmsResponse = new SendSmsResponse();
        }
        return sendSmsResponse ;
    }

    public static void main(String[] args) throws URISyntaxException {
        SpringApplication.run(SmsApplication.class, args);
    }

}
```

For more information about SMS , please refer to the SMS official [SMS](https://help.aliyun.com/document_detail/55284.html?spm=a2c4g.11186623.6.568.715e4f30ZiVkbI) (SendSms)---JAVA\] docs .

有关 SMS 的详细信息，请参阅 SMS 官方 SMS (SendSms)——-JAVA \]文档。

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>Due to an issue with the earlier SMS sdk version, if the text message fails to be sent, please delete the line of code that contains the explicit MethodType as POST. If you still have problems, please contact us as soon as possible. <span contenteditable="true">由于早期 SMS sdk 版本的问题，如果未能发送文本消息，请删除包含显式 MethodType 作为 POST 的代码行。如果您还有问题，请尽快与我们联系</span></td></tr></tbody></table>

### 11.3. The Advanced Features of SMS Api

### 11.3. 短信 Api 的高级功能

In order to reduce the cost of learning, the API interface of the Spring Cloud Alicloud SMS package is kept as consistent as the API and Example provided by the official website.

为了降低学习的成本，Spring Cloud Alicloud SMS 包的 API 接口保持与官方网站提供的 API 和示例一致。

-   Batch SMS sending
    
    批量短信发送
    

Refer to the following example to quickly develop a feature with bulk SMS sending. Add the following code in the Controller or create a new Controller:

请参考下面的示例来快速开发一个具有批量短信发送功能的特性。在 Controller 中添加以下代码或创建一个新 Controller:

```
@RequestMapping("/batch-sms-send.do")
public SendBatchSmsResponse batchsendCheckCode(
        @RequestParam(name = "code") String code) {
    SendBatchSmsRequest request = new SendBatchSmsRequest();
    request.setMethod(MethodType.GET);
    request.setPhoneNumberJson("[\"177********\",\"130********\"]");
    request.setSignNameJson("[\"*******\",\"*******\"]");
    request.setTemplateCode("******");
    request.setTemplateParamJson(
            "[{\"code\":\"" + code + "\"},{\"code\":\"" + code + "\"}]");
    SendBatchSmsResponse sendSmsResponse ;
    try {
        sendSmsResponse = smsService
                .sendSmsBatchRequest(request);
        return sendSmsResponse;
    }
    catch (ClientException e) {
        e.printStackTrace();
        sendSmsResponse =  new SendBatchSmsResponse();
    }
    return sendSmsResponse ;
}
```

<table><tbody><tr><td><p>Note<span> 注意</span></p></td><td>The MethodType of the request is set to GET, which is somewhat different from the example given by the official website. This is because the inconsistent version of the dependent Alibaba Cloud POP API version causes incompatibility issues, set to GET. <span contenteditable="true">请求的 MethodType 被设置为 GET，这与官方网站给出的示例有些不同。这是因为相关的阿里巴巴 Cloud POP API 版本的不一致版本会导致不兼容性问题，设置为 GET</span></td></tr></tbody></table>

-   SMS Query
    
    短讯查询
    

Refer to the following example to quickly develop a history of sending SMS messages based on a specified number. Add the following code in the Controller or create a new Controller:

请参考下面的示例，以快速开发基于指定数字发送 SMS 消息的历史。在 Controller 中添加以下代码或创建一个新 Controller:

```
@RequestMapping("/query.do")
public QuerySendDetailsResponse querySendDetailsResponse(
        @RequestParam(name = "tel") String telephone) {
    QuerySendDetailsRequest request = new QuerySendDetailsRequest();
    request.setPhoneNumber(telephone);
    request.setSendDate("20190103");
    request.setPageSize(10L);
    request.setCurrentPage(1L);
    try {
        QuerySendDetailsResponse response = smsService.querySendDetails(request);
        return response;
    }
    catch (ClientException e) {
        e.printStackTrace();
    }

    return new QuerySendDetailsResponse();
}
```

More parameter descriptions can be found at [reference here](https://help.aliyun.com/document_detail/55289.html?spm=a2c4g.11186623.6.569.4f852c78mugEfx)

更多的参数描述可以在这里找到参考

-   SMS receipt message
    
    短信接收信息
    

By subscribing to the SmsReport SMS status report, you can know the status of each SMS message and whether it knows the status and related information of the terminal user. These efforts have been encapsulated internally by Spring Cloud AliCloud SMS. You only need to complete the following two steps.

通过订阅 SmsReport 短讯状态报告，您可以知道每条短讯的状态，以及它是否知道终端用户的状态和相关信息。这些努力已经被 SpringCloudAliCloud 短信封装在内部。您只需完成以下两个步骤。

1、Configure the queue name for SmsReport in the `application.properties` configuration file (which can also be application.yaml).

1、在 application.properties 配置文件(也可以是 application.yaml)中为 SmsReport 配置队列名称。

application.properties 应用性能

```
spring.cloud.alicloud.sms.report-queue-name=Alicom-Queue-********-SmsReport
```

2、Implement the SmsReportMessageListener interface and initialize a Spring Bean.

2、实现 SmsReportMessageListener 接口并初始化一个 Spring Bean。

```
@Component
public class SmsReportMessageListener
implements SmsReportMessageListener {

@Override
public boolean dealMessage(Message message) {
    //do something
System.err.println(this.getClass().getName() + "; " + message.toString());
return true;
}
}
```

More message body format for Message can be [reference here](https://help.aliyun.com/document_detail/55496.html?spm=a2c4g.11186623.6.570.7f792c78rOiWXO).

此处可参考更多讯息正文格式。

-   Upstream SMS message
    
    上游短信
    

By subscribing to the SmsUp upstream SMS message, you can know the content of the end user replying to the SMS. These efforts have also been packaged by Spring Cloud AliCloud SMS. You only need to complete the following two steps.

通过订阅 SmsUp 上游短讯，您可以了解最终用户回复短讯的内容。这些努力也被 SpringCloudAliCloudSMS 打包。您只需完成以下两个步骤。

1、Configure the queue name for SmsReport in the `application.properties` configuration file (which can also be application.yaml).

1、在 application.properties 配置文件(也可以是 application.yaml)中为 SmsReport 配置队列名称。

application.properties 应用性能

```
spring.cloud.alicloud.sms.up-queue-name=Alicom-Queue-********-SmsUp
```

2、Implement the SmsUpMessageListener interface and initialize a Spring Bean.

2、实现 SmsUpMessageListener 接口并初始化一个 Spring Bean。

```
@Component
public class SmsUpMessageListener
implements org.springframework.cloud.alicloud.sms.SmsUpMessageListener {

@Override
public boolean dealMessage(Message message) {
    //do something
System.err.println(this.getClass().getName() + "; " + message.toString());
return true;
}
}
```

More message body format for Message can be [reference here](https://help.aliyun.com/document_detail/55496.html?spm=a2c4g.11186623.6.570.7f792c78rOiWXO).

此处可参考更多讯息正文格式。