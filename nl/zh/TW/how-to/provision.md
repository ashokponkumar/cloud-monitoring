---

copyright:
  years: 2017

lastupdated: "2017-07-10"

---


{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# 佈建監視服務
{: #provision}

您可以從 {{site.data.keyword.Bluemix}} 使用者介面或從指令行，佈建 {{site.data.keyword.monitoringshort}} 服務。
{:shortdesc}


## 從使用者介面佈建
{: #ui}

完成下列步驟，以在 {{site.data.keyword.Bluemix_notm}} 中佈建 {{site.data.keyword.monitoringshort}} 服務的實例：

1. 登入 {{site.data.keyword.Bluemix_notm}} 帳戶。

    {{site.data.keyword.Bluemix_notm}} 儀表板可以在下列網址找到：[http://bluemix.net ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://bluemix.net "外部鏈結圖示"){:new_window}。
    
	使用您的使用者 ID 和密碼登入之後，{{site.data.keyword.Bluemix_notm}} 使用者介面即會開啟。

2. 按一下**型錄**。這時會開啟 {{site.data.keyword.Bluemix_notm}} 上的可用服務清單。

3. 選取 **DevOps** 種類，以過濾顯示的服務清單。

4. 按一下 **Monitoring** 磚。

5. 選取服務方案。若要收集和存取最多 45 天的度量值，以及定義警示規則（包括具有萬用字元的規則），請選取**進階**方案。依預設，會設定**精簡**方案，它讓您能夠在您要佈建服務的空間中收集平台度量值，並且對於那些度量值有 15 天的保留期間。 

    如需服務方案的相關資訊，請參閱[服務方案](/docs/services/cloud-monitoring/monitoring_ov.html#plans)。
	
6. 按一下**建立**以在您登入的 {{site.data.keyword.Bluemix_notm}} 空間中佈建 {{site.data.keyword.monitoringshort}} 服務。
  
 

## 從 CLI 佈建
{: #cli}

完成下列步驟，以透過指令行在 {{site.data.keyword.Bluemix_notm}} 中佈建 {{site.data.keyword.monitoringshort}} 服務的實例：

1. [必要條件] 安裝 {{site.data.keyword.Bluemix_notm}} CLI。

   如需相關資訊，請參閱[安裝 {{site.data.keyword.Bluemix_notm}} CLI](/docs/services/cloud-monitoring/qa/cli_qa.html#cli_qa)。
   
   如果已安裝 CLI，請繼續進行下一步。
    
2. 登入 {{site.data.keyword.Bluemix_notm}} 地區、組織及空間。執行下列指令：

    ```
    cf login -a [https://api.<span class="keyword" data-hd-keyref="DomainName">DomainName</span>](https://api.{DomainName})
    ```
    {: codeblock}

    遵循指示。輸入您的 {{site.data.keyword.Bluemix_notm}} 認證，選取組織及空間。
	
3. 執行 `cf create-service` 指令以佈建實例。

	```
	cf create-service service_name service_plan service_instance_name
	```
	{: codeblock}
	
	其中
	
	* service_name 是服務的名稱，亦即 **Monitoring**。
	* service_plan 是服務方案名稱。若要收集和存取最多 45 天的度量值，以及定義警示規則（包括具有萬用字元的規則），請選取**進階**方案。依預設，會設定**精簡**方案，它讓您能夠在您要佈建服務的空間中收集平台度量值，並且對於那些度量值有 15 天的保留期間。如需方案清單，請參閱 [{{site.data.keyword.monitoringshort}} 服務方案](/docs/services/cloud-monitoring/monitoring_ov.html#plan)。
	* service_instance_name 是您想要用於所建立之新服務實例的名稱。
	
	如需 cf 指令的相關資訊，請參閱 [cf create-service](/docs/cli/reference/cfcommands/index.html#cf_create-service)。

	例如，若要建立具有進階方案的 {{site.data.keyword.monitoringshort}} 服務實例，請執行下列指令：
	
	```
	cf create-service Monitoring premium my_monitoring_svc
	```
	{: codeblock}
	
4. 驗證服務已順利建立。執行下列指令：

	```	
	cf services
	```
	{: codeblock}
	
	執行指令的輸出如下所示：
	
	```
    Getting services in org MyOrg / space MySpace as xxx@yyy.com...
    OK
    
    name                           service                  plan                   bound apps              last operation
    my_monitoring_svc              Monitoring               premium                                        create succeeded
	```
	{: screen}

	



