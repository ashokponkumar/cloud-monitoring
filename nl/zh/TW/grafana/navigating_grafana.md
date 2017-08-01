---

copyright:
  years: 2017

lastupdated: "2017-05-25"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# 導覽至 Grafana 儀表板
{:#navigating_grafana}

在 {{site.data.keyword.Bluemix}} 中，您可以使用 Grafana（一種開放程式碼分析與視覺化平台），在各種圖形（例如圖表和表格）中監視、搜尋、分析及視覺化您的度量值。使用 Grafana 來執行進階分析作業。
{:shortdesc}

您可以使用下列任何方式來啟動 Grafana：

* 從 {{site.data.keyword.Bluemix_notm}}

    在 Grafana 中，您可以在特定 Docker 容器的環境定義下，啟動至該特定容器的度量值。此特性只適用於 {{site.data.keyword.Bluemix_notm}} 所管理雲端基礎架構中部署的容器。 
    
    如需相關資訊，請參閱[從 {{site.data.keyword.Bluemix_notm}} 儀表板導覽至 Grafana 儀表板](navigating_grafana.html#launch_grafana_from_bluemix)。

* 從直接瀏覽器鏈結

    您可以啟動 Grafana，以讓您看到的資料能從所提供的 {{site.data.keyword.Bluemix_notm}} 空間內的服務聚集日誌。
    
    如需相關資訊，請參閱[從 Web 瀏覽器導覽至 Grafana 儀表板](navigating_grafana.html#launch_grafana_from_browser)。
    
如需 Grafana 的相關資訊，請參閱 [Grafana 使用手冊 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.grafana.org/guides/getting_started/ "外部鏈結圖示"){: new_window}。


##  從 Bluemix 儀表板導覽至 Grafana 儀表板
{: #launch_grafana_from_bluemix}

**附註：**此特性只適用於 {{site.data.keyword.Bluemix_notm}} 所管理雲端基礎架構中部署的容器。 

用來過濾 Grafana 顯示資料的查詢，會擷取 Kibana 啟動所在之 {{site.data.keyword.Bluemix_notm}} 容器的資料。 

若要在 Grafana 中查看 Docker 容器的度量值，請完成下列步驟：

1. 登入 {{site.data.keyword.Bluemix_notm}}，然後從 {{site.data.keyword.Bluemix_notm}} 儀表板按一下容器。 
    
2. 在導覽列中，按一下**監視及日誌**。即會開啟「監視」標籤。 
    
3. 按一下**進階視圖**。即會開啟 **Grafana** 儀表板。


##  從 Web 瀏覽器導覽至 Grafana 儀表板
{: #launch_grafana_from_browser}

用來過濾 Grafana 顯示資料的查詢，會擷取 {{site.data.keyword.Bluemix_notm}} 組織中某個空間的資料。Grafana 顯示的度量值資訊，包括部署在 {{site.data.keyword.Bluemix_notm}} 組織中您登入空間內之所有資源的記錄。

完成下列步驟，以從瀏覽器啟動 Grafana：

1. 開啟 [https://metrics.ng.bluemix.net](https://metrics.ng.bluemix.net) 以登入 Grafana 使用者介面。
2. 選取 **Grafana**。
     

