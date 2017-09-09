# ESB技术分析文档

<a name="index"></a>
## 目录

<!-- MarkdownTOC -->

- [架构 \(Architecture\)](#%E6%9E%B6%E6%9E%84-architecture)
    - [消息架构 \(Messaging Architecture\)](#%E6%B6%88%E6%81%AF%E6%9E%B6%E6%9E%84-messaging-architecture)
    - [组件架构 \(Component Architecture\)](#%E7%BB%84%E4%BB%B6%E6%9E%B6%E6%9E%84-component-architecture)
        - [运输 \(Transport\)](#%E8%BF%90%E8%BE%93-transport)
        - [终端 \(EndPoint\)](#%E7%BB%88%E7%AB%AF-endpoint)
        - [调解者 \(Mediator\)](#%E8%B0%83%E8%A7%A3%E8%80%85-mediator)
        - [序列 \(Sequence\)](#%E5%BA%8F%E5%88%97-sequence)
        - [代理 \(Proxy\)](#%E4%BB%A3%E7%90%86-proxy)
        - [RESTful接口 \(API\)](#restful%E6%8E%A5%E5%8F%A3-api)
        - [主题 \(Topics\)](#%E4%B8%BB%E9%A2%98-topics)
        - [注册 \(Registry\)](#%E6%B3%A8%E5%86%8C-registry)
        - [发布 \(Publish\)](#%E5%8F%91%E5%B8%83-publish)
        - [服务质量 \(QoS Quality of Service\)](#%E6%9C%8D%E5%8A%A1%E8%B4%A8%E9%87%8F-qos-quality-of-service)
        - [监控 \(Monitor\)](#%E7%9B%91%E6%8E%A7-monitor)
        - [分析 \(Analytics\)](#%E5%88%86%E6%9E%90-analytics)
        - [部署 \(Deployment\)](#%E9%83%A8%E7%BD%B2-deployment)
        - [消息协商器 \(Message Broker\)](#%E6%B6%88%E6%81%AF%E5%8D%8F%E5%95%86%E5%99%A8-message-broker)

<!-- /MarkdownTOC -->

<a name="%E6%9E%B6%E6%9E%84-architecture"></a>
## 架构 (Architecture)

<a name="%E6%B6%88%E6%81%AF%E6%9E%B6%E6%9E%84-messaging-architecture"></a>
### 消息架构 (Messaging Architecture)
![Arch](./MsgArch.png "Arch") 

ESB的消息架构如上图所示。具体过程如下：

1. 应用程序发送一个消息到ESB。
- 运输组件装载消息。
- 运输组件通过消息管道发送消息，消息管道负责消息的安全性等和服务相关的内容。这里的管道实质上是Axis2的入流和出流。ESB可以运行在两种模式下：消息调解和代理模式。
- 消息传输和路由可以看作一个组件。在WSO2 ESB中，这部分是解调框架。
- 消息根据目的地进入不同的管道中。
- 运输层处理传输协议相关的问题。 

[返回](#index)
<a name="%E7%BB%84%E4%BB%B6%E6%9E%B6%E6%9E%84-component-architecture"></a>
### 组件架构 (Component Architecture)
![CompArch](./CompArch.jpg "CompArch")
ESB的组件架构如上图所示。其中注册、UI管理界面、Carbon平台均是WSO2的产品，其余部分来自于开源软件Apache Synapse。

[返回](#index)
<a name="%E8%BF%90%E8%BE%93-transport"></a>
#### 运输 (Transport)
运输组件负责把运输特定格式的消息。WSO2 ESB支持很多常用的格式，如HTTP/s,JMS,VSF,FIX等。可以通过Axis2运输框架定制新的运输组件，并且加入到ESB中。

运输组件包括两部分：

- 消息创建者
    + 该部分组件根据消息内容进行识别，并转化成为通用的XML格式。每一种类型的消息均有各自的创建者。WSO2 ESB有基于文本和二进制内容的消息创建者。
- 消息范化者
    + 消息创建者的反面。范化者把消息根据内容类型转化为原有的格式。

这两种组件均可使用Axis2的框架进行定制。

[返回](#index)
<a name="%E7%BB%88%E7%AB%AF-endpoint"></a>
#### 终端 (EndPoint)
一个端定义了用来处理一个消息的外部服务。终端的类型有以下几种：

- 基于地址的终端
- 基于WSDL的终端
- 实现负载均衡的终端

同时，端的定义可以和传输类型分开，因此，同一个端可以使用不同种的传输类型。当你定义一个调解器或者一个代理服务的时候，你需要指定传输类型和目的端。

端管理的界面如下：
![EndPoints](./Endpoints.png "EndPoints")

[返回](#index)
<a name="%E8%B0%83%E8%A7%A3%E8%80%85-mediator"></a>
#### 调解者 (Mediator)
调解者是独立的处理单元，每个调解者均有自己独特的功能，比如发消息或者过滤消息，记录日志等等。WSO2 ESB有一个较全的调解者的库，提供了很多常用的企业集成模式。使用JAVA、脚本语言、Spring可以方便的实现定制化的调解者。

[返回](#index)
<a name="%E5%BA%8F%E5%88%97-sequence"></a>
#### 序列 (Sequence)
序列是调解者的配置，在序列中你可以流式的处理一个消息，调用不同的解调者来完成相应的功能。

序列的管理界面如下：
![Sequence](./Sequence.png "Sequence")

![EditSequence](./EditSequence.png "EditSequence")

[返回](#index)
<a name="%E4%BB%A3%E7%90%86-proxy"></a>
#### 代理 (Proxy)
代理是虚拟服务，代理接收消息，然后做一些处理，再有选择性的转发到指定的终端上。这种方法如同装饰模式一样，可以在不修改原有服务的情况下，对服务增加新的功能。

[返回](#index)
<a name="restful%E6%8E%A5%E5%8F%A3-api"></a>
#### RESTful接口 (API)
WSO2 ESB中的API就像一个部署在ESB中的web应用一样，每个API被用户定义的URL上下文锚定，如同部署在servlet容器中的一样。API 仅仅处理完全符合的URL。每个API可以定义一个或者多个资源。这种方法可以使ESB直接处理REST请求。

API的管理界面如下：
![API](./API.png "API")

API的编辑界面如下：
![EditAPI](./EditAPI.png "EditAPI")

[返回](#index)
<a name="%E4%B8%BB%E9%A2%98-topics"></a>
#### 主题 (Topics)
主题可以让服务监听事件，通过事件触发服务。

[返回](#index)
<a name="%E6%B3%A8%E5%86%8C-registry"></a>
#### 注册 (Registry)
WSO2 ESB 可以注册到自带的仓库中，也可以使用远程仓库。WSO2 ESB提供了一个管理界面，可以使用图形化的方式进行Endpoint、Sequence、Proxy、API的注册，也可以使用已经写好的xml文件进行注册。

[返回](#index)
<a name="%E5%8F%91%E5%B8%83-publish"></a>
#### 发布 (Publish)
WSO2 ESB中的服务包含Proxy、API两种主要类型，定义好Proxy和API之后，服务的发布就完成了。WSO2 ESB的UI管理系统中可以暂停、继续、删除服务，也可以对服务进行跟踪。

[返回](#index)
<a name="%E6%9C%8D%E5%8A%A1%E8%B4%A8%E9%87%8F-qos-quality-of-service"></a>
#### 服务质量 (QoS Quality of Service)
服务质量包含了认证、流量管控、缓存等功能。

[返回](#index)
<a name="%E7%9B%91%E6%8E%A7-monitor"></a>
#### 监控 (Monitor)
WSO2 ESB中可以对系统记录的日志根据级别进行筛选，也可以通过关键字对日志记录搜索。日志中记录的信息可以通过在Sequence中的配置进行修改。
![Log](./Log.png "Log")

WSO2 ESB中还可以看到系统运行的整体情况，如下图所示：
![SystemStatistics](./SysStats.png "SystemStatistics")
[返回](#index)
<a name="%E5%88%86%E6%9E%90-analytics"></a>
#### 分析 (Analytics)
需要启动Analytics的服务。

![Analytics](./Analytics.png "Analytics")
[返回](#index)
<a name="%E9%83%A8%E7%BD%B2-deployment"></a>
#### 部署 (Deployment)
根据官方文档，通过对配置文件的修改，可以实现负载均衡和故障转移的部署。

[返回](#index)
<a name="%E6%B6%88%E6%81%AF%E5%8D%8F%E5%95%86%E5%99%A8-message-broker"></a>
#### 消息协商器 (Message Broker)
需要启动broker的服务。

[返回](#index)