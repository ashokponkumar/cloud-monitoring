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


# 使用 Grafana 時常見的問題與解答
{: #grafana_qa}

以下是關於搭配使用 Grafana 與 {{site.data.keyword.monitoringshort}} 服務的常見問題與解答。
{:shortdesc}

* [登入監視服務 Web 使用者介面時顯示 404](/docs/services/cloud-monitoring/qa/grafana_qa.html#404)
* [我只上傳了 Grafana 儀表板的 json 資料，為何捲軸消失？](/docs/services/cloud-monitoring/qa/grafana_qa.html#2)


## 使用 UAA 鑑別模型登入監視服務 Web 使用者介面時顯示 404
{: #404}

當您嘗試登入 {{site.data.keyword.monitoringshort}} 服務 Web 使用者介面 (Grafana) 時，如果顯示 404，請確認該空間存在，並確認您具有其存取權。

使用 [UAA 鑑別模型](/docs/services/cloud-monitoring/security/auth_uaa.html#auth_uaa)來存取 {{site.data.keyword.monitoringshort}} 服務時，您必須使用您用來登入 {{site.data.keyword.Bluemix_notm}} 主控台的相同使用者 ID 及密碼。 

若要驗證您有權可存取要登入的帳戶、組織及空間，請登入 {{site.data.keyword.Bluemix_notm}} 主控台並切換至空間。 

您也可以使用指令行，來驗證您有權可存取該空間。請執行下列指令，以登入 {{site.data.keyword.Bluemix_notm}} 地區、組織及空間：

```
bx login -a https://api.ng.bluemix.net
```
{: codeblock}

遵循指示。輸入您的 {{site.data.keyword.Bluemix_notm}} 認證，選取組織及空間。


## 我只上傳了 Grafana 儀表板的 JSON 資料，為何捲軸會消失？
{: #2}

Grafana 中有一個錯誤，會在您匯入儀表板之後隱藏捲軸。 

若要看到捲軸，請在匯入儀表板之後儲存儀表板。 








