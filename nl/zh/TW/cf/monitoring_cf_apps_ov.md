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

# 監視 Cloud Foundry 上執行的應用程式
 {:#monitoring_bluemix_apps}

若要將度量值傳送至 {{site.data.keyword.monitoringshort}} 服務，Cloud Foundry (CF) 應用程式可以使用 {{site.data.keyword.monitoringshort}} REST API，直接從應用程式傳送度量值。
{:shortdesc}

當您使用 Cloud Foundry 基礎架構在 {{site.data.keyword.Bluemix_notm}} 上執行應用程式時，您可能會想要監視應用程式的性能狀態、資源用量及資料流量度量值。運用此效能資訊，您接著可以進行決策，或相應地採取動作。



在 Cloud Foundry 上執行的應用程式無法執行 collectd。而是必須使用 lumberjack 用戶端或 RESTful API，從應用程式本身傳送資料。 

##其他要從 CF 應用程式傳送資料的方式 [@lindj 已建立 collectd 及 statsd 建置套件] (https://github.com/jimlindeman/python-buildpack-collectd)

這是 Python 建置套件版本的複本，而此版本已安裝 collectd，以使用 statsd 輸入外掛程式及 Bluemix 的多方承租戶 Graphite 輸出外掛程式，在 localhost:8125 上進行接聽 

您可以使用建置套件，在 Bluemix 中部署 CF 應用程式。建置套件提供應用程式的架構及運行環境支援。您可以安裝並配置 collectd，以使用 statsd 輸入外掛程式及 Bluemix 的多方承租戶 Graphite 輸出外掛程式，在 localhost:8125 上進行接聽 
