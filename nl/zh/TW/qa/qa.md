---

copyright:
  years: 2017

lastupdated: "2017-07-14"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# 常見問題與解答
{: #qa}

以下是關於 {{site.data.keyword.monitoringshort}} 服務的常見問題與解答。
{:shortdesc}

* [為何我看不到最近未傳送度量值之度量值的舊資料？](#qa3)


## 為何我看不到最近未傳送度量值之度量值的舊資料？
{: #qa3}

{{site.data.keyword.monitoringshort}} 會刪除本質上似乎是暫時性的度量值路徑的所有資料，方法為識別過去 7 天內未寫入的度量值。 

例如：

* 當刪除容器時，與該容器相關聯的度量值會存在 7 天，之後即會刪除這些度量值。
* 如果您有一個稱為 `<space_id>.test.statsd.gauge-hello` 的 statsd 量規，而且您有一個星期未寫入其中，則度量值將被識別為暫時性，而且該度量值與其所有歷程資訊將會被刪除。 

