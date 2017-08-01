---

copyright:
  years: 2017

lastupdated: "2017-06-19"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# 监视 Cloud Foundry 上运行的应用程序
 {:#monitoring_bluemix_apps}

要将度量值发送到 {{site.data.keyword.monitoringshort}} 服务，Cloud Foundry (CF) 应用程序可以使用 {{site.data.keyword.monitoringshort}} REST API 直接从该应用程序发送度量值。
{:shortdesc}

使用 Cloud Foundry 基础架构在 {{site.data.keyword.Bluemix_notm}} 上运行应用程序时，您可能希望监视应用程序的运行状态、资源使用情况和流量度量值。通过这些性能信息，您可以相应地进行决策或执行操作。



在 Cloud Foundry 上运行的应用程序不能运行 collectd。必须改为使用 lumberjack 客户机或 RESTful API 从应用程序本身发送数据。 

##从 CF 应用程序发送数据的其他方法 [@lindj 已创建 collectd 和 statsd buildpack](https://github.com/jimlindeman/python-buildpack-collectd)

这是 python-buildpack 发行版的克隆，安装了 collectd，因而能够通过 statsd 输入插件和 Bluemix 的多租户 graphite 输出插件来侦听 localhost:8125。 

可以使用 buildpack 在 Bluemix 中部署 CF 应用程序。buildpack 为应用程序提供框架和运行时支持。可以安装并配置 collectd 以通过 statsd 输入插件和 Bluemix 的多租户 graphite 输出插件来侦听 localhost:8125。 
